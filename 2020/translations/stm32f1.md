# Исключительный провал – Взлом защиты от чтения STM32F1

Прошивка микроконтроллеров зачастую содержит ценные данные, такие как интеллектуальная собственность, а иногда и ключи шифрования \(криптографические материалы\). Для их защиты в большинстве микроконтроллеров используют механизмы защиты от чтения. Задача такого механизма – ограничить чтение данных из внутренней флэш памяти злоумышленником, обладающим физическим доступом к устройству. Однако, как исследователи безопасности, так и любители, регулярно показывают, что эти механизмы можно обойти. В данном исследовании мы рассмотрим защиту от чтения флеш памяти в семействе микроконтроллеров STM32F1 STMicroelectronics. Мы обсудим новую уязвимость, эксплуатация которой является первым неинвазивным способом обхода защиты. Причиной этой уязвимости является недостаточное ограничение доступа: чтение данных из флэш-памяти через отладочный интерфейс блокируется, но система прерываний ЦПУ имеет доступ к этим данным по шине ICode. Мы подробно рассмотрим почему и как эта уязвимость раскрывает значительную часть внутренней памяти, тем самым компрометируя устройство.

**\[**[**Видео**](https://blog.zapb.de//files/stm32f1-exceptional-failure/firmware_extraction_1080p.mp4)**\]**

## **Введение**

Для защиты интеллектуальной собственности и прочих ценных данных, таких как ключи шифрования, учетные данные и т.д. – защита внутренней памяти становится задачей первостепенной важности. В том случае, если атакующий получает доступ к прошивке, он может клонировать устройство, изменить его функционал или извлечь учётные данные. В связи с этим защита микроконтроллера играет серьезную роль в безопасности встраиваемых систем. Это касается не только устройств, требующих повышенный уровень защиты, но и коммерческих микроконтроллеров.

Отключение отладочного интерфейса является частым способом противодействия злоумышленникам, однако его реализация отличается у разных микроконтроллеров. К примеру в [семействе STM32F0](https://www.st.com/en/microcontrollers-microprocessors/stm32f0-series.html) этот интерфейс можно полностью отключить. В то же время, семейство STM32F1 такой возможностью не располагает, предлагая несколько альтернатив. Одной из них является защита флэш-памяти от чтения \(read-out protection, RDP\). Этот механизм блокирует доступ к любым данным во флэш-памяти через отладочный интерфейс. Это значит, что атакующий может подключить отладчик к микроконтроллеру, но прочитать из него данные он не сможет.  
****  
Однако исследования показали, что у некоторых микроконтроллеров этот механизм имеет изъяны. Например, в своем исследовании ["Shedding too much Light on a Microcontroller's Firmware Protection”](https://www.usenix.org/system/files/conference/woot17/woot17-paper-obermaier.pdf), ****Йоханнес Обермайер и Штефан Тачнер описали атаку на семейство STM32F0. Некоторые исследователи предположили, что эта уязвимость может также затрагивать другие семейства, такие как STM32F1. [Однако один из авторов это опроверг](https://www.mikrocontroller.net/topic/442881#5274755). До сего момента механизм защиты семейства STM32F1 считался безопасным, поскольку не было свидетельств, что его можно обойти. В этой статье мы рассматриваем уязвимость \([CVE-2020-8004](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8004)\), которая приводит к первой неинвазивной атаке на систему защиты флэш-памяти STM32F1**.**

## **Обнаружение уязвимости**

Семейство STM32F1 не предоставляет механизма перманентного отключения отладочного интерфейса. По этой причине атакующий с физическим доступом всегда имеет возможность получить контроль над ним. Однако встроенный механизм защиты от чтения пресекает любые запросы к данным при подключенном отладчике.  
Для того, чтобы изучить систему защиты мы использовали отладочную плату[ STM32 Nucleo-64](https://www.st.com/en/evaluation-tools/nucleo-f103rb.html) с установленным на ней микроконтроллером STM32F103RB. Защита от чтения на нём была включена и запрещала доступ к данным флэш-памяти по отладочному интерфейсу. Через интерфейс SWD этот микроконтроллер был подключён к отладчику SEGGER J-Link, как изображено на рисунке 1.

![&#x420;&#x438;&#x441;&#x443;&#x43D;&#x43E;&#x43A; 1. &#x41E;&#x442;&#x43B;&#x430;&#x434;&#x43E;&#x447;&#x43D;&#x430;&#x44F; &#x43F;&#x43B;&#x430;&#x442;&#x430; STM32 Nucleo-64 &#x438; &#x43F;&#x43E;&#x434;&#x43A;&#x43B;&#x44E;&#x447;&#x435;&#x43D;&#x43D;&#x44B;&#x439; &#x43A; &#x43D;&#x435;&#x439; &#x43E;&#x442;&#x43B;&#x430;&#x434;&#x447;&#x438;&#x43A; SEGGER J-Link](https://lh5.googleusercontent.com/WFspmRCT7mu7uxTdEO7c_fu43-pVNALx8DyhEjmVmvNviH_aOhsfemyA_VnYf-h5NJ3iNxh1cZ6mMSypCJRzX_FlJujqPa2gpkBT6IG_EmDDrKTrIDLR596raAPsiGFEFHV5hIc)

Мы начали наше исследовать с установки соединения с целевым микроконтроллером. Для этого мы запустили [OpenOCD ](http://openocd.org/)следующей командой:

`openocd -f interface/jlink.cfg -c "transport select swd" -f target/stm32f1x.cfg`

Затем мы открыли Telnet-сессию через OpenOCD, что дало контроль над микроконтроллером. Наконец, мы перезагрузили микроконтроллер командой перезагрузки с остановом, получив такой вывод:

`target halted due to debug-request, current mode: Thread`

`xPSR: 0x01000000 pc: 0x08000268 msp: 0x20005000`

На первый взгляд в нём нет ничего особенного. Однако, если внимательно прочитать вторую строчку, одно из значений должно вас заинтересовать: счетчик инструкций \(program counter, PC\) имеет значение 0x08000268, что является корректным адресом, расположенным во флэш-памяти. Это очень важно, поскольку перезагрузка является особым видом исключения. При обработки каждого исключения, в процессор загружается соответствующий адрес обрабатывающей функции из таблицы векторов в PC. Эта процедура иногда называется запросом вектора \(vector fetch\). После перезагрузки устройства, таблица векторов располагается во флэш-памяти. Это наблюдение показывает, что процессор получил вектор обработчика перезагрузки из флэш-памяти при условии того, что защита от считывания включена.  
Почему же процесс обработки исключений смог прочитать адрес? [Руководство к STM32F1](https://www.st.com/resource/en/reference_manual/cd00171190.pdf) даёт подсказку:

`ЦПУ Cortex®-M3 всегда получает значение вектора перезагрузки по шине ICode, что подразумевает, что загрузчик должен располагаться только в области кода (обычно, флэш-памяти).`

Запрос вектора перезагрузки происходит через шину ICode и таким образом обрабатывается как запрос инструкции, который разрешен несмотря на включенную защиту. По всей видимости, она следит только за обращениями к данным, что позволяет беспрепятственно запрашивать вектор перезагрузки по ICode. [Справочное руководство на Cortex-M3](http://infocenter.arm.com/help/topic/com.arm.doc.ddi0337e/DDI0337E_cortex_m3_r1p1_trm.pdf) предоставляет дополнительную информацию по запросам векторов:

`Запрос вектора производится либо по системной шине, либо по шине ICode в зависимости от местонахождения таблицы векторов [...]`

В конечном счёте, защита от чтения семейства STM32F1 не блокирует доступ к памяти по шине ICode. При возникновении исключения, соответствующий адрес, расположенный во внутренней памяти запрашивается по ICode, тем самым раскрывая содержимое памяти через PC.

## **Эксплуатация**

Мы заметили, что при возникновении исключения запрос вектора раскрывает защищенные данные через PC. Далее мы рассмотрим, как такое поведение можно эксплуатировать для обхода системы защиты.

Мы уже упоминали таблицу векторов: она содержит инициализирующее значение главного указателя на стек \(main stack pointer, MSP\), за которым следуют указатели на обработчики всех исключений. Её содержимое, определённое архитектурой ARMv7-M, показано в Таблице 1. В ней содержатся исключения и их смещения относительно начала таблицы. Первые 16 исключений являются обязательными и определены архитектурой ARMv7-M. Остальные, так называемые "внешние прерывания", опциональны и зависят от устройства.

| Номер прерывания | Прерывание | Смещение |
| :--- | :--- | :--- |
| - | Инициализирующее значение MSP | 0x00 |
| 1 | Reset | 0x04 |
| 2 | NMI | 0x08 |
| 3 | HardFault | 0x0c |
| 4 | MemManage | 0x10 |
| 5 | BusFault | 0x14 |
| 6 | UsageFault | 0x18 |
| 7-10 | Reserved | 0x1c |
| 11 | SVCall | 0x20 |
| 12 | DebugMonitor | 0x24 |
| 13 | Reserved | 0x28 |
| 14 | PendSV | 0x2c |
| 15 | SysTick | 0x30 |
| 16 | Внешнее прерывание 1 | 0x34 |
| … | … | … |

Таблица 1. Таблица векторов для микроконтроллеров архитектуры ARMv7-M

Главная идея эксплуатации заключается в намеренной генерации исключений, при этом соответствующая ячейка таблицы будет запрашиваться из флэш-памяти в регистр PC. Однако в Таблице 1 видно, что некоторые элементы зарезервированы и не имеют привязки к исключению. В частности это элементы с 7 по 10-й, а также 13-й. К тому же инициализирующее значение MSP также не связано с исключением. В связи с этим рассматриваемый метод не позволяет извлечь данные по этим смещениям. Мы ненадолго отвлечемся от этой проблемы, но вернёмся к ней позже.

## Смещение таблицы векторов

На данном этапе может возникнуть вопрос: зачем мне беспокоиться о безопасности содержимого таблицы векторов?

Действительно, её содержимое обычно не является конфиденциальным, поскольку содержит только указатели на обработчики прерываний. Однако архитектура ARMv7-M имеет регистр смещения таблицы векторов \(Vector Table Offset Register, VTOR\). Он определяет расположение таблицы в адресном пространстве. Обычно его используют, когда на микроконтроллере должно работать несколько приложений, например, загрузчик \(bootloader\) и основное приложение. С его помощью каждое из приложений может иметь свою собственную таблицу прерываний и свои обработчики. Ключевым моментом является то, что отладочный интерфейс предоставляет доступ ко всему устройству за исключением флэш-памяти. При помощи VTOR мы можем перемещать таблицу по всей внутренней памяти, и таким образом получать доступ к большей части данных.

Для того, чтобы изменить расположение таблицы векторов, мы разделим всё адресное пространство флэш-памяти на одинаковые блоки по 32 слова каждый, как изображено на Рисунке 2. Размер блока определяется размером самой таблицы и согласно спецификации ARMv7-M должен быть степенью 2. К примеру, STM32F103 имеет 59 исключений. Таким образом его таблица векторов имеет размер 64. Тем не менее, это значит, что у нас не хватает исключений, чтобы получить доступ ко всем элементам таблицы. Поэтому мы будем использовать самую большую таблицу размером менее 59, то есть 32.

![&#x420;&#x438;&#x441;&#x443;&#x43D;&#x43E;&#x43A; 2. &#x41F;&#x435;&#x440;&#x435;&#x43C;&#x435;&#x449;&#x435;&#x43D;&#x438;&#x435; &#x442;&#x430;&#x431;&#x43B;&#x438;&#x446;&#x44B; &#x432;&#x435;&#x43A;&#x442;&#x43E;&#x440;&#x43E;&#x432; &#x432;&#x43E; &#x444;&#x43B;&#x44D;&#x448;-&#x43F;&#x430;&#x43C;&#x44F;&#x442;&#x438;. &#x41F;&#x43E;&#x434;&#x441;&#x432;&#x435;&#x447;&#x435;&#x43D;&#x44B; &#x43D;&#x435;&#x434;&#x43E;&#x441;&#x442;&#x443;&#x43F;&#x43D;&#x44B;&#x435; &#x44D;&#x43B;&#x435;&#x43C;&#x435;&#x43D;&#x442;&#x44B;.](https://lh6.googleusercontent.com/bXzaGZai330C1hhXvg_AyU2DtpsUZwq0lrm0Y_S0SpqpMOGVPOZpGu2BaINOx4MYCz5bE2ljwgj9oOHSZgbXCWitIH063pov7QGCwYDJ7AtWtjXAg4CRNi2bRrUVRvLiov1ZW1A)

Семь подсвеченных ячеек на Рисунке 2 невозможно извлечь предлагаемым способом. Первые две - инициализирующее значение MSP и вектор перезагрузки. Остальные соответствуют зарезервированным областям. Инициализирующее значение MSP и вектор перезагрузки могут быть извлечены только когда сама таблица расположена по умолчанию в начале флэш-памяти. Причиной этому является то, что при перезагрузке нужно получить эти значения, но при этом перезагрузка сбрасывает значение VTOR, возвращая таблицу векторов на стандартное место.  
  
Эти ограничения можно обойти, используя исключения, которые превосходят размеры таблицы. В нашем случае те, что больше 31. Согласно разделу 4.4.4 инструкции по программированию STM32F1 \([STM32F1 Programming Manual](https://www.st.com/resource/en/programming_manual/cd00228163.pdf)\), адрес расположения таблицы векторов должен быть выровнен в соответствии с размером таблицы \(64 в случае STM32F103\). Но что происходит, когда таблица не выровнена, а мы генерируем исключения, номера которых выходят за границы таблицы? Мы выяснили, что эти исключения возвращаются в начало таблицы. На Рисунке 3 изображен этот возврат значений на не выровненной таблице, содержащей 32 элемента. С левой стороны - обычная таблица с подсвеченными не доступными элементами. Справа - положение позволяющее получить к ним доступ. Теперь к подсвеченным ячейкам можно получить доступ через прерывания, расположенные за пределами таблицы.  


![](https://lh3.googleusercontent.com/SyhhlB0TDHWjf0P2tJJHeieo58dra53MMPikSUhgMm2s7pMu-QXGXhJe-qcUTdyGr0LT30NrlHtC8lm6gRhBxZ2fRn8jBu8ksrlV9ZADKSVMruYiklNV2h52sdce0BfII4skR6U)

Рисунок 3. Перемещение значений в не выровненной таблице векторов. Недоступные ячейки \(красные\) становятся доступными \(синие\) при генерации исключения, выходящего за пределы таблицы.

Обратите внимание на недоступные части таблицы векторов на Рисунке 3. Остальные элементы также могут быть извлечены с помощью подобного "оборота", хотя в этом нет необходимости, потому что они доступны обычным способом. С помощью "оборота" мы получили доступ к недоступным ранее элементам. Это касается и первых двух элементов, доступных только когда таблица расположена в начале флэш-памяти. Единственным ограничением осталось лишь то, что этот подход можно использовать только для не выровненных таблиц векторов. Так или иначе, мы сократили количество недоступных элементов вдвое. Заметьте, что этот способ является лишь одним вариантом использования внешних прерываний. Мы использовали его, потому что его просто реализовать.

Теперь у нас есть почти всё для извлечения произвольных частей внутренней памяти. В следующем разделе мы рассмотрим последний недостающий элемент: как целенаправленно генерировать исключения.

## **Генерация исключений**

Чтобы генерировать исключения мы пойдём следующим путём. Каждое исключение потребует трёх шагов:

1. Произвести перезагрузку устройства, чтобы микроконтроллер находился в определенном, заранее известном, состоянии и чтобы восстановить его в случае сбоя или зависания.
2. Добиться того, чтобы следующим шагом вызывалась обработка требуемого исключения.
3. Выполнить шаг, чтобы запустить обработчик.

Таких прерываний, как Non-Maskable Interrupt \(NMI\), PendSV и SysTick несложно добиться установкой битов NMIPENDSET, PENDSVSET и PENDSTSET соответственно. Эти биты находятся в регистре состояния и управления прерываниями \(Interrupt Control and State Register, ICSR\). То же самое относится к исключению DebugMonitor, который можно активировать установкой бита MON\_PEND в регистре управления и мониторинга исключения отладки \(Debug Exception and Monitor Control Register, DEMCR\).

 К примеру, следующая команда OpenOCD вызовет прерывание PendSV:  
`mww 0xe000ed04 0x10000000`

**Поскольку процессор остановлен и находится в режиме отладки, мы должны дать ему шанс вызвать исключение. Для этого мы делаем отладчиком один шаг, выполняя одну инструкцию. Для того, чтобы избежать побочных эффектов от неё, мы используем инструкцию nop, расположенную в SRAM по адресу 0x20000000.**  

