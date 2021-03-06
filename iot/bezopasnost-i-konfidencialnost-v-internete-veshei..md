---
description: 'Текст @N3M351DA t.me/in51d3 и @beh1ndy0urback. December 04, 2019.'
---

# Безопасность и конфиденциальность в Интернете вещей.

Несмотря на то, что технический прогресс во многом облегчает жизнь человека, с другой стороны, он порождает новые угрозы. С учетом того, что IoT является важным элементом во многих сферах жизни - системы умного дома, умные города, здравоохранение, логистика, развлечения и т. д., эта статья посвящена ключевым областям этой технологии, рискам безопасности и соответствующим контрмерам, а также вопросам конфиденциальности и проблемам, которые могут возникнуть при условии, если процесс разработки не проходит должным образом. Несмотря на то, что невозможно охватить все аспекты IoT, в этой статье все же упоминаются некоторые из них: от эволюции IoT до встраиваемых систем, приложений, от проблем безопасности для каждой области в отдельности до проблем конфиденциальности. И, наконец, будет описаны наиболее часто применяемые технологии.

**Введение**

Термин IoT, впервые использованный Кевином Эштоном в 1999 году, обладает несколькими определениями, которые имеют более или менее одинаковые значения. В этом документе будет использоваться только один из них, который предоставляется IETF, поскольку «в концепцию интернета вещей могут входить разнообразные элементы - компьютеры, датчики, люди, исполнительные механизмы, холодильники, камеры, транспортные средства, мобильные телефоны, одежда, продукты питания, лекарства, книги и т. д.». Другими словами, «вещь» в данном контексте относится к «умным» вещам, окружающим нас повсюду. Умные вещи - это те предметы, которые имеют такие возможности, как агрегация и обработка данных, установка связи с другими предметами или пользователями. Тем не менее, одной из ключевых особенностей является наличие возможности установки контакта с окружающей средой с помощью датчиков.

Чтобы проиллюстрировать это, приведем пример подобной системы. В последнее время наиболее распространенными являются системы «умный дом» и системы здравоохранения, которые будут кратко упомянуты. С другой стороны, очевидно, что IoT не ограничевается только ими, диапазон довольно широк: от логистических систем до розничной торговли, промышленности, развлечений и т. д.

Охват областей применения этих технологий расширяется в геометрической прогрессии, особенно в последнее десятилетие, что также требует обсуждения вопросов безопасности и конфиденциальности. Тем не менее, общество еще не в полной мере осознает возможные проблемы и риски. Игнорирование этих проблем может привести к неожиданным и неприятным последствиям для людей, поскольку эти интеллектуальные устройства являются неотъемлемой частью повседневной жизни. Данная сфера не ограничивается частными лицами, организации так же могут также пострадать в результате недостаточной защиты данных или иных проблем безопасности. Таким образом, это может поставить под угрозу репутацию компании или лишить потребителей услуг. Итальянский бренд одежды Benetton может быть подходящим примером в подобном сценарии. В 2003 году компания была обвинена в том, что у нее существует стратегия, заключающаяся в отслеживании своих продуктов на пути к розничным продавцам с использованием RFID. Это было серьезным нарушением конфиденциальности клиентов, что позволило бы компании отслеживать их позже. Таким образом, эта ситуация вызвала бойкот Benetton его потребителями.

 В целом, IoT является растущей, расширяющейся сферой и, по-видимому, требует развития контрмер для решения возникающих проблем конфиденциальности. Правовые меры являются одним из подходов защиты прав потребителей, не препятствующим инновациям в любой области. Тем не менее, новые законы адаптируются намного медленнее, чем происходит совершенствование и обновление технологий. Поэтому новые возможности IoT всегда выходят за рамки действующего законодательства. В частности, отслеживание пользователей существует давно, но закон, связанный с этой проблемой, был только недавно Европейской комиссией в 2011 году.

**2. Рост и развитие Интернета вещей**

