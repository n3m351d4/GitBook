---
cover: >-
  https://images.unsplash.com/photo-1592919347498-69564e361791?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw0fHx3aXJlbGVzc3xlbnwwfHx8fDE2MzY0NzA2NzY&ixlib=rb-1.2.1&q=85
coverY: 0
---

# 4.2 Ключевые проблемы безопасности сетей 5G

С момента своего появления системы беспроводной связи были подвержены проблемам безопасности. В сетях первого поколения (1G) мобильные телефоны и беспроводные каналы связи имели такие проблемы безопасности как клонирование и подделка идентификаторов. Второе поколение (2G) сетей было подвержено рассылке спама, которая использовалась не только для трансляции нежелательной маркетинговой информации, но и для распространения ложной информации. В беспроводных сетях третьего поколения (3G) связь на основе IP перенесла уязвимости и проблемы безопасности Интернета в беспроводные каналы связи. С ростом потребности в IP-связи мобильные сети четвертого поколения (4G) позволили повсеместно распространить интеллектуальные устройства, мультимедийный трафик и привнесли новые возможности в сотовую связь, что повлекло появление более сложного и динамичного ландшафта угроз. С появлением беспроводных сетей пятого поколения (5G) векторов угроз безопасности станет больше, чем когда-либо прежде.

В связи с этим, важно определить проблемы безопасности, которые свойственны не только мобильным сетям, но и существуют в технологиях, которые очень важны для 5G. В этой работе мы рассмотрим проблемы безопасности, которые занимают важное место в технологии 5G и нуждаются в принятии оперативных мер безопасности. Далее мы обсудим решения безопасности, которые можно применить для угроз, рассматриваемых далее.

Наибольшее внимание необходимо уделить отраслям, использующим технологию 5G для поддержания работы инфраструктур, содержащих критически важные объекты. Например, умные города и беспилотное автомобили, а также сети экстренного реагирования и контроля трафика, которые будут использовать возможности 5G.

В случае, если сети 5G, поддерживающие подобные объекты, столкнутся с помехами или отключатся, последствия могут стать катастрофическими. Например, если злоумышленник отключит городское водоснабжение.

Сети 5G будут объединять объекты критически важной инфраструктуры, которые, в свою очередь потребуют усиления мер безопасности не только для промышленных объектов, но и общества в целом. Например, нарушение безопасности в системах электропитания может быть катастрофическим для всех электрических и электронных систем, от которых зависит общество. Точно так же мы знаем, что данные имеют решающее значение при принятии решений, но что, если критические данные будут повреждены при передаче по сетям 5G? Поэтому очень важно исследовать и выделить проблемы безопасности в сетях 5G и рассмотреть потенциальные решения обеспечения безопасности систем 5G. Основные и широко обсуждаемые в литературе проблемы 5G, заключаются в следующем:

• **Всплеск сетевого трафика:** Повышение количества оконечных пользователей и устройств IoT.

• **Безопасность радиоинтерфейсов:** передача ключей шифрования радиоинтерфейса по незащищенным каналам

• **Целостность пользовательских данных:** отсутствие криптографической защиты пользовательских данных.

• **Мандатная сетевая защита:** архитектура безопасности, изменяющая ограничения в зависимости от используемого сервиса, приведет к необязательному использованию мер безопасности.

• **Безопасность роуминга:** параметры безопасности пользователя не обновляются при переходе из сети одного оператора в другую сеть, что приводит к нарушениям безопасности.

• **Атаки типа «отказ в обслуживании» (DoS) на инфраструктуру**: открытость элементов управления сетью и незашифрованные каналы управления.

• **«Сигнальные штормы»**: могут быть вызваны распределенными системами управления, требующими координации, например, на «уровне без доступа» (NAS - Non-Access Stratum).

• **Атаки типа «отказ в обслуживании» (DoS) на пользовательское оборудование**: при отсутствии принятия мер безопасности для операционных систем, приложений и конфигураций пользовательских устройств.

