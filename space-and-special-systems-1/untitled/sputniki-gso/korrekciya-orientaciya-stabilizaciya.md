# Коррекция, ориентация, стабилизация

#### Подсистема ориентации и стабилизации \(СОС\): назначение подсистемы, состав оборудования и принцип работы СОС. <a id="&#x41F;&#x43E;&#x434;&#x441;&#x438;&#x441;&#x442;&#x435;&#x43C;&#x430;-&#x43E;&#x440;&#x438;&#x435;&#x43D;&#x442;&#x430;&#x446;&#x438;&#x438;-&#x438;-&#x441;&#x442;&#x430;&#x431;&#x438;&#x43B;&#x438;&#x437;&#x430;&#x446;&#x438;&#x438;-(&#x421;&#x41E;&#x421;):-&#x43D;&#x430;&#x437;&#x43D;&#x430;&#x447;&#x435;&#x43D;&#x438;&#x435;-&#x43F;&#x43E;&#x434;&#x441;&#x438;&#x441;&#x442;&#x435;&#x43C;&#x44B;,-&#x441;&#x43E;&#x441;&#x442;&#x430;&#x432;-&#x43E;&#x431;&#x43E;&#x440;&#x443;&#x434;&#x43E;&#x432;&#x430;&#x43D;&#x438;&#x44F;-&#x438;-&#x43F;&#x440;&#x438;&#x43D;&#x446;&#x438;&#x43F;-&#x440;&#x430;&#x431;&#x43E;&#x442;&#x44B;-&#x421;&#x41E;&#x421;."></a>

**Система ориентации** служит для обеспечения определенного положения осей КА относительно заданного направления.

**Система стабилизации** – служит для перехода на другую орбиту или траекторию спуска, когда для сохранения положения осей КА работает основная двигательная установка.

Обычно системы ориентации и стабилизации объединяют, так как они используют одинаковые датчики.

**Система СОС:**

- успокоение спутника и начальная стабилизация с включением СОС перед режимом трехосной стабилизации

- трехосная стабилизация в орбитальных координатах с заданными требованиями в течении срока службы спутника

- ориентирует БС на солнце

- выполняет режим аппаратно – солнечной ориентации \(РАСО\) – режим живучести

**Приборы СОС:**

- приборы ориентации на Землю \(ПОЗ\)

- звездный прибор \(SED26\)

- прибор ориентации на Солнце \(ПОС\)

- датчик направления на Солнце \(ДНС\)

- малогабаритный блок измерения угловой скорости \(МБСИ\)

- электромеханический исполнительный орган \(ЭМИО\)

- привод БС