Несмотря на то, что Интернет вещей не является новой парадигмой, его особенности, технологии и инновации сохраняют свежесть и новизну. Каждый день тысячи новых устройств присоединяются к огромному семейству устройств по всему миру благодаря его возможностям и преимуществам. Благодаря исследованиям Gartner прогнозируемое количество устройств IoT достигнет примерно 20 миллиардов в 2020 году, где большая часть придется на частных потребителей. Не так просто даже приблизительно оценить масштаб в таком случае. Основная причина, по которой процесс прогнозирования становится сложным, заключается в том, что нет уверенности ни в отношении будущих устройств, ни в их безопасности. Таким образом, другой прогноз, который дает Cisco, заявляет, что количество устройств, подключенных к Интернету, возрастет до 50 миллиардов устройств в 2020 году.

RFID и беспроводные сенсорные сеть могут рассматриваться как два ключевых направления развития IoT. RFID играет огромную роль в IoT, будучи многообещающей и дешевой технологией. Основная проблема конфиденциальности, связанная с RFID, заключается в том, что она может позволить своим владельцам отслеживать отдельных лиц, как упоминалось в предыдущем примере Benetton. Такие метки очень удобно размещать внутри одежды или любого другого предмета без особых требований. Следующим ключевым направлением являются беспроводные сенсорные сети \(WSN\), которые имеют широкий диапазон размеров - от крошечных домашних датчиков размером в миллиметры до огромных, используемых в военных целях и прогнозировании погоды. Датчики собирают информацию в различных средах, таких как офисы, дома, человеческое тело, используя медицинские устройства, такие как измерители артериального давления и так далее.

**2.1. Преимущества и риски**

Нельзя отрицать, что IoT предоставляет пользователям устройства, которые способны радикально изменить ряд задач и сделать их решение гораздо удобнее и доступнее. Как упоминалось ранее, одно из революционных улучшений сделано в индустрии здравоохранения: теперь пациенты могут самостоятельно получать информацию о своем здоровье с помощью устройств IoT. Ключевой момент заключается в том, что пользователи не только обладают информацией, но и способны самостоятельно оказывать первую помощь. Инсулиновые помпы, которые регулярно проверяют уровень глюкозы в крови и снабжают организм инсулином, ремни кровяного давления - два известнейших примера этих устройств, которые используются вместе с приложениями, собирающими данные о состоянии здоровья людей и позволяющими им контролировать свое состояние. Кроме того, эти данные могут быть переданы в больницу для улучшения плана медицинского обслуживания благодаря наличию постоянных измерений. С другой стороны, невозможно слепо игнорировать риски безопасности этих приложений и систем, которые влияют на потребителей. Нарушение целостности данных, обмен статистикой с третьей стороной для различных целей - лишь несколько примеров возможных сценариев нарушения безопасности.

**3. Применение IoT**

Ранее упомянутых прогнозов о количестве устройств IoT, вероятно, достаточно, чтобы представить широту охвата. Хотя в этой статье невозможно перечислить все области применения IoT \(или даже большинство из них\), некоторые основные разделы будут упомянуты. Было бы целесообразно классифицировать эти области применения, чтобы иметь возможность дать краткое резюме. Для этой цели будет использована одна из самых популярных классификаций применения IoT устройств, предложенная Ацори, Иерой, Морабито:

 • Транспорт;

 • Здоровье;

 • Умный дом \(офис, здание и т. д.\);

 • Личное применение.

 Как показано, категории распределены по четырем основным областям, которые начинаются с транспортной системы \(включая логистику\). Все те области, которые упомянуты выше, несут в себе различные виды угроз безопасности и конфиденциальности для своего потребителя, что будет описано отдельно в будущем. Тем не менее, в общем виде, потоки данных для системы IoT показаны на рисунке 1.