**• Атаки, использующие понижение протокола**: атаки использующие помехи и заставляющие пользовательское устройство подключаться к уязвимым сетям предыдущих поколений.

Рабочая группа 3GPP активно участвует в определении требований безопасности и конфиденциальности, а также в создании архитектуры протоколов безопасности для 5G. Организация Open Networking Foundation (ONF) занимается адаптацией технологий программно-определяемых сетей (SDN) и виртуализацией сетевых функций (NFV) и публикует технические спецификации, включая спецификации безопасности этих технологий.

Принципы проектирования 5G, находящиеся за пределами радиосвязи, заключаются в следующем: создание составного ядра и упрощение операций и управления за счет использования новых вычислительных и сетевых технологий. Поэтому, целесообразно было бы сосредоточиться на безопасности тех технологий, которые будут соответствовать принципам проектирования, то есть мобильным облакам, SDN и NFV, и линиям связи, которые используются этими технологиями или между ними. В связи с беспокойством о конфиденциальности пользователей, необходимо также выявлять потенциальные проблемы конфиденциальности. Проблемы безопасности изображены на рис. 9. В таблице 5 представлен обзор различных типов угроз безопасности и атак на целевые элементы и службы сети, технологии, наиболее подверженные атакам и угрозам отмечены знаком «+».

&#x20;

![](<../../../.gitbook/assets/image (253).png>)

&#x20;

![](<../../../.gitbook/assets/image (2).png>)

![](<../../../.gitbook/assets/image (1).png>)

![](<../../../.gitbook/assets/image (4).png>)

&#x20;

![Рисунок 9. Проблемы безопасности сетей 5G](<../../../.gitbook/assets/image (7).png>)

## 4.2.1 Атаки, использующие понижение версии протокола связи <a href="#_toc41602133" id="_toc41602133"></a>

Один из основных рисков безопасности связан с протоколом, разработанным для обеспечения соединений 4G или 3G, когда надежный сигнал 5G недоступен. Когда устройство 5G переключается на 3G или 4G, оно подвергается уязвимостям, которым подвержены протоколы предыдущих поколений.

При переключении с 5G на 3G или 4G архитектура будет полагаться на бесшовный переход между этими двумя сетями. Но пока что не известно, как оператор мобильной связи сможет гарантировать, что в переходе не будет слабых мест.

Еще одной проблемой являются поставщики 5G. Недавние опасения были связаны с потенциальной угрозой со стороны поставщика сетевой инфраструктуры Huawei, который получил одобрение Великобритании на развертывание национальной сети 5G.

Многие опасаются, что производитель сможет отслеживать данные пользователей. Учитывая эту и другие угрозы безопасности 5G, Европейская комиссия выпустила набор инструментов 5G, чтобы помочь европейским странам координировать свой подход к конфигурированию сетей.

Органы по стандартизации, такие как 3GPP ETSI и IETF, работают над стандартами и спецификациями для защиты автономной версии технологии 5G, используемой для поддержки бизнес-приложений.

## 4.2.2 Проблемы безопасности мобильных облаков <a href="#_toc41602134" id="_toc41602134"></a>

Поскольку системы облачных вычислений содержат ресурсы, которые совместно используются пользователями, существует возможность, что пользователь распространит вредоносный трафик, чтобы снизить производительность такой системы, потреблять больше ресурсов или скрытно обращаться к ресурсам других пользователей. Аналогично, в многопользовательских облачных сетях, где арендаторы используют собственную логику управления, подобные взаимодействия могут вызывать конфликты в конфигурациях сети. Мобильные облачные вычисления (MCC) переносят концепции облачных вычислений в экосистемы 5G. Это создает ряд уязвимостей в системе безопасности, которые в основном зависят в архитектуры и инфраструктуры сети. Открытая архитектура MCC и универсальность мобильных терминалов порождают уязвимости, с помощью которых злоумышленники смогут создавать угрозы и нарушать конфиденциальность в мобильных облаках.

