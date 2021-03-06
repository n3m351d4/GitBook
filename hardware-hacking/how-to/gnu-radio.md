---
description: 2020 @in51d3
---

# Hardware Hacking: Основы работы с GNU Radio

> Перевела @ZzzNein \(безграничную благодарность можно выразить [ЗДЕСЬ](https://yasobe.ru/na/na_perevody_i_kontent)\), проверила @N3M351DA. [Оригинальный текст](https://www.blackhillsinfosec.com/gnu-radio-primer/) - Реймонд Фелч, 9 декабря 2019 г.‌

**Внимание!** При передаче любых данных обязательно используйте клетку Фарадея или экранированную сумку, чтобы случайно не нарушить каких-либо законов несанкционированным вмешательством в эфир на определенных частотах. Кроме того, помните, что перехват и дешифрование/декодирование чужих данных является незаконным, поэтому будьте осторожны при исследовании вашего трафика.

![](../../.gitbook/assets/image%20%28281%29.png)

##  **Предисловие**

Недавно я окунулся в мир SDR \(программно-определяемого радио\) и в процессе многому научился. Я выяснил, что могу не только эмулировать радиосигналы посредством применения различных программных средств и приложений \(например, SDR Sharp, GQRX, SIGINTOS, CubicSDR, ПО, поддерживающее RTL-SDR, и т.д.\), но и использовать схемы GNU Radio, такие как ‘grgsm\_livemon.grc’, для захвата пакетов мобильной сети GSM в реальном времени.

Каким бы информативным и образовательным ни был этот проект, мне бы хотелось подробнее разобраться в схемах GNU Radio. Также хочется изучить основы создания таких схем, а не просто полагаться на чьи-то чужие примеры, до конца не понимая, как они работают.

Несмотря на то, что в сети можно найти довольно много документации, GNU Radio \(и в частности - GNURADIO-COMPANION\) может представлять трудность и пугать юзеров при первом обращении. Немного поискав в Google, я в конце концов наткнулся на [онлайн-уроки Майкла Османа из Great Scott Gadgets](https://greatscottgadgets.com/sdr/%20) и, стоит сказать, что они мне здорово помогли. Кроме того, [гайды по GNU Radio](https://wiki.gnuradio.org/index.php/Tutorials) помогли закрепить свежие знания.

Чтобы познакомиться с основами GNU Radio, я выбрал проект, который мог быть использован для рассмотрения атак методом повторения радиосигнала \(обнаружение, захват и воспроизведение необработанного радиочастотного сигнала и разблокировки моего Ford Mountaineer 1999 г. путем обхода частотно-регулируемой кнопки \(FOB\).

![](../../.gitbook/assets/image%20%28274%29.png)

Нужно заранее сказать, что, хотя я и смог захватить сигнал разблокировки с FOB и воспроизвести его \(с помощью HackRF\), автомобиль я не открыл. Дело в том, что много лет назад были внесены изменения с целью предотвращения упомянутых атак методом повторения радиосигнала. Использовалась техника непрерывно изменяющегося кода \(иногда называется «псевдослучайный код»\). 

Перед внесением обозначенного выше изменения автомобили и устройства открывания гаражных ворот использовали фиксированные коды, и атакующий с помощью соответствующего приемника мог обнаружить эти устройства. Злоумышленник получал данные для последующего использования. На момент написания этой статьи было уже много упоминаний об обходе непрерывно изменяющегося кода, включая использование методов глушения и сброса кода.

Тем не менее, я обнаружил, что работа над проектом стоила затраченных усилий. Она позволила лучше понять GNU Radio и приобрести необходимые знания для созданий собственных рабочих схем.

## **Основы** **GNU** **Radio** **и информация о схемах:**

* Вся обработка сигналов в GNU Radio выполняется с использованием схем, состоящих из отдельных блоков, каждый из которых выполняет всего одну операцию цифровой обработки сигнала: фильтрацию, дешифрование, мультиплексирование и т.д.
* Данные передаются между блоками в различных форматах, комплексных или вещественных целых числах, с плавающей запятой или просто в виде данных любого типа по вашему выбору.
* Каждой схеме требуется как минимум один исходный блок \(вход\) и один блок-приемник \(выход\).
* Источником или приемником может быть USB-донгл, HackRF, BladeRF, звуковая карта, файл или fft \([БПФ – быстрое преобразование Фурье](https://en.wikipedia.org/wiki/Fast_Fourier_transform)\), и это лишь некоторые из них. 

##  **Получение информации о моем** **FOB:**

Чтобы определить рабочую частоту моего FOB, я нашел номер FCC, который ассоциирован с моим брелоком. После поиска в Google, а затем поиска FCC, я получил следующую информацию:

![](../../.gitbook/assets/image%20%28277%29.png)

Результатом поиска по FCC была рабочая частота:

![](../../.gitbook/assets/image%20%28272%29.png)

Вооружившись данными о рабочей частоте моего устройства \(315 МГц\), я посчитал, что пора сделать попытку захвата сигнала «разблокировки автомобиля». В стандартной ситуации я мог бы использовать одно из многих доступных приложений \(SDR Sharp и т.д.\) для его отслеживания и захвата необработанных данных. Однако в данном случае я хотел самостоятельно создать схему GNU Radio.

Я перешел в директорию моих проектов и вызвал следующую команду:

```text
$ sudo gnuradio-companion 
```

После загрузки gnuradio-companion, я создал новую схему:

![](../../.gitbook/assets/image%20%28268%29.png)

Обратите внимание, что новая схема открывается уже с двумя блоками - «Опции» и «Переменная». «Опции» \(ID: top\_block\) определяют библиотеку цепочки инструментов, которую мы будем использовать: QT-GUI, WX-GUI, none, блок иерархии \(hier block\) и т.д. А блок «Переменной» \(ID: samp\_rate\) создан для выбора частоты дискретизации нашего сигнала, по умолчанию – 32 ksps \(32 тысяч выборок в секунду\).

**Примечание**: Щелкните правой кнопкой мыши по любому блоку, чтобы отобразить его свойства и сменить параметры.

![](../../.gitbook/assets/image%20%28275%29.png)

Я буду использовать HackRF \(с блоком osmocom\). Рекомендуемая минимальная частота дискретизации при использовании HackRF составляет 2M выборок в секунду. Выберем источник osmocom, найдя и дважды кликнув по нему в списке.

![](../../.gitbook/assets/image%20%28278%29.png)

На данном этапе, если бы мне захотелось изменить переменную частоты дискретизации с 32k \(32e3\) \(по умолчанию\) на 2M \(2e6\), я бы мог это сделать. Любой блок схемы, содержащий переменную ‘samp\_rate’ теперь будет использовать новое \(скорректированное\) значение.

Также было бы очень полезно иметь переменную для центральной частоты 315 МГц \(315e6\). Этого легко добиться, скопировав и вставив блок переменных samp\_rate и присвоив новому блоку переменных имя center\_freq. 

![](../../.gitbook/assets/image%20%28269%29.png)

Теперь можно щелкнуть правой кнопкой мыши по блоку источника osmocom и сменить частоту канала CH0, заданную по умолчанию \(100e6\), чтобы она стала переменной «center\_freq». Моя схема теперь выглядит так:

![](../../.gitbook/assets/image%20%28283%29.png)

Обратите внимание, что за значениями двух переменных «samp\_rate» и «center\_freq» теперь следует блок источника osmocom. Определенные переменные очень удобны, поскольку размер схем увеличивается и становится сложнее для понимания. 

Также обратите внимание на вывод источника osmocom, окрашенный в синий цвет. Этот цвет указывает на комплексное значение \(действительное и мнимое\), которое будет предоставлено на выходе. Если бы цвет вывода был оранжевым, то на выходе было бы действительное значение \(целое, с плавающей запятой, абсолютное и т.д.\). Это важная концепция, о которой нужно знать. Цвета ввода и вывода должны совпадать, иначе будет выводиться ошибка. Несоответствия можно исправить, отредактировав свойство «Тип вывода» или «Тип ввода» блока.

Наконец, обратите внимание, что источник \(ID\) osmocom подсвечен красными буквами. GNU Radio использует этот цвет для обозначения ошибки в схеме. В этом случае ошибка относится к блоку Source, у которого отсутствует выходное соединение. Это можно обойти, если добавить выходной \(приемный\) блок и соединить два блока вместе.

## **Создание** **приемника** **сигнала \(Sink\):**

Для завершения создания «предварительной» схемы, мне нужно добавить блок FFT Sink, чтобы можно было отслеживать активность сигнала и гарантировать, что все работает, как ожидалось. Этого можно добиться, выбрав блок QT-GUI Frequency Sink в разделе Инструменты \(Instrumentation\). Нужно открыть окно свойств и указать переменную center\_freq в качестве параметра центральной частоты. Кроме того, необходимо подключить вывод источника osmocom к вводу QT-GUI Frequency Sink.

![](../../.gitbook/assets/image%20%28280%29.png)

Чтобы запустить схему, надо нажать на кнопку «Execute». 

**Примечание**: Чтобы остановить процесс, нажмите на красный “X” справа от зеленой кнопки Execute.

![](../../.gitbook/assets/image%20%28284%29.png)

Должно открыться окно FFT с изображением сигнала 315 МГц, полученного с HackRF.

![](../../.gitbook/assets/image%20%28282%29.png)

В данной точке я вижу, что схема дискретизирует полосу пропускания 2 МГц с центральной частотой 315 МГц. Пока что она работает, как ожидалось. 

Далее предположим, что я бы хотел иметь возможность нажать на кнопку разблокировки брелока FOB и отобразить результирующий сигнал. Это можно сделать с помощью той же схемы, но сначала нужно открыть уже запущенное окно FFT и нажать на среднюю кнопку мыши. Откроется выпадающее меню, где можно выбрать опцию ‘Max Hold’. Данная функция позволяет вывести любой сигнал выше, чем текущий отображаемый.

Выполнение моей схемы с опцией Max Hold приводит к следующим результатам при нажатии на кнопку брелока:

![](../../.gitbook/assets/image%20%28266%29.png)

## **Захват сигнала разблокировки** **FOB** **для последующего воспроизведения:**

Чтобы захватить результирующий сигнал, сгенерированный FOB, и сохранить его в файл, мне нужно включить в мою потоковую схему еще один блок Sink \(вывод\). Данный новый блок можно найти в разделе операторов \(File Operators\) под названием File Sink. Нажатием правой кнопки мыши нужно вызвать окно со свойствами и ввести имя файла для захваченных необработанных данных. Название файла - ‘fob\_capture’. 

![](../../.gitbook/assets/image%20%28271%29.png)

**Примечание**: Записанные файлы могут занимать много памяти, поскольку мы будем заниматься дискретизацией 2-х миллионов выборок в секунду с использованием представления с плавающей запятой. По этой причине я запущу схему, нажму кнопку FOB, а затем как можно быстрее остановлю ее.

Измененная схема теперь выглядит следующим образом:

![](../../.gitbook/assets/image%20%28273%29.png)

В результате работы схемы и захвата сигнала получаем необработанный файл размером 49,3 МБ.

Перечень файлов каталога:

```text
-rw-r--r-- 1 root    root    49311744 Nov 10 21:06 fob_capture
```

##  **Воспроизведение захваченного сигнала из файла:**

**Примечание**: В приложении GNU Radio можно отключать блоки нажатием правой кнопки мыши на блок и выбрав опцию «отключить» \(‘Disable’\) в выпадающем меню. Блок будет по-прежнему отображаться, но станет серым - неактивным.

Для воспроизведения захваченного сигнала обычно создается абсолютно новая схема. Однако, чтобы продемонстрировать опцию «отключения» блока \(‘Disable block’\), я решил просто отключить блоки osmocom Source, QT GUI Frequency Sink и File Sink. После отключения эти блоки подсвечиваются серым.

Далее я создаю новый блок источника, выбрав блок File Source из набора операторов File Operators. Я открыл свойства блока и ввел имя ранее захваченного файла - ‘fob\_capture’.

![](../../.gitbook/assets/image%20%28270%29.png)

Затем я создал копию предыдущего блока QT-GUI Frequency Sink \(FFT\), чтобы иметь возможность отслеживать воспроизведение сигнала.

Последнее, что я сделал - добавил блок osmocom Sink для воспроизведения сигнала с помощью HackRF.

Моя измененная схема теперь выглядит так:

![](../../.gitbook/assets/image%20%28276%29.png)

На [видео ](https://www.blackhillsinfosec.com/wp-content/uploads/2019/12/20191110_221754-1.mp4)с телефона виден результат работы схемы.

**Примечание**: Параметр «Repeat» в блоке File Source указывает на то, что файл будет повторяться \(в бесконечном цикле\). В идеале мы могли бы разрешить воспроизведение сигнала только один раз, но в демонстрационных целях я решил повторить воспроизведение.

## **Другая полезная информация о** **GNU** **Radio:**

Если у вас есть какой-либо опыт использования таких аппаратных средств, как осциллографы, генераторы частоты, частотомеры, анализаторы спектра и т.д., хорошо иметь понимание того, что их можно добавлять к схемам Gnu Radio для более глубокого изучения и тестирования в рамках ваших проектов.

Ниже показано использование осциллографа для того, чтобы зафиксировать захваченный сигнал FOB на гораздо более низком уровне, показывая реальные цифровые импульсы, составляющие код. Хотя такая подробная информация для атаки с воспроизведением не обязательна, она поможет узнать больше о записанном сигнале.

Обратите внимание, что вместо библиотеки QT-GUI я использую библиотеку WX-GUI для демонстрации использования блока scope. 

![](../../.gitbook/assets/image%20%28267%29.png)

##  **Блок** **throttle** **\(когда использовать и зачем\):**

Также стоит упомянуть блок Throttle, который можно найти в категории «Разное» \(‘Misc’\). Он обычно присоединяется непосредственно к выводу «неаппаратного» исходного блока \(блока источника сигнала - Signal Source и т.д.\), чтобы ограничить скорость, с которой этот исходный блок создает выборки. Можно использовать Throttle для того, чтобы ограничить поток выборок с тем, чтобы средняя скорость не превышала заданную \(samp\_rate\). 

Блок Throttle необходимо исключительно в том случае, если ваша схема не содержит блока ограничения скорости, который часто представлен аппаратными средствами \(SDR, динамик, микрофон и т.д.\). К примеру, блок Throttle может применяться в схеме, симулирующей реальное оборудование, но при отсутствии действующего синхрогенератора.

На примере потоковой схемы ниже, если удалить блок Throttle, результат будет выглядеть так же, но ЦП будет загружен на 100% и утилита GNU Radio может вылететь. Это хороший пример того, когда следует использовать блок Throttle.

![](../../.gitbook/assets/image%20%28265%29.png)

Следует также отметить, что «усеченная» выборка не предназначена для точного управления частотой дискретизации. Ее должен контролировать источник или приемник, привязанный к тактовому сигналу \(USRP или звуковая карта\), в этом случае блок Throttle будет не нужен.

В целом, блок Throttle обычно не требуется, и его следует намеренно избегать, особенно при использовании синхрогенератора аппаратного источника \(USRP, SDR-RTL и т.д.\). Это связано с тем, что блок Throttle – программный синхрогенератор и, как правило, его использование может привести к конфликту с подобным генератором аппаратного обеспечения, что потенциально может привести к ошибкам.

**Практическое правило**: если к схеме подключено радиоустройство \(USRP, SDR-RTL и т.д.\) **ИЛИ** аудиоустройство \(звуковая карта и т.д.\), **НЕ ИСПОЛЬЗУЙТЕ** блок Throttle.

## **Выводы**

Как сказано ранее, несмотря на то, что мне удалось захватить сигнал разблокировки моего FOB и воспроизвести его, но не удалось открыть автомобиль, я тем не менее считаю данную попытку удачной. 

В будущем, несомненно, я буду не раз возвращаться к проектам, работающим с радиочастотой. Следовательно, будут всплывать новые уязвимости. Поскольку GNU Radio дает возможность эмулировать дорогостоящее радиооборудование, эта утилита всегда будет ценным инструментом в моем арсенале.

Это похоже на игру в кошки-мышки, но благодаря постоянным исследованиям и вкладу сообщества мы можем делиться знаниями и обеспечивать безопасность наших систем.

