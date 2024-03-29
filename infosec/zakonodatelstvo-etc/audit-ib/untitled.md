# Классификация межсетевых экранов

В основе классификации заложено деление по уровням контроля межсетевых информационных потоков (рисунок 1). &#x20;

![](<../../../.gitbook/assets/image (38).png>)

Рисунок 1. Классы защищенности межсетевых экранов

Самый высокий — 1-й класс. Он применим, когда требуется обеспечить безопасное взаимодействие автоматизированных систем класса 1А с внешней средой.

Если в автоматизированных системах классов 3А, 2А обрабатывается информация с грифом «особой важности», то требуется использовать межсетевой экран не ниже 1 класса.

Для защиты взаимодействия с внешним миром систем класса 1Б предназначены межсетевые экраны 2 класса. Если в системах классов 3А, 2А обрабатывается информация с грифом «совершенно секретно», необходимо использовать межсетевые экраны не ниже указанного класса.

Защита взаимодействия систем класса 1В обеспечивается межсетевыми экранами 3 класса. Если в автоматизированных системах классов 3А, 2А происходит обработка информации с грифом «секретно», потребуется межсетевой экран не ниже 3 класса.

Межсетевые экраны 4 класса требуются в случае защиты взаимодействия автоматизированных систем класса 1Г с внешней средой.

Взаимодействие систем класса 1Д должны обеспечивать межсетевые экраны 5 класса. Также требуется использовать их не ниже указанного класса для автоматизированных систем класса 3Б и 2Б.

**Классы защиты межсетевых экранов**

В 2016 г. введена шестиуровневая классификация межсетевых экранов (рисунок 2).

![](<../../../.gitbook/assets/image (7) (1).png>)

Рисунок 2. Классификация межсетевых экранов по классам защиты

По такому подходу, в информационных системах защита информации, содержащей сведения, отнесенные к государственной тайны, должна обеспечиваться с помощью межсетевых экранов 3, 2 и 1 классов защиты.

Что касается остальных случаев, то в государственных информационных системах (ГИС) 1, 2 и 3 классов защиты должны использоваться межсетевые экраны 4, 5 и 6 классов соответственно. Аналогичная ситуация в отношении автоматизированных систем управления производственными и технологическими процессами (далее — АСУ).

Для обеспечения 1 и 2 уровней защищенности персональных данных в информационных системах персональных данных (ИСПДн) потребуется устанавливать межсетевые экраны 4 и 5 классов соответственно. Если же требуется обеспечить 3 или 4 уровень защищенности персональных данных, то достаточно 6 класса защиты.

Регулятор также определил необходимость использования межсетевых экранов 4 класса для защиты информации в информационных системах общего пользования II класса.

**Типы межсетевых экранов**

Регулятор выделяет межсетевые экраны уровня:

·        сети (тип «А»);

·        логических границ сети (тип «Б»);

·        узла (тип «В»);

·        веб-сервера (тип «Г»);

·        промышленной сети (тип «Д»).

Межсетевые экраны типа «А» реализуется только в программно-техническом исполнении. «Б», «Г» и «Д» могут быть как в виде программного продукта, так и в ПТИ. Исключительно в виде программного продукта реализуется межсетевые экраны типа «В».

Межсетевые экраны типа «А» устанавливают на физическом периметре инфомационных систем. Если в них есть несколько физических сегментов, то такой межсетевой экран может устанавливаться между ними (рисунок 3). &#x20;

![](<../../../.gitbook/assets/image (27).png>)

Рисунок 3. Пример размещения межсетевых экранов типа «А»

Межсетевые экраны типа «Б» устанавливаются на логической границе информационных систем. Если в них выделяются логические сегменты, то между ними также может быть установлен межсетевой экран данного типа (рисунок 4).

![](<../../../.gitbook/assets/image (49).png>)

Рисунок 4. Пример размещения межсетевого экрана класса «Б»

Для размещения на мобильных или стационарных узлах информационных систем предназначены межсетевые экраны типа «В».

Разбор http(s)-трафика между веб-сервером и клиентом осуществляют межсетевые экраны типа «Г». Они устанавливаются на сервере, обслуживающем веб-сайты (службы, приложения). Возможна установка на физической границе сегмента таких серверов.

С промышленными протоколами передачи данных работают межсетевые экраны типа «Д». Именно они устанавливаются в АСУ.

Источник: https://www.anti-malware.ru/analytics/Market\_Analysis/infosecurity-systems-classification-fsb-fstek