В этой работе угрозы MCC классифицируются в соответствии с целевыми облачными сегментами на внешние, внутренние и сетевые угрозы безопасности для мобильных устройств. Внешняя часть архитектуры MCC — это клиентская платформа, которая состоит из мобильного терминала, на котором работают приложения и интерфейсы, необходимые для доступа к облачным средствам. В этом сегменте угрозы могут варьироваться от физических - где основными целями является фактическое мобильное устройство и другие интегрированные аппаратные компоненты, до угроз уровня приложений, где злоумышленниками используется шпионское и другое злонамеренное программное обеспечение для нарушения работы приложений или сбора конфиденциальной информации пользователя. Внутренняя часть состоит из облачных серверов, систем хранения данных, виртуальных машин, гипервизора и протоколов, необходимых для предоставления облачных услуг. В этом сегменте угрозы безопасности в основном направлены на мобильные облачные серверы. Диапазон этих угроз может варьироваться от репликации данных до атак на HTTP и XML DoS (HX-DoS).

Сетевые угрозы безопасности мобильных устройств направлены на технологии радиодоступа (RAT), которые связывают мобильные устройства с облаком. Это может быть традиционный Wi-Fi, 4G Long Term Evolution (LTE) или другие новые RAT, которые будут поставляться с 5G. Атаки в этой категории включают в себя сниффинг Wi-Fi, DoS-атаки, подделку адресов и перехват сессий. Облачная сеть радиодоступа (C-RAN) является еще одной ключевой областью интереса при анализе проблем безопасности в мобильных облаках 5G. C-RAN обладает потенциалом для удовлетворения потребностей отрасли в наращивании пропускной способности для повышения мобильности в системах связи 5G. Однако C-RAN подвержен сложным проблемам безопасности, связанным с виртуальными системами и технологией облачных вычислений, например, централизованная архитектура C-RAN подвергается угрозе единой точки отказа. Другие угрозы – это вторжения (злоумышленники проникают в виртуальную среду для мониторинга, изменения или запуска программных средств), они также представляют собой существенные угрозы для системы.

## 4.2.3 Проблемы безопасности SDN и NFV <a href="#_toc41602135" id="_toc41602135"></a>

SDN централизует платформы управления сетью и обеспечивает возможность программирования сетей связи. Однако эти две разрушительные функции создают возможности для взлома сети. Например, централизованное управление будет наилучшей целью для DoS-атак, а уязвимость критически важных интерфейсов прикладного программирования (API) может привести к разрушению всей сети. Контроллер SDN изменяет правила передачи потока в тракте данных, поэтому контроллер трафика может быть легко идентифицирован. Это делает контроллер объектом, видимым в сети, делая его наилучшим выбором для DoS-атак. Централизация сетевого управления может сделать контроллер слабым местом всей сети из-за неустойчивости перед атаками, использующими метод подавления. Поскольку большинство сетевых функций могут быть реализованы как приложения SDN, могут появиться подобные вредоносные приложения, нарушающие работоспособность сети при наличии соответствующего доступа.

Несмотря на то, что NFV (виртуализация сетевых функций) очень важна для будущих сетей связи, она сталкивается с базовыми проблемами безопасности, такими как конфиденциальность, целостность, подлинность и доступность. Существующие платформы NFV не обеспечивают надлежащей безопасности и изоляции для виртуальных телекоммуникационных услуг, и использования в мобильных сетях. Одной из основных проблем, сохраняющихся при использовании NFV в мобильных сетях, является динамическая природа функций виртуальных сетей (VNF), которая влечет за собой появление ошибок конфигурации и нарушения безопасности. Так же, существует проблема, заключающаяся в том, что компрометация гипервизора может повлечь за собой компрометацию всей сети.

