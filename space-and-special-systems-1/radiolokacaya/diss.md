---
description: 'June 24, 2018'
---

# ДИСС

_**Доплеровские измерители путевой скорости и сноса**_ \([_**ДИСС**_](http://lliric.narod.ru/usl_/usl_.htm)\) предназначены для непрерывного определения путевой скорости и угла сноса [_**ЛА**_](http://lliric.narod.ru/usl_/usl_.htm), а также выдачи этих данных в прицельно-навигационный комплекс, в систему автоматического управления и на индикаторы.

Относительная ошибка определения 'путевой скорости составляет 0,2 - 0,3%, а точность определения угла сноса характеризуется среднеквадратической ошибкой 0.1 - 0.5°.

Работа [_**ДИСС**_](http://lliric.narod.ru/usl_/usl_.htm) основана на использовании эффекта Доплера. Сущность эффекта Доплера заключается в том, что при облучении земной поверхности с движущегося [_**ЛА**_](http://lliric.narod.ru/usl_/usl_.htm) частота принятых на нем отраженных от земли радиоволн fпр отличается от частоты радиоволн, излученных передатчиком fпep. Абсолютная величина разности этих частот называется доплеровской частотой

fд = \| fпр - fпер \| .

Доплеровская частота зависит от радиальной скорости [_**ЛА**_](http://lliric.narod.ru/usl_/usl_.htm) относительно земли:

![](https://telegra.ph/file/18241fa9262a9973ae452.png)

где _W_r—радиальная скорость [_**ЛА**_](http://lliric.narod.ru/usl_/usl_.htm) — скорость в направлении излучения радиоволны; _h_ — длина радиоволны передатчика.

Антенная система доплеровского измерителя формирует диаграмму направленности в виде узких лучей, отклоненных в сторону Земли в вертикальной плоскости на угол o и относительно продольной оси [_**ЛА**_](http://lliric.narod.ru/usl_/usl_.htm) на угол в \(рис.28.\).

![](https://telegra.ph/file/66715391d149f1238b6a6.png)

Радиальная скорость _W_R представляет собой проекцию путевой скорости на направление излучения.

 Для повышения точности измерения путевой скорости и угла сноса, а также автоматической компенсации погрешностей измерений, возникающих при продольном и поперечном кренах  [_**ЛА**_](http://lliric.narod.ru/usl_/usl_.htm) , неподвижная антенная система  [_**ДИСС**_](http://lliric.narod.ru/usl_/usl_.htm) формирует четыре луча, направленные симметрично относительно продольной оси  [_**ЛА**_](http://lliric.narod.ru/usl_/usl_.htm). Применяются также доплеровские измерители, у которых три луча расположены симметрично, а четвертый расположен по продольной оси  [_**ЛА**_](http://lliric.narod.ru/usl_/usl_.htm). Этот луч используется для компенсации погрешностей, возникающих из-за различной отражающей способности пролетаемой местности.

Измерение путевой скорости и угла сноса осуществляется сравнением доплеровских частот по парам лучей. При отсутствии сноса вектор путевой скорости совпадает с продольной осью [_**ЛА**_](http://lliric.narod.ru/usl_/usl_.htm), доплеровские частоты по обоим лучам одинаковы и разность их равна нулю. При наличии сноса доплеровские частоты неодинаковы, так как вектор путевой скорости отклоняется от продольной оси [_**ЛА**_](http://lliric.narod.ru/usl_/usl_.htm) на величину угла сноса.

Определение угла сноса и путевой скорости в четырехлучевых доплеровских измерителях производится в вычислителе [_**ДИСС**_](http://lliric.narod.ru/usl_/usl_.htm) путем сравнения суммы и разности доплеровских частот, измеренных каждым радиолучом антенны.

В комплект [_**ДИСС**_](http://lliric.narod.ru/usl_/usl_.htm) входят передатчик, приемник, приемная и передающая антенны с коммутационными устройствами, частотомер, вычислительное устройство, индикатор и пульт управления \(рис.29.\).

![](https://telegra.ph/file/bdae7befa17c933047d40.png)

Высокочастотная энергия передатчика излучается направленно к поверхности земли. Передающая и приемная антенны имеют остронаправленную \(игольчатую\) четырехлучевую диаграмму направленности. Излучение и прием энергии по парам лучей происходит поочередно. Очередность излучения и приема задается синхронизатором. В приемнике в результате сложения частот прямого f0 и отраженного f0 + fд сигналов выделяется доплеровская частота каждой пары лучей. Выделенная и усиленная в приемнике доплеровская частота поступает в частотомер, который вырабатывает постоянные напряжения U1 и U2, пропорциональные доплеровским частотам каждой пары лучей приемной антенны. После усиления оба напряжения выдаются в вычислитель, который предназначен для вычисления путевой скорости и угла сноса. Из вычислителя сигналы, пропорциональные путевой скорости и углу сноса, выдаются на индикатор и потребителям \(рис.30.\).

![](https://telegra.ph/file/25cd3e8afe09b4553aadf.png)

В _****_[_**ДИСС**_](http://lliric.narod.ru/usl_/usl_.htm) предусмотрены следующие режимы работы:

 - «Работа»;

 - «Контроль»;

 - «Память».

> Пилотажный комплекс вертолета ПКВ-8 предназначен для обеспечения улучшения управляемости и повышения устойчивости вертолета, автоматизации управления угловым и пространственным положением вертолета на всех режимах полета, сокращения и упрощения действий пилота при ручном, автоматическом, директорном и комбинированном способах пилотирования.

> Он прошел все виды летных испытаний на вертолете Ми‑17В-5, и был допущен до серийного производства. Данный комплекс получил высокую оценку у летчиков ГЛИЦ за эффективность, количество и качество выполняемых режимов и увеличение безопасности выполнения полетов вследствие снижения нагрузок на летчика. Это также подтверждается отзывами летчиков использующих серийные вертолеты. Вертолет Ми-17В-5, в настоящее время, большой партией поставляется на экспорт и также заказывается некоторыми силовыми структурами РФ.

> ПКВ-8, заменяющий АП-34Б, при взаимодействии со штатным оборудованием базового вертолета Ми-8 \(АГР, ГМК, ПНП\), будет выполнять следующие функции, увеличивающие возможности базовых функций АП-34Б, а также выполнять новые:

> **1.**  Режим «АП» ‑ стабилизация углового положения вертолета, улучшение управляемости вертолетом, управление от 4-х позиционного переключателя угловым положением вертолета, установленного на РЦШ;

> **2.**  Режим «Автоматическое триммирование» ‑ автоматическое триммирование проводки управления, для расширения возможности управления автопилотом до 100 % диапазона управления;

> **3.**  Режим «Стабилизация Vпр» ‑ стабилизация приборной скорости, возможность управления заданным значением приборной скорости от 4-х позиционного переключателя, установленного на РЦШ;

> **4.**  Режим «Стабилизация Нбар» ‑ стабилизация барометрической высоты;

> **5.**  Режим «Стабилизация Vу» ‑ стабилизация вертикальной скорости, с возможностью управления заданным значением от 2-х позиционной кнопки, установленной на РОШ;

> **6.**  Режим «Стабилизация ЗК» ‑ Стабилизация заданного вручную курса \(это ПНП – при наличии его на борту, либо с других задатчиков\);

> **7.**  Режим «Выход на заданную высоту» - Автоматизированный выход на заранее заданную летчиком высоту;

> _При наличии или дополнительной установке системы ДИСС и РВ \(которые, как правило, присутствуют на всех вертолетах\), так же будет обеспечено выполнение следующих функций:_

> **8.**  Режим «Висение» ‑ стабилизация точки висения вертолета в горизонтальной плоскости по сигналам ДИСС, с возможностью управления горизонтальным положением вертолета от 4-х позиционного переключателя, установленного на РЦШ;

> **9.**  Режим «Стабилизация Нрв» ‑ автоматическая стабилизация геометрической высоты, с возможностью изменения геометрической высоты от 2-х позиционного переключателя, установленного на РОШ;

> **10.** Режим «Стабилизация малых скоростей» ‑ стабилизация малых поступательных скоростей вертолета, с возможностью управления малыми скоростями вертолета от 4-х позиционного переключателя, установленного на РЦШ;

> **11.** Режим «Уход» ‑ уход с зависания с набором высоты и скорости на высоту Нрв=100м, скорость Vпр=120 км/ч.

> **12.** Режим «Работа с грузом на внешней подвеске» - автоматическое устранение колебаний груза на внешней подвеске.

> _При наличии или дополнительной установке системы инструментальной посадки ILS и взаимодействии с ней ПКВ-8, будет обеспечено выполнение следующего режима:_

> **13.** Режим «Инструментальная посадка» ‑ автоматическая стабилизация вертолета на траектории захода на посадку по радиомаякам ILS до высоты принятия решения.

> По желанию заказчика на объект может быть установлен **навигационный вычислитель ПВН**, разработки нашего предприятия, при этом имеется возможность выполнения следующих функций:

> **14.** Режим «Маршрут» - стабилизация вертолета на траектории маршрута, по сигналам, сформированным ПВН.

> **15.** Режим «Заход» - стабилизация вертолета на траектории захода на посадку, сформированную ПВН, с последующим зависанием над конечной точкой траектории посадки.

> **16.** Режим «Висение» - стабилизация точки висения вертолета в горизонтальной плоскости по комплексированной информации сигналов СНС и датчиков перегрузок.

> Сравнение выполняемых функций автопилота АП-34Б и пилотажного комплекса вертолета ПКВ-8 наглядно представлено в таблице приложения 1.

> **Пульт – вычислитель навигационный ПВН** способен управлять рядом специализированных систем, например:

> - радиотехническая система посадки NAV-4000

> - система DME-4000;

> - доплеровская навигационная система CMA-2012 и др.

> При его использовании дополнительно появляется возможность работы с системами:

> - раннего предупреждения близости земли СРПБЗ;

> - аппаратурой приема дифференцированных данных АПДД и др.

> ПВН также обеспечивает:

> - непрерывное автоматическое определение и индикацию текущих координат;

> - местоположения вертолета путем комплексной обработки информации от встроенного приемника сигналов группировок ГЛОНАСС/GPS и взаимодействующих датчиков из состава КБО. Разрабатывается модификация с приемником ГЛОНАСС/GPS/GALILEO;

> - коррекцию координат вертолета по информации радиотехнических систем;

> - работу с аэронавигационной базой данных;

> - формирование траектории для выполнения следующих режимов:

> - полет по запрограммированному маршруту в режиме ЗК или ЛЗП;

> - полет по заданному путевому углу;

> - полет на выбранную навигационную точку;

> - полет по параллельным траекториям со смещением на выбранное расстояние относительно ЛЗП;

> - полет на ближайший аэродром;

> - разворот типа «Fly-By» или «Fly-Over»;

> - полет по линии соединяющей заданные точки;

> - выход на заданную точку с заданного направления.

> - выполнение полетов в районе аэродрома, включая маневрирование в зонах аэродромов по стандартным маршрутам SID, STAR и APPROACH;

> - выполнение неточного захода на посадку на аэродромы и посадочные площадки, не оборудованные радиотехническими средствами, в том числе методом зональной навигации по СНС;

> - выполнение полетов в режиме галсирования по встречно-параллельным курсам с заданным расстоянием между отрезками ЛЗП;

> - выполнение полетов на морские буровые установки \(МБУ\), морские суда \(МС\), вертолетные площадки, с обеспечением выхода на них с заданного направления, на заданной высоте, или по кратчайшему расстоянию;

> - формирование и выдачу управляющих сигналов в пилотажный комплекс и информационных сигналов в систему электронной индикации;

> - формирование, индикацию и выдачу во взаимодействующие системы команд для автоматической программной настройки радиотехнических систем навигации и посадки VOR, АРК, ILS;

> - формирование информации для обеспечения режима «Висение»;

> - формирование траектории точного захода на посадку по СНС с использованием дифференциальных поправок \(Опция\);

> - обеспечение управления режимами наземного обслуживания пилотажного комплекса и индикация результатов.

{% embed url="https://t.me/in51d3" %}

{% embed url="https://telegra.ph/DISS-06-24" %}

{% embed url="https://www.youtube.com/watch?v=h4OnBYrbCjY" %}



