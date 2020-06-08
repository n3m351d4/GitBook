---
description: 'June 21, 2018'
---

# Глава 1. Дискретная ошибка измерения дальности и шкала времени.

Спутниковая навигационная система представляет собой комплекс орбитальных объектов, контрольных станций и приемников потребителей, имеющих жесткую привязку по пространственно-временным параметрам. Более того, большинство параметров орбитального движения НКА \(навигационный космический аппарат\) с точки зрения потребителя выражается через функции от времени. Применение пассивных \(беззапросных\) дальномерных методов подразумевает взаимную синхронизацию повремени излучения всех НКА.

При этом все компоненты СНС должны использовать единую шкалу времени. Очевидно, что реализовать эти требования в полной мере невозможно, так как для этого потребуется оснастить идентичными высокоточными эталонами частоты \(времени\) все составляющие системы, включая аппаратуру потребителей.

Поэтому на практике применяют три шкалы времени – **системную шкалу, бортовую шкалу и шкалу потребителя**, перечисленные в порядке снижения доступной точности.

Основной шкалой времени СНС является **системная**, поскольку она оказывает прямое или косвенное влияние на синхронизацию всех подсистем СНС. Для ее формирования используются наиболее точные квантовые эталоны времени и частоты, расположенные в наземных командно-измерительных станциях и обеспеченные специальными техническими и алгоритмическими решениями. Бортовая шкала времени формируется квантовым эталоном \(“атомными часами”\) расположенным на борту НКА. К этому эталону привязываются все навигационные сигналы, излучаемые НКА.

**Например, сдвиг излучаемых дальномерных кодов и меток времени на 1 мс соответствует погрешности измерения дальности в 300 км.**

**Бортовые эталоны** работают в гораздо более жестких условиях и технически менее обеспечены, поэтому имеют меньшую стабильность, чем системные эталоны. Между бортовой и системной шкалой неизбежно возникает расхождение, которое измеряется и периодически устраняется наземным сегментом управления. Для определения величины ухода бортовой шкалы относительно системной наземный комплекс управления производит сверку шкал. В процессе сверки по принятым наземной станцией навигационным сигналам НКА измеряется значение времени по бортовой шкале на момент начала излучения сигнала т.изл. В момент начала его приема наземной станцией время по бортовой шкале вычисляется, **как**

![](https://telegra.ph/file/b492e88f21e53ec600f64.png)

Обычно коррекция бортовой шкалы времени происходит эпизодически, при ее уходе относительно системной шкалы больше допустимого значения.

**Существует два вида коррекции**.

В случае обнаружения смещения временных интервалов шкал \(разность фаз кодовых последовательностей\) производится **фазирование** бортовой шкалы, позволяющее совместить шкалы с точностью до десятков наносекунд.

Совместно с **фазированием** либо независимо от него может проводится изменение оцифровки \(коррекции кода на целое число секунд\).

В настоящее время все СНС управляются с ограниченных территорий и коррекцию бортовых шкал времени удается производить лишь эпизодически. Поэтому применяется **прогнозирование** ухода бортовой шкалы.

По результатам длительных наблюдений за уходом бортовой шкалы составляется математическая модель и вычисляется систематическая составляющая ухода. Предсказание систематической составляющей позволяет загрузить в бортовую память каждого НКА соответствующие частотно-временные поправки, которые затем передаются потребителю в составе эфемеридной информации.

Наиболее нестабильной и смещенной относительно системной шкалы является шкала времени **потребителя**, которая формируется кварцевым генератором приемника. Шкала времени **потребителя** корректируется при помощи соответствующей части навигационной информации, принимаемой с НКА. Существует несколько способов синхронизации шкалы времени потребителя.

**В первом способе информация**, принятая потребителем, используется как для расчета текущего ухода бортовой шкалы времени относительно системной, так и для привязки шкалы времени потребителя к системной шкале при нахождении временной координаты Этот способ наиболее распространен и обеспечивает точность привязки не менее 1 мкс.

**Второй способ основан на том**, что потребителю в навигационном сообщении передаются метка времени НКА и частотно-временные поправки. Точность привязки в этом случае ниже и зависит от знания дальности до НКА \(т.е. времени распространения сигнала ДtРаспр\).

**Третий способ аналогичен второму**, но в качестве источника информации используются дальномерные коды.

#### Дискретная ошибка измерения дальности  <a id="&#x414;&#x438;&#x441;&#x43A;&#x440;&#x435;&#x442;&#x43D;&#x430;&#x44F;-&#x43E;&#x448;&#x438;&#x431;&#x43A;&#x430;-&#x438;&#x437;&#x43C;&#x435;&#x440;&#x435;&#x43D;&#x438;&#x44F;-&#x434;&#x430;&#x43B;&#x44C;&#x43D;&#x43E;&#x441;&#x442;&#x438;"></a>

Дискретное измерение дальности сводится к счету числа Nэт эталонных импульсов, которые генерируются в течении времени tд запаздывания импульса цели относительно импульса синхронизации. При периоде Тэт или частоте Fэт эталонных импульсов, время

tд=Nэт\*Тэт=Nэт/Fэт,

а дальность цели

Д = c\*tд/2=с\*Nэт/2Fэт.

**Эталонные импульсы могут не совпадать с импульсами синхронизации и цели; тогда истинная дальность отличается от подсчитанной** по формуле на величину ΔДдкр, называемую **ошибкой дискретности.** Полагая, что суммарная ошибка начала и окончания отсчета равна периоду Тэт, имеем

ΔДдкр=с\* Тэт/2=с/2Fэт

или в среднеквадратических значениях

σд дкр= ΔДдкр/2√3= с\* Тэт/4√3=с/4√3\*Fэт.

Между дальностью Д и периодом Тэт существует прямо пропорциональная зависимость. Следовательно, относительная среднеквадратическая ошибка дальности δд нст/Д равно относительной среднеквадратической нестабильности периода δт/Тэт или частоты σF/Fэт следования импульсов. Отсюда

δд дкр= ДσF/Fэт

{% embed url="https://t.me/in51d3" %}

{% embed url="https://telegra.ph/Radiolokaciya-i-radionavigaciya-Glava-1-Diskretnaya-oshibka-izmereniya-dalnosti-i-shkala-vremeni-06-21" %}