## 4.2.4 Проблемы безопасности каналов связи <a href="#_toc41602136" id="_toc41602136"></a>

Сеть 5G будет иметь сложную экосистему, включающую в себя беспилотные аппараты и управление воздушным движением, облачную виртуальную реальность, подключенные к сети транспортные средства, интеллектуальные фабрики, транспорт и электронное здравоохранение. Таким образом, появится необходимость в защищенной системе связи, которая будет поддерживать более частую аутентификацию и обмен более конфиденциальными данными. Так как сетями нового поколения будут пользоваться как поставщики государственных услуг, так и операторы мобильных сетей (MNO), то в такой экосистеме потребуется реализация нескольких уровней инкапсулированной аутентификации – на уровне доступа к сети, на уровне обслуживания и между участниками сети.

До сетей 5G мобильные сети имели выделенные каналы связи на основе туннелей GTP и IPsec. Интерфейсы связи, такие как X2, S1, S6, S7, которые используются только в мобильных сетях, требуют значительного уровня знаний для проведения атаки. Однако сети 5G на основе SDN не будут иметь выделенных интерфейсов и будут использовать только общие. Открытость этих интерфейсов увеличит количество возможных атак для злоумышленников. Связь в мобильных сетях 5G на основе SDN может быть разделена на три канала связи: канал данных, канал управления и межконтроллерный канал. В современной системе SDN эти каналы защищены с помощью TLS (Transport Layer Security) и SSL (Secure Sockets Layer). Однако сеансы TLS/SSL уязвимы для атак на уровне IP, SDN сканирования и не имеют надежных механизмов аутентификации.

## 4.2.5 Проблемы конфиденциальности <a href="#_toc41602137" id="_toc41602137"></a>

Проблемы конфиденциальности пользователей связаны с передачей пользовательских данных, раскрытием местоположения и личности пользователя. Большинство приложений для смартфонов запрашивают перед установкой детальную информацию о персональных данных абонента. Разработчики приложений или компании редко конкретизируют, как хранятся подобные данные и для каких целей они будут использоваться. Использование семантической информации, атаки по времени и граничные атаки, в основном, направлены на раскрытие местоположения абонентов. На физическом уровне местоположение абонента может быть раскрыто благодаря алгоритмам 5G, использующимся для выбора точки доступа. Для выявления личности может использоваться перехват абонента путем перехвата международного идентификатора абонента мобильной связи (IMSI) абонентского пользовательского оборудования (UE). Такие атаки также могут быть осуществлены путем установки поддельной базовой станции, которая будет определяться пользовательскими устройствами приоритетной для подключения, и будет вынуждать абонентов отправлять себе свои идентификаторы IMSI.

Сети 5G будут объединять таких участников, как виртуальные операторы мобильной связи (VMNO), поставщики услуг связи (CSP) и поставщики сетевой инфраструктуры. Все они имеют разные приоритеты безопасности и конфиденциальности. Проблемой сетей 5G станет синхронизация несовпадающих политик конфиденциальности между этими участниками. В предыдущих поколениях операторы мобильной связи имели прямой доступ и контроль над всеми компонентами системы. Однако в 5G операторы мобильной связи потеряют полный контроль над системами, поскольку они будут полагаться на новых участников, таких как CSP. Таким образом, операторы 5G потеряют управление безопасностью и конфиденциальностью. Конфиденциальность пользователей и данных ставится под серьезную угрозу в общих средах, где одна и та же инфраструктура совместно используется различными участниками, например, VMNO и прочими конкурентами. Более того, в сетях 5G нет физических границ, поскольку они используют облачные хранилища данных и функции NFV. Следовательно, операторы 5G не будут иметь контроля над местом хранения данных в облачных средах. Поскольку разные страны имеют разные уровни механизмов защиты данных в зависимости от своих предпочтений, конфиденциальность ставится под сомнение, например, если пользовательские данные хранятся в облаке, находящемся в другой стране.