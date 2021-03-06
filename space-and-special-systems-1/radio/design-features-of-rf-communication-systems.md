# НИР Особенности проектирования ВЧ линий связи

## 1 Введение

Во время прохождения учебной практики были получены навыки и знания, необходимы для проектирования ВЧ линий связи. В представленном далее отчете будут рассмотрены основные определения и описаны базовые концепции проектирования ВЧ систем, проведено сравнение архитектур ВЧ приемников, перечислены основные факторы системного уровня, которые необходимо учитывать при проектировании линии связи.

## 2 Конструкция трансивера

Трансивером называется система радиосвязи, состоящая из приёмника и передатчика. Передатчик преобразует информацию модулирующего сигнала в радиочастотный сигнал и передаёт его по каналу связи, обычно через антенну \(рис. 1\). ВЧ передатчик выполняет три основные функции: модуляция, преобразование частоты и усиление мощности. Типовой передатчик характеризуется тремя основными показателями: точность, излучаемый спектр и выходная мощность.

![&#x420;&#x438;&#x441;. 1. &#x421;&#x442;&#x440;&#x443;&#x43A;&#x442;&#x443;&#x440;&#x43D;&#x430;&#x44F; &#x441;&#x445;&#x435;&#x43C;&#x430; &#x43F;&#x440;&#x438;&#x451;&#x43C;&#x43D;&#x438;&#x43A;&#x430; &#x438; &#x43F;&#x435;&#x440;&#x435;&#x434;&#x430;&#x442;&#x447;&#x438;&#x43A;&#x430; &#x442;&#x440;&#x430;&#x43D;&#x441;&#x438;&#x432;&#x435;&#x440;&#x430;.](../../.gitbook/assets/image%20%28105%29.png)

  
В зависимости от стоимости, исполнения и мощности для проектирования ВЧ передатчика могут использоваться разные архитектуры. На выбор архитектуры передатчика влияет также и число коммуникационных стандартов, которые он должен поддерживать. Кроме того, большинство архитектур передатчика похожи на архитектуры приёмника, например, гетеродинные и гомодинные архитектуры.

Архитектуры передатчиков можно разбить на две основные категории:

· **Архитектура со смесителем**: к таким архитектурам относятся гетеродинные \(т.е. с двойным преобразованием\) и гомодинные \(т.е. с прямым преобразованием\) передатчики. Эти архитектуры могут работать со схемами модуляции с постоянной и непостоянной огибающей, и удобны для работы с несколькими стандартами.

· **Архитектуры с фазовой автоподстройкой частоты \(ФАПЧ\)**: эти архитектуры более компактны из-за отсутствия дискретных компонентов. Тем не менее, они менее пригодны для работы с несколькими стандартами, так как поддерживают только схемы модуляции с постоянной огибающей. Архитектуры на основе ФАПЧ обычно используют генератор, управляемый напряжением \(ГУН\). На рис. 2 показана типовая архитектура передатчика с ФАПЧ. В этом передатчике ФАПЧ поддерживает постоянную центральную частоту для точного управления передаваемым сигналом.

![&#x420;&#x438;&#x441;. 2. &#x410;&#x440;&#x445;&#x438;&#x442;&#x435;&#x43A;&#x442;&#x443;&#x440;&#x430; &#x441; &#x424;&#x410;&#x41F;&#x427;.](../../.gitbook/assets/image%20%28106%29.png)

##  3 Конструкция приёмника

Приёмником называется радиосистема, принимающая некоторую полосу частот и обрабатывающая её для восстановления переданной информации. Основными компонентами приёмника являются антенна, ВЧ интерфейс и демодулятор, в котором выполняется базовая обработка сигнала. Общая структура ВЧ приёмника показана на рис. 3.

![&#x420;&#x438;&#x441;. 3. &#x422;&#x438;&#x43F;&#x43E;&#x432;&#x430;&#x44F; &#x430;&#x440;&#x445;&#x438;&#x442;&#x435;&#x43A;&#x442;&#x443;&#x440;&#x430; &#x43F;&#x440;&#x438;&#x451;&#x43C;&#x43D;&#x438;&#x43A;&#x430;.](../../.gitbook/assets/image%20%2888%29.png)

###  3.1. Супергетеродинные приёмники

Супергетеродинный приёмник \(называемый также приёмником двойного преобразования\) состоит из двух смесителей, которые понижают частоту сигнала из ВЧ в ПЧ в первом каскаде и из ВЧ в модулирующий сигнал во втором каскаде \(рис. 4\). Для настройки на ВЧ канал и подавления зеркальных составляющих используется несколько фильтров с различными технологиями и характеристиками.

![&#x420;&#x438;&#x441;. 4. &#x422;&#x438;&#x43F;&#x43E;&#x432;&#x43E;&#x439; &#x441;&#x443;&#x43F;&#x435;&#x440;&#x433;&#x435;&#x442;&#x435;&#x440;&#x43E;&#x434;&#x438;&#x43D;&#x43D;&#x44B;&#x439; &#x43F;&#x440;&#x438;&#x451;&#x43C;&#x43D;&#x438;&#x43A;.](../../.gitbook/assets/image%20%2884%29.png)

### 3.2 Приёмники с низкой ПЧ

Для обеспечения поддержки различных схем модуляции современные разработчики используют в приёмниках архитектуру с низкой ПЧ \(рис. 5\). Эта архитектура отличается переносом ВЧ сигналов на очень низкие промежуточные частоты. С помощью аналого-цифрового преобразователя \(АЦП\) с высоким разрешением можно непосредственно преобразовать сигнал ПЧ в модулирующий сигнал, который затем подвергается базовой обработке сигнальным процессором.

![&#x420;&#x438;&#x441;. 5. &#x422;&#x438;&#x43F;&#x43E;&#x432;&#x43E;&#x439; &#x43F;&#x440;&#x438;&#x451;&#x43C;&#x43D;&#x438;&#x43A; &#x441; &#x43D;&#x438;&#x437;&#x43A;&#x43E;&#x439; &#x41F;&#x427;.](../../.gitbook/assets/image%20%2881%29.png)

### 3.3 Приёмники прямого преобразования

Приёмником прямого преобразования \(известным также, как приёмник с нулевой ПЧ\) называется гомодинный приёмник, который преобразует ВЧ сигнал непосредственно в модулирующий сигнал, содержащий исходную передаваемую информацию \(рис. 6\). При прямом преобразовании ВЧ сигнала в модулирующий сигнал, на выходе смесителя появляется постоянная составляющая. Эту постоянную составляющую \(называемую также постоянным смещением\) нужно подавить во избежание ухудшения чувствительности демодулятора.

![&#x420;&#x438;&#x441;. 6. &#x410;&#x440;&#x445;&#x438;&#x442;&#x435;&#x43A;&#x442;&#x443;&#x440;&#x430; &#x442;&#x438;&#x43F;&#x43E;&#x432;&#x43E;&#x433;&#x43E; &#x433;&#x43E;&#x43C;&#x43E;&#x434;&#x438;&#x43D;&#x43D;&#x43E;&#x433;&#x43E; &#x43F;&#x440;&#x438;&#x451;&#x43C;&#x43D;&#x438;&#x43A;&#x430; &#x43F;&#x440;&#x44F;&#x43C;&#x43E;&#x433;&#x43E; &#x43F;&#x440;&#x435;&#x43E;&#x431;&#x440;&#x430;&#x437;&#x43E;&#x432;&#x430;&#x43D;&#x438;&#x44F;.](../../.gitbook/assets/image%20%28101%29.png)

### 3.4 Приёмники с субдискретизацией

Приёмник с субдискретизацией имеет гомодинную архитектуру, которая выполняет преобразование в ПЧ и затем субдискретизацию, что упрощает применение высокоскоростной цифровой обработки сигнала. В этой архитектуре используются два цифровых смесителя, которые предотвращают возникновение дополнительных шумов и искажений. Преобразование частоты не создаёт постоянной составляющей и шума 1/f. Оцифровка несущей частоты реализуется интегральными схемами структуры КМОП. Общая схема архитектуры приёмника с субдискретизацией показана на рис. 7.

![&#x420;&#x438;&#x441;. 7. &#x422;&#x438;&#x43F;&#x43E;&#x432;&#x430;&#x44F; &#x430;&#x440;&#x445;&#x438;&#x442;&#x435;&#x43A;&#x442;&#x443;&#x440;&#x430; &#x43F;&#x440;&#x438;&#x451;&#x43C;&#x43D;&#x438;&#x43A;&#x430; &#x441; &#x441;&#x443;&#x431;&#x434;&#x438;&#x441;&#x43A;&#x440;&#x435;&#x442;&#x438;&#x437;&#x430;&#x446;&#x438;&#x435;&#x439;](../../.gitbook/assets/image%20%2879%29.png)

Частота дискретизации Fд должна как минимум в два раза превышать частоту несущей.

### 3.5 Сравнение архитектур приёмников

В таблице 1, представлено сравнение самых распространённых архитектур приёмников.

Таблица 1- Сравнение широко распространённых архитектур приёмников.

![](../../.gitbook/assets/image%20%2883%29.png)

## 4 Конструктивные особенности системного уровня

При проектировании системы радиосвязи, должны учитываться различные факторы системного уровня.

### 4.1 Показатели качества

Показателями качества систем радиосвязи являются математические критерии, используемые для оценки производительности системы или некоторых её функциональных блоков. Среди этих показателей необходимо упомянуть следующие:

**· Коэффициент шума \(Кш\):** описывает уровень шума, создаваемого системой радиосвязи.

**· Отношение сигнала к шуму \(С/Ш\):** отношение сигнала к уровню шумов, созданных системой радиосвязи.

**· Относительный уровень мощности в соседнем канале \(ACPR\):** отношение общей мощности соседнего канала \(интермодуляционная составляющая\) к мощности основного канала \(полезный сигнал\).

**· Усиление:** отношение выходной мощности или амплитуды к входной мощности или амплитуде для данного устройства.

**· Точка пересечения по интермодуляционным составляющим 3-го порядка:** показатель нелинейности устройств, таких как усилители и смесители, а также коммуникационных систем.

· **Точка компрессии на 1 дБ:** точка, в которой усиление снижается на 1 дБ.

Некоторые устройства, например, усилители, имеют собственные показатели качества \(например, КПД, выходной динамический диапазон и устойчивость\). В следующем разделе будут представлены некоторые показатели качества системного уровня и описано, как они соотносятся с различными каскадами системы связи.

### 4.2 Уравнения дальности

Канал связи включает весь коммуникационный тракт от источника информации через кодеры и модуляторы передатчика, через канал до приёмника. Для определения бюджета канала, необходимого для надежной работы системы связи без деградации характеристик канала, нужно учитывать множество параметров. В число этих параметров входит и уравнение дальности, связывающее принимаемую мощность с расстоянием между приёмником и передатчиком.

Оценка фундаментального соотношения мощностей для приёмника и передатчика начинается обычно с предположения о всенаправленном источнике ВЧ сигнала, равномерно передающим сигнал в телесный угол 4π стерадиан. Плотность мощности на расстоянии d определяется функцией p\(d\), а мощность, извлекаемая приёмной антенной, описывается выражением Pr, где Aer представляет собой сечение поглощения \(эффективную площадь\) приёмной антенны. Концепция уравнения дальности проиллюстрирована рис. 8.

![&#x420;&#x438;&#x441;. 8. &#x423;&#x440;&#x430;&#x432;&#x43D;&#x435;&#x43D;&#x438;&#x435; &#x434;&#x430;&#x43B;&#x44C;&#x43D;&#x43E;&#x441;&#x442;&#x438; &#x432;&#x44B;&#x440;&#x430;&#x436;&#x430;&#x435;&#x442; &#x437;&#x430;&#x432;&#x438;&#x441;&#x438;&#x43C;&#x43E;&#x441;&#x442;&#x44C; &#x43F;&#x440;&#x438;&#x43D;&#x438;&#x43C;&#x430;&#x435;&#x43C;&#x43E;&#x439; &#x43C;&#x43E;&#x449;&#x43D;&#x43E;&#x441;&#x442;&#x438; &#x43E;&#x442; &#x440;&#x430;&#x441;&#x441;&#x442;&#x43E;&#x44F;&#x43D;&#x438;&#x44F;.](../../.gitbook/assets/image%20%2896%29.png)

### 4.3 Бюджет канала

Как правило, система радиосвязи состоит из передатчика, приёмника и канала передачи \(например, для радиосвязи это свободное пространство\). Если существует прямой путь между приёмником и передатчиком, он называется линией прямой видимости \(рис. 9\).

Анализ бюджета радиоканала призван ответить на следующие вопросы: «Какой длины может быть канал»? и «Какова его пропускная способность»? На самом деле на характеристики радиосистемы влияет несколько факторов. Эти факторы включают выходную мощность, коэффициент усиления антенны, чувствительность приёмника, условия окружающей среды \(например, наличие снега, дождя и особенности рельефа\) и многие другие. Бюджет канала определяется тремя основными компонентами \(уравнение 1\):

| Принимаемая мощность \(дБм\) = Передаваемая мощность \(дБм\) + Усиление \(дБ\) – Потери \(дБ\) |
| :--- |


Усиление складывается, в основном, из усиления антенны и усилителя, тогда как потери состоят из потерь в свободном пространстве, потерь рассогласования, потерь, связанных с условиями окружающей среды \(например, дождь и снег\), и газового поглощения \(например, облака и туман\).

В каналах с прямой видимостью основной причиной затухания излучаемого сигнала являются потери в свободном пространстве \(FSPL\). Они рассчитываются по уравнению 2:

![](../../.gitbook/assets/image%20%2898%29.png)

где    d – расстояние между приёмником и передатчиком в метрах,

f – частота передаваемого сигнала в Гц,

c – скорость света в вакууме \(≈ 299 792 458 м/с\).

![&#x420;&#x438;&#x441;. 9. &#x422;&#x438;&#x43F;&#x43E;&#x432;&#x43E;&#x439; &#x43A;&#x430;&#x43D;&#x430;&#x43B; &#x441;&#x432;&#x44F;&#x437;&#x438;.](../../.gitbook/assets/image%20%2899%29.png)

Канал по линии прямой видимости часто подвержен многолучевому распространению и затуханию. Многолучевое распространение возникает в том случае, если сигнал проходит разными маршрутами и интерферирует в приёмнике. В некоторых случаях сигналы достигают приёмника в противофазе и подавляют друг друга. Затуханием называется подавление передаваемого сигнала случайного характера. Оно может возникать в результате многолучевого распространения. Для оценки пределов затухания, необходимых для снижения влияния многолучевого распространения, используются такие модели затухания, как модели Накагами и Рэлея. Из-за многолучевого распространения ослабление сигнала может превышать 30 дБ. Таким образом, для преодоления результирующих потерь нужно предусмотреть существенный запас по затуханию \(уравнение 3\).

| Запас канала = Принимаемая мощность – Чувствительность приёмника |
| :---: |


Максимальная пропускная способность, достижимая в данном канале передачи, в общем случае связана с коэффициентом битовых ошибок \(BER\), который можно получить в приёмнике. BER тесно связан с отношением сигнала к шуму \(С/Ш\), которое определяется способом модуляции \(уравнение 4\).

| С/Ш = Принимаемая мощность – Шум канала |
| :---: |


### 4.4 Чувствительность и избирательность

Чувствительность и избирательность являются основными характеристиками любого радиоприёмника, поскольку они определяют способность приёмника регистрировать требуемый уровень полезных сигналов.

#### **Чувствительность**

Общепринятые методы оценки чувствительности приёмника предполагают, что фактором, ограничивающим его чувствительность, является собственный и внешний шум, а не доступный уровень усиления, который можно применить к сигналу. По этой причине для определения чувствительности приёмника используются многие показатели качества. Среди них стоит упомянуть следующие:

· С/Ш;

· Отношения полного сигнала к полному уровню помех \(SINAD\);

· Шум-фактор;

· Коэффициент шума;

· Отношение несущей к шуму \(Н/Ш\);

· Минимально различимый сигнал \(МРС\);

· Коэффициент битовых ошибок \(BER\).

В общем случае чувствительность определяется как уровень сигнала, необходимый для достижения некоторого качества принимаемой информации. В частности, она определяется уровнем мощности, необходимым для достижения требуемого отношения С/Ш.

#### **Избирательность**

Избирательность является важным параметром, характеризующим возможность радиоприёмника принимать только полезные сигналы. В зависимости от архитектуры приёмника, избирательность подразумевает способность выбора одной полосы частот или нескольких приёмных каналов. Кроме того, выбранная полоса может быть постоянной или переменной, а также узкой или широкой. В основном, избирательность достигается за счёт применения ВЧ и ПЧ фильтров.

Тип передачи, для которого будет использован приёмник, определяет необходимую полосу пропускания, соответствующую избирательность и тип используемого фильтра. Например, избирательность определяет, насколько будут подавляться внеполосные сигналы, но никак не влияет на внутриполосные помехи.

### 4.5 Подавление зеркальных частот

В гетеродинных приёмниках ВЧ сигнал преобразуется в более низкую ненулевую промежуточную частоту, на которой выполняется обработка сигнала. Как показано на рис. 10, такой архитектуре присуща врожденная проблема, которая называется зеркальной частотой. Она возникает при смешивании полезного сигнала с другим сигналом, расположенным по другую сторону от частоты гетеродина с такой же отстройкой от неё. Зеркальная частота может значительно искажать полезный сигнал, особенно, если её мощность превышает мощность полезного сигнала.

Для решения этой проблемы в приёмный тракт до каскада смешения можно добавить фильтр, подавляющий зеркальную частоту. Кроме того, проблему зеркальной частоты можно решить, использовав специальную схему смесителя, такую как Хартли или Уивера.

![&#x420;&#x438;&#x441;. 10. &#x420;&#x430;&#x431;&#x43E;&#x442;&#x430; &#x441;&#x43C;&#x435;&#x441;&#x438;&#x442;&#x435;&#x43B;&#x44F; &#x432; &#x442;&#x438;&#x43F;&#x43E;&#x432;&#x43E;&#x43C; &#x433;&#x435;&#x442;&#x435;&#x440;&#x43E;&#x434;&#x438;&#x43D;&#x43D;&#x43E;&#x43C; &#x43F;&#x440;&#x438;&#x451;&#x43C;&#x43D;&#x438;&#x43A;&#x435;.](../../.gitbook/assets/image%20%2885%29.png)

### 4.6 Фазовый шум

Фазовый шум описывает случайные флуктуации частоты гетеродина. Когда эти флуктуации воздействуют на смеситель, с обеих сторон от несущей частоты возникают боковые полосы шума \(рис. 11а\). Боковые полосы шума обычно измеряются в дБн/Гц при заданной отстройке от частоты несущей \(рис. 11б\). Фазовый шум является источником паразитных сигналов, создаваемых им в полосе пропускания. Кроме того, он создаёт серьёзные проблемы в системах OFDM \(мультиплексирование с ортогональным делением частот\), где он нарушает ортогональность сигналов.

#### Отстройка от несущей

![&#x420;&#x438;&#x441;. 11. &#x424;&#x430;&#x437;&#x43E;&#x432;&#x44B;&#x439; &#x448;&#x443;&#x43C;. ](../../.gitbook/assets/image%20%28104%29.png)

Отображение фазового шума на экране анализатора спектра \(а\). Две боковые полосы фазового шума вокруг несущей частоты \(б\).

### 4.7 Постоянное смещение

Постоянное смещение проявляет себя в основном в схемах с прямым преобразованием, где нежелательный сильный сигнал может преобразоваться вместе с полезным сигналом прямо в демодулированный сигнал. Этот эффект называется «самосмешением». В этом случае большой уровень преобразованного нежелательного сигнала создаёт сильные помехи полезному сигналу.

Эффект постоянного смещения может порождаться тремя основными механизмами:

**1. Утечка частоты гетеродина.** Плохая развязка между гетеродином и малошумящим усилителем, особенно на кремниевых подложках, может привести к утечке сигнала гетеродина через ВЧ порт и его самосмешению с основным сигналом гетеродина. Ситуация становится ещё хуже, если прежде чем достичь ВЧ входа смесителя, сигнал гетеродина усиливается малошумящим усилителем.

**2. Переизлучение гетеродина.** В некоторых случаях сигнал гетеродина может переизлучаться антенной и отражаться от препятствий. Переизлучённый сигнал принимается вместе с полезными сигналами и смешивается с ними. В этом случае переизлучённый сигнал влияет не только на локального пользователя, но и на других пользователей, использующих те же каналы.

**3. Сильная внутриполосная помеха.** Сильная близлежащая помеха тоже может генерировать большое постоянное смещение, которое может исказить принимаемую информацию.

Кроме того, постоянное смещение может порождаться внутриполосными нелинейными составляющими второго порядка, генерируемыми ВЧ интерфейсом приёмника.

## 5 Нелинейное поведение ВЧ системы

Нелинейная ВЧ система генерирует выходной сигнал, который не является линейной функцией входных сигналов \(рис. 12\). Предположим, что: Vвх = A cos\(ω1t\) + A cos\(ω2t\).

![&#x420;&#x438;&#x441;. 12. &#x421;&#x445;&#x435;&#x43C;&#x430; &#x43D;&#x435;&#x43B;&#x438;&#x43D;&#x435;&#x439;&#x43D;&#x43E;&#x439; &#x441;&#x438;&#x441;&#x442;&#x435;&#x43C;&#x44B;.](../../.gitbook/assets/image%20%2893%29.png)

Результирующий выходной сигнал будет определяться уравнением 5:

![](../../.gitbook/assets/image%20%2891%29.png)

### 5.1 Интермодуляционные искажения

Интермодуляционные искажения – это эффект, присутствующий в нелинейных коммуникационных системах. Интермодуляционные искажения возникают, когда два и более сигналов занимают один и тот же канал передачи. На выходе смесителя нелинейность проявляет себя в виде двойных составляющих основной частоты, которые называются интермодуляционными искажениями. Такие нежелательные сигналы могут вызывать блокировку приёмного тракта.

На рис. 13 показаны интермодуляционные составляющие первого, второго и третьего порядка для двухтональных несущих. Частоты интермодуляционных составляющих двухтональных несущих можно рассчитать по формуле 6:

![](../../.gitbook/assets/image%20%2876%29.png)

![&#x420;&#x438;&#x441;. 13. &#x418;&#x43D;&#x442;&#x435;&#x440;&#x43C;&#x43E;&#x434;&#x443;&#x43B;&#x44F;&#x446;&#x438;&#x43E;&#x43D;&#x43D;&#x44B;&#x435; &#x441;&#x43E;&#x441;&#x442;&#x430;&#x432;&#x43B;&#x44F;&#x44E;&#x449;&#x438;&#x435;.](../../.gitbook/assets/image%20%2877%29.png)

### 5.2 Коэффициент шума

Коэффициентом шума определенного устройства называется мера ухудшения отношения С/Ш, вызванного этим устройством в тракте передачи и/или приёма. Его можно рассчитать по шум-фактору F с помощью уравнения 7.

Шум-фактор представляет собой отношение входного С/Ш к выходному С/Ш. Коэффициент шума равен разности между входным и выходным С/Ш, как показано в уравнении 8.

![](../../.gitbook/assets/image%20%2894%29.png)

Каждое ВЧ устройство характеризуется коэффициентом шума, поскольку его электрическая схема добавляет шум к полезному сигналу и может иметь собственное усиление \(рис. 14\). При каскадном включении нескольких компонентов результирующий коэффициент шума рассчитывается по формуле Фриза, представленной уравнением 9.

![&#x420;&#x438;&#x441;. 14. &#x41A;&#x43E;&#x44D;&#x444;&#x444;&#x438;&#x446;&#x438;&#x435;&#x43D;&#x442; &#x448;&#x443;&#x43C;&#x430; &#x434;&#x43B;&#x44F; &#x43D;&#x435;&#x441;&#x43A;&#x43E;&#x43B;&#x44C;&#x43A;&#x438;&#x445; &#x43F;&#x43E;&#x441;&#x43B;&#x435;&#x434;&#x43E;&#x432;&#x430;&#x442;&#x435;&#x43B;&#x44C;&#x43D;&#x44B;&#x445; &#x43A;&#x430;&#x441;&#x43A;&#x430;&#x434;&#x43E;&#x432; &#x432; &#x412;&#x427; &#x442;&#x440;&#x430;&#x43A;&#x442;&#x435;.](../../.gitbook/assets/image%20%2892%29.png)

![](../../.gitbook/assets/image%20%2889%29.png)

### 5.3 Внутриполосные и внеполосные помехи и блокирующие сигналы

Каждый передатчик создаёт в каждом приёмнике некоторые помехи. С этими помехами нужно бороться, чтобы предотвратить потерю чувствительности приёмников.

**· Внутриполосные помехи:** порождаются перекрытием частот приёмника и передатчика. Оба устройства могут работать совершенно нормально, но только если они разнесены на относительно большое расстояние, когда мешающие сигналы передатчика достаточно сильно затухают и не подавляют полезные сигналы. 

**· Внеполосные помехи:** этот тип помех не связан с перекрытием частот приёмника и передатчика. Такие помехи порождаются, во-первых, внутриполосными излучениями, принимаемыми из внеполосного диапазона из-за несовершенства фильтров и аттенюаторов приёмника, и во-вторых, внеполосными излучениями, принимаемыми внутри полосы, из-за несовершенства и нелинейности передатчика.

### 5.4 Соседние каналы

Соседними каналами называются ближайшие частотные диапазоны, расположенные выше и ниже по частоте относительно рассматриваемого канала \(рис. 15\). На эти каналы может влиять расширение спектра, а также гармонические и интермодуляционные составляющие, нарушая передачу других пользователей. Поэтому этим побочным эффектам нужно уделять особое внимание.

![&#x420;&#x438;&#x441;. 15. &#x412;&#x435;&#x440;&#x445;&#x43D;&#x438;&#x439; &#x438; &#x43D;&#x438;&#x436;&#x43D;&#x438;&#x439; &#x441;&#x43E;&#x441;&#x435;&#x434;&#x43D;&#x438;&#x435; &#x43A;&#x430;&#x43D;&#x430;&#x43B;&#x44B;.](../../.gitbook/assets/image%20%2880%29.png)

Относительным уровнем мощности в соседнем канале \(ACPR\) называется уровень помех или мощности в соседнем частотном канале. Обычно ACPR определяется, как отношение средней мощности в соседнем частотном канале \(или при определённой отстройке\) к средней мощности в передающем частотном канале. ACPR каскадного включения можно рассчитать по следующей формуле:

![](../../.gitbook/assets/image%20%28108%29.png)

Концепция ACPR иллюстрируется рис. 16 \(а\) и \(б\).

![&#x420;&#x438;&#x441;. 16.](../../.gitbook/assets/image%20%2897%29.png)

Рис. 16. Расширение спектра, вызванное усилением сигнала OFDM \(а\), внутриполосная и внеполосная интермодуляция: ACPR равен разности между мощностью основного внутриполосного тона и интермодуляционными составляющими в соседнем канале \(б\).

### 5.5 Компрессия усиления

Компрессией усиления называется явление, проявляющееся в нелинейных ВЧ системах. В линейных системах любое изменение входной мощности вызывает пропорциональное изменение выходной мощности. Однако в нелинейных системах выходная мощность линейно зависит от входной мощности только до определенного предела – насыщения. В этой точке выходная мощность перестаёт линейно зависеть от входной.

#### **Усиление**

Усилением называется отношение выходной мощности или амплитуды к входной. Обычно оно обозначается буквой G и измеряется в децибелах.

![](../../.gitbook/assets/image%20%2890%29.png)

Усиление нескольких каскадов равно линейной сумме усилений каждого каскада:

![](../../.gitbook/assets/image%20%2886%29.png)

#### **Точка пересечения по интермодуляционным составляющим третьего порядка**

Точка пересечения по интермодуляционным составляющим третьего порядка \(обычно обозначается как **IP3** или **TOI**\) является характеристикой нелинейных устройств и систем. Она равна отношению нелинейных составляющих третьего порядка к линейно усиленному сигналу. Точка пересечения по интермодуляционным составляющим второго порядка \(IP2\) подобна IP3, но учитывает составляющие второго порядка. При каскадном включении IP3 можно рассчитать по формуле 13:

![](../../.gitbook/assets/image%20%2887%29.png)

![&#x420;&#x438;&#x441;. 17. &#x422;&#x43E;&#x447;&#x43A;&#x430; &#x43F;&#x435;&#x440;&#x435;&#x441;&#x435;&#x447;&#x435;&#x43D;&#x438;&#x44F; &#x43F;&#x43E; &#x438;&#x43D;&#x442;&#x435;&#x440;&#x43C;&#x43E;&#x434;&#x443;&#x43B;&#x44F;&#x446;&#x438;&#x43E;&#x43D;&#x43D;&#x44B;&#x43C; &#x441;&#x43E;&#x441;&#x442;&#x430;&#x432;&#x43B;&#x44F;&#x44E;&#x449;&#x438;&#x43C; &#x442;&#x440;&#x435;&#x442;&#x44C;&#x435;&#x433;&#x43E; &#x43F;&#x43E;&#x440;&#x44F;&#x434;&#x43A;&#x430;.](../../.gitbook/assets/image%20%28103%29.png)

**Точка компрессии на 1 дБ**

Обычно точка компрессии на 1 дБ обозначается как P1дБ и определяет входную мощность нелинейной системы, при которой усиление малого сигнала отклоняется от прямой линии и уменьшается на 1 дБ. Определение точки компрессии на 1 дБ нелинейной системы проиллюстрировано на рис. 17. Для нескольких каскадов P1дБ можно рассчитать рекурсивно по формуле 14.

![](../../.gitbook/assets/image%20%2882%29.png)

### 5.6 Динамический диапазон

Динамический диапазон \(ДД\) равен полезному диапазону сигнала, который приёмник может принять и корректно обработать. В общем случае динамический диапазон определяется как разность между точкой компрессии на 1 дБ и уровнем собственных шумов для данного каскада \(15\).

![](../../.gitbook/assets/image%20%2895%29.png)

## 6 Компромиссы при конструировании передатчика

### 6.1 Отношение сигнала к шуму

Как следует из уравнения 16 и показано на рис. 18, отношение сигнала к шуму \(обозначаемое как С/Ш\) определяет отношение уровня полезного сигнала к уровню собственных шумов. Оно равно отношению интегральной мощности сигнала к интегральной мощности шума в интересующей нас полосе.

![](../../.gitbook/assets/image%20%2878%29.png)

![&#x420;&#x438;&#x441;. 18. &#x41E;&#x442;&#x43D;&#x43E;&#x448;&#x435;&#x43D;&#x438;&#x435; &#x441;&#x438;&#x433;&#x43D;&#x430;&#x43B;&#x430; &#x43A; &#x448;&#x443;&#x43C;&#x443;.](../../.gitbook/assets/image%20%28107%29.png)

### 6.2 Планирование частот

В абонентском терминале передатчик использует для передачи сигналов канал, называемый восходящим, а приёмник использует другой канал, называемый нисходящим \(или прямым каналом\). Полосы восходящего и нисходящего каналов как правило равны между собой \(рис. 19\). Для организации связи нужно выбрать пару из нисходящего и восходящего каналов. Для каждого коммуникационного стандарта определяется полоса восходящего и нисходящего канала, разнесение каналов и полоса разделения. Например, в сотовой связи CDMA17 восходящие и нисходящие каналы занимают диапазоны 824 – 849 МГц и 869 – 894 МГц соответственно, причем полоса разделения равна 20 МГц, а разнесение каналов составляет 30 кГц.

![&#x420;&#x438;&#x441;. 19. &#x420;&#x430;&#x441;&#x43F;&#x440;&#x435;&#x434;&#x435;&#x43B;&#x435;&#x43D;&#x438;&#x435; &#x432;&#x43E;&#x441;&#x445;&#x43E;&#x434;&#x44F;&#x449;&#x438;&#x445; &#x438; &#x43D;&#x438;&#x441;&#x445;&#x43E;&#x434;&#x44F;&#x449;&#x438;&#x445; &#x43A;&#x430;&#x43D;&#x430;&#x43B;&#x43E;&#x432; &#x438; &#x43F;&#x43E;&#x43B;&#x43E;&#x441;&#x44B; &#x440;&#x430;&#x437;&#x434;&#x435;&#x43B;&#x435;&#x43D;&#x438;&#x44F;.](../../.gitbook/assets/image%20%28102%29.png)

Распределение частот в ВЧ устройствах оказывает решающее влияние на планирование частот, которое заключается в оптимальном выборе промежуточных частот, при которых паразитные составляющие в супергетеродинных приёмниках будут минимальны. Эта проблема дополнительно усложняется, если трансивер использует полнодуплексную схему, в которой сигналы приёмника и передатчика могут создавать гармоники и оказывать отрицательное воздействие друг на друга.

Для минимизации искажений выбор промежуточной частоты \(ПЧ\) должен опираться на два основных правила:

**· Правило 1**. Если приёмник и передатчик используют общий гетеродин, то для улучшения характеристик ПЧпр и ПЧпер должны выбираться согласно уравнению 17:

![](../../.gitbook/assets/image%20%28110%29.png)

**· Правило 2**. Для предотвращения возникновения внутриполосных помех в приёмнике, ПЧпр должно соответствовать уравнению 18:

![](../../.gitbook/assets/image%20%28100%29.png)

Для проверки частотного плана нужно выполнить анализ паразитных сигналов и измерить гармоники, попадающие в полезный диапазон. Для сигналов основной частоты этот расчёт выполняется достаточно просто:

![](../../.gitbook/assets/image%20%28109%29.png)

где A и B – сигналы основной частоты.

## Заключение

Во время прохождения учебной практики были рассмотрены конструкции цифровых приемников и передатчиков - архитектуры цифровых супергетеродинных приемников, приемников с низкой ПЧ, прямого преобразования и приемников с субдискретизацией. Были изучены конструктивные особенности проектирования цифровых радиоприемников, рассмотрено нелинейное поведение высокочастотных систем, а также выявлены компромиссы, которые нужно учитывать при ведении разработки такой системы.  


**Литература**

1. TRANZEO Wireless Technologies, “Анализ бюджета радиоканала: как рассчитать бюджет канала для радиосети”, официальный документ, 2010 г.

2. П. Е. Аллен, Д. Р. Холберг, “Проектирование аналоговых КМОП цепей”, второе издание, Oxford University Press, 2002 г.

3. Джеффери А. Велдон, “Высокопроизводительные КМОП передатчики для радиосвязи”, докторская диссертация, Калифорнийский университет Беркли, 2005 г.

4. Р. Свитек, С. Раман, “Постоянные смещения в приёмниках прямого преобразования: измерение характеристик и обсуждение”, IEEE Microwave Magazine, сс. 76-86, сентябрь 2005 г.

