---
description: 25.07.2020
---

# Throwing Star LAN Tap

{% embed url="https://www.youtube.com/watch?v=i-2Xc8JqKBI" %}

{% embed url="https://github.com/n3m351d4/Throwing-Star-LAN-Tap" %}

Всем привет! Как вы знаете, я периодически начинаю делать разные девайсы/бейджи, но не могу никак запустить производство \(хотя бы по 100 штук\). Я решила начать с небольшого бейджа, который реально выпустить массово, так как производство и отладка одного активного бейджа может доходить до 14 тысяч рублей и нести негативные последствия, как в случае с котами \(были такие бейджи у меня\) – в тот момент мне не хватило средств на их доработку и поэтому было решено начать с чего-то попроще, чтобы собрать средства на что-то более сложное и затратное. Я, наверное, не буду рассказывать о ценообразовании и текущей ситуацией с заказами электроники в России – тут, конечно, выходит намного дороже, чем в Китае. Поэтому, платы, детали и аксессуары я буду преимущественно заказывать оттуда. Итак, было решено начать с простого концепта, который доступен в открытом доступе и в продаже в магазине Hak5 и Great Scott Gadgets.

![https://greatscottgadgets.com/throwingstar/](../.gitbook/assets/image%20%28199%29.png)

Итак, для производства данного девайса нам потребуется: 

#### Печатная плата 

Печатная плата будет доступна в моем гитхабе [https://github.com/n3m351d4](https://github.com/n3m351d4). Так же хочу отметить, что она потерпела некоторые изменения в плане дизайна, для того, чтобы соответствовать классическим бейджам. На рисунках ниже изображена плата, заказанная в первой партии из 5 штук, в синем цвете. Если проект окажется рабочим, то я планирую дозаказать еще 95 плат разных цветов \(постепенно\). Прошу обратить внимание на то, что на стороне для монтажа компонентов есть дополнительные линии! То есть разъемы и конденсатор вставляются с этой стороны, а запаиваются с другой.

![](../.gitbook/assets/image%20%28204%29.png)

![](../.gitbook/assets/image%20%28201%29.png)

#### Компоненты

Так как данное устройство – пассивное, нам потребуется совсем немного компонентов. 

#### **Разъемы.**

Я выбрала четыре модульных разъема Amphenol RJHSE-5380 \(shielded\) – они дорогие, но экранированные.

![](../.gitbook/assets/image%20%28206%29.png)

 \(В дальнейшем мы постараемся их не использовать, так как они уже три недели идут до меня :D\). Так же существует возможность использования RJHSE-5080 \(unshielded\), как указано в [https://github.com/greatscottgadgets/throwing-star-lan-tap](https://github.com/greatscottgadgets/throwing-star-lan-tap). Они дешевле, но не экранированы.

![](../.gitbook/assets/image%20%28207%29.png)

#### **Конденсаторы**

Нам потребуется по два выводных конденсатора на 220 пФ, аналогов Xicon 140-50P2-221K-RC \(Ceramic Disc Capacitors 50V 220pF Y5P 10% Tol.\). Это керамические конденсаторы на 50 B 220 пФ с допуском 10%. Я заказала в Чип и Дип позицию «Конд.кер.диск. 220 пФ х 50В,+/-10%» по 2 штуки на плату. Они выглядят следующим образом:

![](../.gitbook/assets/image%20%28202%29.png)

#### **Ленты** 

Ленты для бейджей я нашла в Китае. Для синих плат ленты будут выглядеть следующим образом:

![](../.gitbook/assets/image%20%28203%29.png)

#### **Сборка** 

Чтобы собрать детали воедино, вам потребуется сначала вставить разъемы, защелкнув их в отверстиях платы, с той стороны, с которой я указала ранее, затем запаять их снизу. Далее, вставьте конденсаторы \(они не полярные\) в отверстия, можно аккуратно отформовать их выводы, если требуется, для того, чтобы они стояли ровно. Помните, что корпус конденсатора не должен касаться платы, необходимо оставить пару миллиметров выводов над ней! После установки каждого элемента его необходимо запаять, придерживая снизу, а по завершению монтажа надо протереть флюс и прочую грязь спиртом \(изопропанол\), спирт можно использовать с зубной щеткой \(только потом зубы ей не стоит чистить – это должна быть отдельная щетка для подобных работ\).

![](../.gitbook/assets/image%20%28200%29.png)

#### **Использование** 

Для подключения платы к целевой сети используйте кабели Ethernet \(разъемы, напротив которых НЕТ конденсаторов\). Используйте кабели Ethernet для подключения одного или обоих портов мониторинга к портам одной или двух станций мониторинга. Каждый порт отслеживает трафик только в одном направлении. Используйте ваши любимые программы \(например, tcpdump или Wireshark\) на станциях мониторинга для захвата сетевого трафика. 

#### Действие

Подобный перехват данных в локальной сети - это пассивный перехват, не требующий питания для работы. Существуют активные методы перехвата Ethernet-соединений \(например, зеркальный порт на коммутаторе\), но ни один не может превзойти пассивные по мобильности. Для целевой сети, устройство выглядит как часть кабеля, но данные дублируется в порты мониторинга. Порты мониторинга предназначены только для приема, они подключаются к линиям приема данных на станции мониторинга, но не подключаются к линиям передачи, что делает невозможным случайную передачу пакетов данных в сеть. Данное устройство предназначено для мониторинга сетей 10BASET и 100BASETX. Throwing Star LAN Tap не может осуществлять мониторинг сетей 1000BASET \(Gigabit Ethernet\), поэтому он намеренно ухудшает качество целевых сетей 1000BASET, заставляя их переключиться на низкую скорость \(обычно 100BASETX\), которую можно пассивно контролировать. Это обеспечивают конденсаторы \(C1 и C2\). Как и все пассивные перехватчики локальной сети, наш, в некоторой степени ухудшает качество сигнала. За исключением случаев, описанных выше, для гигабитных сетей, это почти никогда не вызывает проблем в целевой сети. В ситуациях, когда используются очень длинные кабели, ухудшение сигнала может уменьшить производительность сети. Рекомендуется использовать кабели, длина которых не превышает необходимую.