![ &#x420;&#x438;&#x441;&#x443;&#x43D;&#x43E;&#x43A; 1.](https://telegra.ph/file/38ca2a73570d7691cf4ed.png)

\*\*\*\*

**3.1  Здоровье**

Теперь пришло время поближе познакомиться со всеми перечисленными разделами. Медицинская промышленность - очевидно, первоочередная и наиболее разумная область для внедрения IoT. Разделение системы IoT в области здравоохранения надвое - персональные устройства и приложения - поможет в дальнейшем яснее представлять себе общую картину. Изначально в качестве примеров устройств IoT у нас были инсулиновая помпа и ремни для измерения артериального давления. Дополнить эти примеры можно устройствами, использующими сенсоры - умные часы и некоторые иные технологии, которые помогают обнаруживать сигналы риска для жизни, например, прогнозировать сердечный приступ заранее.

Вторая часть - приложения из области здравоохранения - требуют более широкого и более сложного подхода, поскольку их основная цель - сбор данных о людях и получение некоторых выводов путем их оценки. Как правило, работу этих приложений можно описать в три этапа: идентификация, авторизация и сбор данных.

**3.1.1**. **Идентификация и авторизация.** Эта часть в первую очередь ориентирована на пациентов, для обеспечения лучшего обслуживания и точного лечения в соответствии с их диагнозом. При этом необходимо понимать, что суть данных устройств в том, чтобы обеспечить пациентов правильным лекарством, в правильной дозе и в нужное время, или избежать подмены новорожденного в больнице и так далее. С точки зрения персонала, идентификация и авторизация позволяют получать доступ к больничным вещам, продуктам и предотвращать их кражу.

**3.1.2**. **Сбор информации.** Хотя к сбору данных в отрасли здравоохранения можно подходить с разных точек зрения, связанных с управлением персоналом, управлением инвентарем больницы и т. д., приоритет составляют пациенты. Агрегирование, мониторинг и оценка данных о пациентах позволяют составить более точный план медицинского обслуживания для отдельных лиц и в целом для общества. Поскольку данные о состоянии здоровья играют ключевую роль в этом сценарии, эта часть привлекает злоумышленников, что делает систему базы данных уязвимой. С другой стороны, с точки зрения стоимости устройства IoT помогают сократить расходы по сравнению с обычными больничными расходами, позволяя пользователям самостоятельно узнавать диагноз.

**3.1.3**. **Датчики**. Последняя часть, которую необходимо упомянуть - это системы датчиков, которые используются, главным образом, в современных устройствах, которые предоставляют пользователям информацию в реальном времени с помощью беспроводной сенсорной сети. Основные области применения - это измерение температуры тела, артериального давления, пульса, глюкозы в крови и так далее. В зависимости от потребностей пользователя устройство поддерживает обмен данными с больницами или другими людьми.

**3.1  Логистика**

С увеличением количества грузов и необходимости в системе инвентаризации, логистика стала нуждаться в технологиях IoT. От автоматизированного управления складами и оборудованием до мониторинга сотрудников - устройства IoT позволяют оптимизировать всю систему и сократить потребность в рабочей силе. Помогая организации в современной и динамичной среде, IoT собирает и исследует сгенерированные данные, чтобы следовать современным тенденциям.

 **3.3. Умный дом**

Область, которая будет последней темой этого раздела - это системы IoT, которые работают в домах, офисах, зданиях. Другими словами - системы «Умный дом». Спрос на устройства для умного дома выходит за рамки других секторов, что позволяет ему эволюционировать гораздо быстрее. Согласно отчету IHS Markit, за период с 2010 по 2016 год количество заказанных умных бытовых приборов достигло 161 миллиарда. Большая часть этих покупок - личные домашние помощники, такие как Amazon Alexa, Google Home, интеллектуальные системы безопасности, интеллектуальные счетчики, связанные с домом, и так далее. Люди предпочитают управлять домашними системами на расстоянии и эту возможность дают системы дистанционного управления домом. В то время как существуют такие преимущества систем умного дома, как возможность управления системой отопления в зависимости от температуры в помещении и погоды, включения и выключения света в определенное время дня или выключения электронных устройств, когда они не используются \(для снижения потребления энергии\), необходимо учитывать растущие проблемы конфиденциальности и безопасности.

**4. Аутентификация и авторизация**

Поскольку следующие части документа будут посвящены главным образом вопросам безопасности и конфиденциальности, было бы разумно выделить аспекты, которые являются отправной точкой этих проблем. Это - аутентификация и авторизация.

**4.1. Аутентификация**

Говоря о любом смарт-объекте, который имеет возможность агрегировать данные, имеет связь с другими устройствами и подключается к сети, необходимо упомянуть аутентификацию. Хотя наиболее популярным методом является аутентификация с использованием идентификатора и пароля, применяются также цифровые подписи и, в последнее время, биометрические данные.

Для защиты конфиденциальности, обеспечения целостности и доступности данных аутентификация является наиболее важным элементом. Слабозащищенные учетные данные могут позволить злоумышленникам получить доступ к системе и действовать как легитимный пользователь, это будет обсуждаться более подробно в разделе безопасности.

Другой сложный момент, связанный с аутентификацией - неоднородная интеллектуальная система, в которую входят несколько устройств от разных поставщиков. Все эти устройства должны управляться одним приложением, но загвоздка кроется в управлении всеми ними поочередно. При переключении с одного устройства на другое не должно быть необходимости в повторной аутентификации, и решение этой проблемы является довольно болезненным вопросом для поставщиков. Вкратце, устройства IoT, в том числе системы «умного дома», должны быть максимально гибкими в данном плане и это должно быть учтено.

Управлять несколькими устройствами одним приложением недостаточно для решения всех проблем. Аутентификация только одного пользователя на устройстве приводит к ограничениям в системе. В большинстве случаев поставщику устройства также требуется доступ к устройству в случае возникновения каких-либо проблем. Устройства Apple могут быть примером в этой ситуации. Можно заметить, что поставщик также имеет доступ к системе, когда Apple отправляет обновление программного обеспечения на свои устройства. Другой пример - ключи от виртуальной машины. Владелец может поделиться своими ключами с нужным человеком. И в этом случае строгая аутентификация является ключевым фактором.

Успешная аутентификация пользователей не является достаточным решением, поскольку злоумышленник может использовать вторую конечную точку канала, что может привести к нарушению целостности данных. Здесь можно вспомнить пример больницы. Предположим, что устройство регулярно отправляет данные пользователя на сервер, а затем врач \(или любой, с кем пользователь хочет поделиться своими данными\) получает доступ к этим данным в любое время. Надо быть уверенным, что данные действительно поступают с нужного устройства, и между ними нет никаких третьих лиц. В целом, обеспечение полной безопасной аутентификации является сложной задачей для систем, игнорирование этой функции делает невозможной уверенность в том, что данные поступают с ожидаемого устройства или будут доставлены на верное устройство.

**4.2. Авторизация**

Одной аутентификации недостаточно для обеспечения безопасности системы IoT. Следующим шагом после аутентификации является авторизация, которая управляет доступом пользователей к функционалу системы или данным. Если мы хотим провести различие между этими двумя определениями, то можно сказать, что аутентификация спрашивает «Кто ты?», а авторизация отвечает на вопрос «Что ты имеешь право делать?». Только пользователь \(человек или приложение\) может выполнять некоторые действия после авторизации. Последствия небезопасной авторизации и аутентификации будут обсуждаться в разделе безопасности более широко.

В основном, механизм управления доступом применяется между системой и приложением, а не человеком, в то время как приложения управляют системой. Поскольку существует несколько моделей контроля доступа \(зависящих от структуры системы\), было бы разумно упомянуть некоторые из них и дать краткую информацию об их основной структуре. В качестве отправной точки можно взять избирательное управление доступом - DAC. В модели DAC привилегии пользователей выбираются администратором, он решает, кто может получить доступ и к каким функциям.

Следующей моделью является разграничение доступа на основе ролей \(RBAC\), где разрешения предоставляются ролям, а не отдельным пользователям.

И последняя модель, которая будет упомянута в этой статье - это управление доступом на основе атрибутов — ABAC — представляет собой более сложную модель, чем предыдущие, так как для доступа пользователя к данным или части системы может потребоваться более одного атрибута. Например, чтобы иметь доступ к системе начисления заработной платы компании, пользователь должен быть сотрудником и работать в отделе кадров. 

**5. Проблемы безопасности в IoT**

В предыдущих частях некоторые области применения, сценарии использования, некоторые преимущества и риски систем IoT давали представление о том, с какими системами мы имеем дело. Теперь основной вопрос - это проблемы безопасности и конфиденциальности, сложности, которые могут возникнуть и как с ними бороться.

В 2016 году одна из служб DNS - Dyn - подверглась DDoS-атаке которая нанесла ущерб системе и коммуникационным ресурсам. Причиной, которая сделала этот инцидент значимым, были источники атаки. Первоначально считалось, что источником атаки были обычные настольные компьютеры, но затем выяснилось, что источником атаки были в основном небольшие устройства, такие как камеры, принтеры, радионяни и так далее. Другим словом Интернет вещей. Эта ситуация показала, что безопасность необходима для любого сетевого устройства. Некоторые устройства, особенно те, которые связаны со здоровьем, обычно взаимодействуют с человеческим телом, и в случае нарушения безопасности это может привести к серьезным проблемам.

Одна из основных причин, по которой устройства IoT становятся более привлекательными для злоумышленников, заключается в том, что они в основном работают в облаках, что облегчает доступ к ним даже без доступа к самому устройству. Достаточно получить доступ к сетевым учетным записям, которые контролируют эти системы, чтобы управлять ими, производить обновления без ведома легитимного пользователя.

Все эти причины дают понять, что вопросы безопасности следует принимать во внимание. Со времени создания сетевых устройств было реализовано несколько механизмов безопасности, таких как брандмауэры и протоколы, которые обеспечивают защиту на высоком уровне. Их цель - предотвращение воздействия вредоносного программного обеспечения, а также его удаление в случае обнаружения. Однако единого решения для предотвращения всех этих вторжений нет.

Поскольку IoT - это огромное семейство технологий с сотнями разнообразных типов устройств, было бы рационально делить их на группы с точки зрения проблем безопасности.

**5.1  Проблемы безопасности в здравоохранении**

В последнее время количество атак, целью которых являются больницы, заметно увеличилось и большинство этих атак осуществляются вредоносными программами, называемыми «Ransomware», которые блокируют всю систему больницы и могут быть обезврежены только владельцем - самим злоумышленником. В то время как раньше атаки были нацелены на получение доступа к базам данных и управлению данными больницы, включая данные о персонале, пациентах и инвентаре, нынешняя тенденция несколько отличается: сейчас атаки в первую очередь направлены на нарушение работоспособности некоторых устройств, таких как томограф, рентгеновские аппараты и так далее.

Такие атаки подвергают опасности конфиденциальность, целостность и доступность. Поскольку злоумышленник имеет доступ к системе, он может управлять устройствами, наблюдать за назначением лекарств, может сделать онлайн-заказ или даже может использовать данные пациента для его шантажа.

**5.2. Проблемы безопасности в логистике**

Логистика включает в себя ряд приложений для повышения эффективности, которые открывают большую область для атак. Любой сценарий для логистики, такой как использование дороги, воды или воздуха, является довольно привлекательной площадкой для третьих лиц. Внесение изменений в данные, изменение города назначения, даты прибытия/отправления или создание совершенно нового судна, которое даже не существует, может привести к нежелательным последствиям.

**5.3. Вопросы безопасности в домах, офисах**

Нет сомнений в том, что одна из наиболее привлекательных областей системы IoT - это дома, офисы и среда, в которой люди проводят много времени. В то время как люди считают эти системы очень функциональными и полезными в повседневной жизни, нужно учитывать, что сетевые устройства имеют свои уязвимости. Например, наличие доступа к системе контроля энергопотребления может рассказать в какое время суток владелец находится на улице. Кроме того, как уже упоминалось в примере Dyn, такие устройства могут быть использованы в качестве атакующих \(DDoS\).

Наряду с системами умного дома, интересны и системы управления зданиями. Доступ к этим системам позволяет злоумышленникам контролировать их, и эти системы могут стать точками доступа к конфиденциальным данным, как, например, Target. Злоумышленники использовали отопительную систему, которую предоставляет Target, для доступа к системе компании, что позволило получить более 40 миллионов записей о пользователях, включая платежную информацию.

**5.4  Противодействие**

Разные среды, разные подходы и требования заставляют системы IoT использовать адаптивные решения безопасности для каждой структуры в отдельности. По этой причине, как упоминалось ранее, нет единого точного решения, хотя общие методы обеспечения безопасности все еще можно предложить.

**5.4.1. Установка программного обеспечения на устройство**. Первый уровень безопасности должен обеспечиваться при запуске устройства. Убедитесь, что устройство может поддерживать только авторизованное программное обеспечение от авторизованного поставщика. В этом случае подлинность программного обеспечения удостоверяется цифровыми подписями, которые предоставляются доверенными серверами.

**5.4.2. Управление доступом.** Разграничение доступа на основе ролей, упомянутое ранее, приводит к мандатному управлению доступом. Когда программное обеспечение развёрнуто, к системе применяются настройки управления доступом, чтобы управлять доступом к каждой части устройства и ресурсов отдельно. Наилучшим подходом является наиболее жесткое ограничение ресурсов, чтобы злоумышленник \(attacker\) имея точку входа к одной части, не мог получить доступ ко всей системе. Этот подход позволяет уменьшить риск для всей системы в целом.

**5.4.3. Аутентификация**. Во встроенных системах зачастую компоненты системы управляют друг другом \(связь между устройствами\), дают и выполняют команды и позволяют получить доступ к источникам информации. Следовательно, подобно обычной аутентификации, которая выполняется путем ввода учетных данных, также существует необходимость в аутентификации устройства, которая дает разрешение сегментам для обработки. Это называется аутентификацией конечной точки. Диапазон конечных устройств достаточно велик при условии, что эти машины находятся в сети TCP/IP.

**5.4.4. Детальная защита**. Хотя для разных систем требуются разные протоколы, не все они являются IT-протоколами, обеспечивающими защиту. Поэтому брандмауэры и системы предотвращения вторжений \(IPS\) удовлетворяют эту потребность, фильтруя пакеты и предотвращая влияние вредоносных программ. Другим важным моментом является анонимизация пользовательских данных, но она требует дополнительных механизмов анонимизации на этапе сбора данных и повторной идентификации при их оценке. 

**6. Конфиденциальность**

В то время как широкий спектр сетевых устройств и датчиков делает повседневную жизнь более комфортной, их растущее количество усиливает беспокойство о конфиденциальности. В любой сфере у людей существуют проблемы, связанные с их личной жизнью и стремление ее защитить. Однако с точки зрения Интернета вещей все не так просто. Не смотря на то, что пользователи приложений или социальных сетей могут защищать свою конфиденциальность, избегая этих систем, в среде IoT не требуется задействовать какую-либо систему или устройство для предоставления им данных. Они просто повсюду - камеры видеонаблюдения, датчики света, встроенные RFID-элементы и так далее. Это означает, что данные могут быть собраны бесчисленным количеством способов. И сценарии их использования также отличаются от одной ситуации к другой. Например, потребление энергии может быть использовано для профайлинга. Поэтому защита конфиденциальности в современном мире является не только сложной задачей, но также очень важной.

В целом, существуют некоторые правила, которые должны применяться в большинстве случаев сбора данных. Чтобы иметь более четкое представление, мы можем взять пример датчика освещенности. Когда человек проходит под датчиком, включается свет. Хотя это устройство кажется безопасным, оно способно собирать данные. Таким образом, для защиты конфиденциальности система должна соблюдать некоторые правила. Во-первых, информация о местоположении и о движении не должна быть собрана и записана. Второе - человек должен быть осведомлен о собранных данных и цели их сбора. И, наконец, собранные данные не должны быть сохранены или переданы в другое место или другим лицам без осведомленности человека. Они должны быть удалены после завершения работы устройства.

С другой стороны, такого рода конфиденциальность не распространяется на камеры видеонаблюдения на улице или устройства, которые используют датчики для обнаружения людей.

Другой доминирующей технологией в системах IoT является технология RFID. Следует заметить, что метки RFID являются пассивными и требуют наличия аутентифицированного считывателя. Тем не менее, сами считыватели все еще остаются открытой дверью для злоумышленников. По-прежнему возможно получить доступ к считывателям и получить нужные данные.

Для решения такого рода проблем RFID используются посредники конфиденциальности в виде прокси-серверов. Эти серверы созданы для установления соединения между пользователем и сервером данных для безопасного обмена данными. Прокси-серверы обеспечивают сбор только необходимых пользовательских данных. С другой стороны, в этом случае пользователи не могут влиять на установленную политику конфиденциальности.

**6.1. Угрозы конфиденциальности**

Исследования, сосредоточенные на конфиденциальности в IoT в целом отсутствуют из-за того, что большинство из них сосредоточены на отдельных устройствах, а не на общей среде. Но такой подход может быть логичным для обработки некоторых общих угроз, которые могут возникнуть в любой сетевой системе.

**6.2. Идентификация**

Камеры, медицинские устройства, даже небольшие бытовые приборы, которые используются дома или в офисе, имеют возможность идентифицировать человека - имя, фамилию, домашний адрес, номер телефона. Это нарушение конфиденциальности. Идентификация может осуществляться различными способами, например, с помощью камер наблюдения, устройств, которым требуется отпечаток пальца, или даже с использованием распознавания речи.

Защита идентификаторов в большинстве систем осуществляется по методике анонимизации, которая не очень полезна в системах IoT. Причина в том, что большинство методов анонимизации могут быть нарушены при использовании дополнительных данных, которые, вероятно, существуют в некоторой части системы IoT. Для защиты личности используются сложные криптографические методы, которые обычно являются наиболее дорогостоящими.

В целом, хотя идентификация не всегда значима для защиты, а иногда и не является целью, люди должны знать как можно больше о сборе данных.

**6.3. Отслеживание**

Следующая проблема, связанная с конфиденциальностью, - это данные о местонахождении людей. Данные о местоположении являются одними из важнейших, которые могут играть роль связующего звена между прочими данными и отдельными людьми. Системы GPS являются одним из основных их источников в данном случае. Получение таких данных может быть интерпретировано как вторжение в личную жизнь, в то время как объект может отслеживаться окружающими его устройствами. Таким образом, системы IoT несут угрозу, собирая данные пассивно, без ведома отдельных лиц и оценивая их в фоновом режиме. Такая ситуация может создать условия для возникновения угрозы безопасности.

Рассмотрим обстоятельства, влекущие условия возникновение угроз безопасности. Во-первых количество устройств, использующих службы определения местоположения, увеличивается в геометрической прогрессии. Сбор данных обычно происходит без ведома пользователя.

Тем не менее, есть некоторые исследования, которые проводятся для обеспечения безопасности данных о местоположении. Но все равно, большинство из них ориентированы на такие устройства, как смартфоны, в то время как основной поток данных проходит через системы, такие как датчики, камеры и так далее.

**6.4. Профайлинг**

Как указывалось ранее, IoT - это огромный набор технологий, в статьи невозможно описать все аспекты этой области. Последний подход, который будет упомянут, - это профайлинг. В основном, он является одним из ведущих инструментов для компаний, чтобы иметь лучший и ориентированный на пользователя маркетинговый план. Помимо того, что он является полезным инструментом для электронной коммерции, у него есть некоторые нарушения и угрозы для отдельных лиц, такие как нежелательная реклама, дифференциация цен из-за доходов пользователей и так далее.

С другой стороны, крупные компании, владеющие огромным количеством пользовательских данных, как правило, продают профиль пользователя другим компаниям, что приводит к еще большему нарушению конфиденциальности.

Подводя итог, можно сказать, что современные методы, такие как обфускация и анонимизация, каким-то образом помогают защитить конфиденциальность данных, но для большей эффективности требуют адаптации к различным областям IoT. И все это имеет свою цену. Сложные алгоритмы, новые технологии являются обязательными требованиями. Учитывая, что собранные данные являются основной движущей силой целых систем IoT, сохранить этот баланс довольно сложно.

**7. Технологические аспекты**

Далее представлено краткое ознакомление с технологическими аспектами IoT для представления общей информации о данных технологиях. Некоторые из них уже были рассмотрены для лучшего понимания примеров. Было бы разумно разделить их на несколько подкатегорий по их индивидуальным характеристикам.

**7.1. RFID**

В предыдущих главах технология RFID были упомянута несколько раз. Поскольку эта технология является одной из ведущих в мире IoT, было бы логично поговорить об ней немного подробнее, предоставив информацию об истории ее возникновения.

Система RFID состоит из двух частей - метки и считывателя. Метки могут быть двух видов: пассивные и активные. Наиболее широко используемыми являются пассивные метки. Метка состоит из двух компонентов: микрочипа, который хранит информацию, и антенны, которая помогает отправлять и принимать радиосигналы от и к считывателю. Каждая метка содержит отдельный идентификатор. Когда считыватель отправляет радиосигналы, метка получает их с помощью антенны и передает данные, хранящиеся в микрочипе. И, наконец, считыватель передает его на подключенный компьютер. Это общая концепция работы всех RFID-систем.

**7.2. WSN**

Следующая ветвь в IoT - это беспроводная сенсорная сеть \(WSN\), которую можно назвать глазами и ушами IoT. WSN помогает установить связь между реальным миром и цифровым.

Как показано на рисунке 2, WSN состоит в основном из сенсорных узлов, шлюзов и контроллеров. Сенсорные узлы, которые распределяются по требуемой площади, собирают и отправляют данные на узел управления с использованием ближайшего шлюза. Пока данные перемещаются от датчика к основному узлу управления через Интернет, сигнал проходит несколько шлюзов.

![](https://telegra.ph/file/e058c0b57f2cde1783d91.png)

 По мере развития технологий стоимость WSN снижалась, а область ее использования расширялось.

**8. Заключение**

Технологии и, следовательно, Интернет постоянно меняют и усложняют свою структуру, которая имеет как преимущества, так и риски, и в основном отдельные пользователи - потребители - находятся в такой зоне. По этой причине методы защиты должны быть разработаны и приняты к применению как можно раньше. К сожалению, иногда это нереализуемо из-за скорости развития IoT. Однако, в любом случае, ответственность за безопасность не может быть возложена только на пользователей. Учитывая, что смыслом IoT является сбор данных, то методы защиты должны обеспечивать баланс между инновациями и безопасностью пользователей. В противном случае прогресс может быть затруднен, что будет невыгодно для общества, либо жизнь потребителей может оказаться под угрозой.

Прогресс является важной частью современного общества и является неизбежным процессом. Тем не менее, он должен проходить таким образом, чтобы безопасность и неприкосновенность частной жизни людей не были нарушены. Однако это не всегда получается. В частности, системы IoT позволяют собирать данные о ком-либо, даже если человек избегает использование технологий.

Среди угроз конфиденциальности упоминается профайлинг, который является одной из основных проблем для потребителей. Таким образом, его результатом могут стать ошибочные автоматические решения, нежелательная реклама и даже опасные для жизни ситуации. Эти проблемы подтверждают идею о том, что защита данных в любой системе IoT является одним из наиболее важных элементов, которые необходимо учитывать еще на самой ранней стадии.



{% embed url="https://telegra.ph/Bezopasnost-i-konfidencialnost-v-Internete-veshchej-12-04" %}

{% embed url="https://telegra.ph/Bezopasnost-i-konfidencialnost-v-Internete-veshchej-CHast-2-12-04" %}

{% embed url="https://t.me/in51d3" %}