![](https://telegra.ph/file/3b15c4ae72620dabbc345.png)

**Прибор ориентации на Солнце**

Дает информацию об угловом положении Солнца в видимом диапазоне излучения. Относится к оптико-электронным приборам сканирующего типа. Формирует выходной сигнал о положении Солнца в системе координат прибора, путем сканирования поля обзора через щелевыми V-образными угловыми полями. Находится на панели Х.

**Прибор ориентации на Землю**

Положение спутника относительно центра Земли по тангажу и крену. ИК датчикю Резервирование – 2шт. \(Геофизика-Космос\)

**Прибор звездный ПЗВ**

Для определения ориентации относительно инерциальной геоцентрической системы координат на основе информации, получаемой из оптических сигналов, получаемых при наблюдении фрагментов звездного неба.

Состав: прибор в виде моноблока, оптический блок – приемный модуль с термоэлектрическим охлаждением, электронный блок обработки сигналов.

ПЗВ работает с помощью объектива и ПЗС матрицы 0,45-1 мкм.

Может использовать автономное обнаружение и автономное слежение.

Располагается на оси Х.

**Датчик направления на Солнце**

Оптико-электронный датчик, связан с посадочной плоскостью прибора. Измеряет угловое положение Солнца.

**Привод БС**

Нереверсивное вращение панелей БС относительно параллельной 0-Z платформы в соответствии с командами управления. Передача эл энергии с вращающихся панелей БС. Формирование и выдача управляющего телеметрического сигнала. \(Угол поворота и токи двигателя\).

**ЭМИО**

Для создания динамических управляющих моментов по 3м осям. 4 двигателя-маховика, один из них-резервный, управляется блоком автоматики.

**Режимы функционирования СОС**

1.       Режим успокоения

2.       Режим начальной ориентации на Солнце

3.       Режим ориентации на Землю

4.       Резервирование

5.       Режим трехосной стабилизации

6.       Режим аппаратной солнечной ориентации

#### Подсистема коррекции \(СК\): назначение подсистемы, состав оборудования и принцип работы СК <a id="&#x41F;&#x43E;&#x434;&#x441;&#x438;&#x441;&#x442;&#x435;&#x43C;&#x430;-&#x43A;&#x43E;&#x440;&#x440;&#x435;&#x43A;&#x446;&#x438;&#x438;-(&#x421;&#x41A;):-&#x43D;&#x430;&#x437;&#x43D;&#x430;&#x447;&#x435;&#x43D;&#x438;&#x435;-&#x43F;&#x43E;&#x434;&#x441;&#x438;&#x441;&#x442;&#x435;&#x43C;&#x44B;,-&#x441;&#x43E;&#x441;&#x442;&#x430;&#x432;-&#x43E;&#x431;&#x43E;&#x440;&#x443;&#x434;&#x43E;&#x432;&#x430;&#x43D;&#x438;&#x44F;-&#x438;-&#x43F;&#x440;&#x438;&#x43D;&#x446;&#x438;&#x43F;-&#x440;&#x430;&#x431;&#x43E;&#x442;&#x44B;-&#x421;&#x41A;"></a>

Используется для создания необходимой силы тяги для движения, посредством преобразования внутренней энергии топлива в кинетическую энергию струи энергии.

**Ионный двигатель –** малое потребление топлива, малая сила тяги. Ускорение в электростатическом поле и выброс заряженных частиц.

**Гидрозиновый двигатель –** рабочее тело с большой скоростью истекает из двигателя в соответствии с законом сохранения импульса, образуется сила, толкающая КА в другом направлении.

Для разгона можно использовать расширение газа – это тепловые ракетные двигатели.

**Функции**:

- приведение КА в установленную точку ГСО

- удержание в точке стояния в направлениях запад-восток и север – юг

- перевод из одной точки в другую

- увод на орбиту захоронения

- формирование управляющих моментов для ориентации КА

**Захоронение –** 300км выше ГСО

**Режимы**

- выведение

- подготовка двигателей ориентации

- подготовка двигателей коррекции

- проверочное включение двигателей коррекции

- дежурный режим

- режим коррекции и приведения

- режим коррекции удержания

- обеспечение режима живучести

**Состав**:

- блок коррекции \(БК\)

- двигатели блока ориентации \(ДБ\)

- блок хранения и подачи топлива \(БХП\)

- блок хранения ксенона \(БХК\)

- блок подачи ксенона \(БПК\)

- горловина проверочная

- горловина заправочная

- межблочные трубопроводы

- система преобразования и управления \(СПУ\)

- ПО

**Блок коррекции**

Ионный двигатель – для размещения и функционирования СПД, формирует требуемую тягу при коррекции.

**Устройство**: каждый блок состоит из одного модуля на базе стационарного плазменного двигателя СПД и двух модулей газораспределения.

**Двигательный блок ориентации**

Для обеспечения работы электротермокатолитических двигателей. Обеспечивающих формирование требуемых тяг при ориентации спутника.

**Блок подачи ксенона**

Для подачи ксенона с заданным давлением в двигатели блоков коррекции.

**Блок хранения и подачи**

Для хранения гидрозина массой до 30кг и подачи его в двигательные блоки. Выполнен по схеме с вытеснительной системой подачи топлива. **Бак –** емкость для хранения и подачи топлива, емкость для хранения вытесняющего газа.

**Система преобразования и управления**

Состоит из резервированных функциональных узлов, обеспечивающих работу 6-ти двигателей коррекции.

**Состав:**

- ИП анода

- стабилизатор накала катода

- поджигающее устройство

- стабилизатор анода

- ИП клапанов

- устройство коммутации

{% embed url="https://t.me/in51d3" %}



