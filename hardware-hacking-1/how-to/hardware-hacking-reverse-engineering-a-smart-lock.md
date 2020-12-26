---
description: Перевод @n3m351da @in51d3 2020
---

# -Hardware Hacking: Реверс умного замка

[Оригинальный текст](https://www.blackhillsinfosec.com/reverse-engineering-a-smart-lock/)

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image24.png)

## **Введение**

Недавно мне представилась возможность изучить результаты исследований известной охранной фирмы \(F-Secure\), которая обнаружила уязвимость в Guardtec KeyWe Smart Lock. Специалисты F-Secure обнаружили, что злоумышленник может перехватить и расшифровать трафик, исходящий от владельца замка. Я нашел их блог чрезвычайно увлекательным и очень информативным. Вскоре у меня появился стимул посмотреть, смогу ли я продублировать их усилия, так как я понял, что F-Secure выпустила уведомление об уязвимости и что поставщику была предоставлена возможность уменьшить ее воздействие. К сожалению, их варианты действий были крайне ограничены из-за того, что у них не было функции обновления прошивки. Вместо этого они решили использовать обфускацию в своем приложении для Android, пытаясь скрыть наиболее важные разделы кода \(что они сделали довольно хорошо\).

Не смотря на то, что F-Secure заложили основу для этого исследования, они старались не раскрывать слишком много информации и даже ИЗМЕНЯЛИ некоторые из своих собственных инструментов. Во время своего исследования я обнаружил, что постоянно возвращаюсь к их блогам. 

После получения моего KeyWe Smart Lock и создания тестового окружения для его настройки, я загрузил фирменное приложение для Android на свой мобильный телефон и создал свою учетную запись. Я познакомился с функциями замка и внешним видом мобильного приложения. Я также запустил мобильное приложение Nordic nRF Connect \(доступно бесплатно в магазине Google Play\), чтобы получить полезную информацию о моем замке - адрес Bluetooth, UUID и прочие характеристики. 

**Примечание**. Спецификации BLE легко доступны [здесь](%20https://www.bluetooth.com/specifications/).

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image20.png)

## **Полезная информация**

Связь между KeyWe \(замок\) и \(мобильным\) приложением поддерживается через пакеты Bluetooth с низким энергопотреблением \(BLE\), которые шифруются с использованием стандартного шифра ECB AES-128 для предотвращения стороннего перехвата. Безопасность канала сообщений основана исключительно на **3 секретных ключах**, используемых для шифрования и дешифрования пакетов AES-128 OTA \(передаваемых по беспроводной сети\). 

* Общий ключ \(**CommonKey**\), используется для первоначального обмена ключами.
* Ключ приложения \(**AppKey**\), используется для шифрования пакетов, отправляемых приложением замку.
* App Key  \(**AppKey**\) used to encrypt packets sent by the App to the Door
* Ключ двери \(**DoorKey**\), используется для шифрования пакетов, отправляемых дверью в приложение.

## **Генерация ключа**

**CommonKey** основан на статическом 16-байтовом значении, которое начинается с последних 5 байт Bluetooth адреса устройства:

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image22.png)

После изучения CommonKey на нескольких устройствах KeyWe выяснилось, что во всех устройствах различаются только последние два байта адреса Bluetooth! Для моего устройства уникальными были значения 4C и 93. Это говорит о том, что CommonKey предсказуем и основан исключительно на двух байтах! 

**AppKey** и **DoorKey** создаются при помощи двух сильно запутанных алгоритмов и методов makeAppKey и makeDoorKey. Сотрудники F-Secure создали инструмент, который генерирует три секретных ключа, моделируя действие этих двух методов. Однако по прошествии значительного времени я смог самостоятельно перепроектировать функциональность этих методов, работая с удобным инструментом с открытым исходным кодом под названием Frida \(подробнее об этом позже\).

Создание **AppKey** и **DoorKey** происходит путем передачи двух аргументов функциям **makeAppKey** и **makeDoorKey**. Этими двумя аргументами являются **AppNumber** и **DoorNumber** соответственно.

AppNumber - статическое \(жестко закодированное\) 12-байтовое значение \(дополненное четырьмя нулевыми байтами\): 92 4b 03 5f bd a5 6a e5 ef 0e d0 5a 00 00 00 00 

**Примечание**: AppNumber зашифровывается с помощью CommonKey и отправляется двери. \(блокировка\) как первый пакет пользовательского сеанса, тем самым инициируя начало сеанса. 

**DoorNumber** - это динамическое \(изменяется каждый новый сеанс\) 12-байтовое значение \(дополненное четырьмя нулевыми байтами\), генерируемое дверью \(замком\). Это значение \(также зашифрованное с помощью CommonKey\) отправляется в приложение в ответ на AppNumber. \(см. диаграмму ниже\)

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image4-3.png)

**Примечание**. Эти две передачи \(**AppNumber** и **DoorNumber**\) завершают процесс обмена ключами и позволяют создать **AppKey** и **DoorKey**, которые обеспечивают безопасную связь OTA между приложением и замком. 

Теперь, когда **AppNumber** и **DoorNumber** созданы , у нас есть два компонента, необходимые для генерации двух оставшихся секретных ключей \(**AppKey** и **DoorKey**\). Это достигается путем вызова функций makeAppKey и makeDoorKey с AppNumber и DoorNumber в качестве аргументов. Это делается внутри прошивки и не отправляется по OTA.

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image18.png)

## **Схема работы**

Теперь, когда мы сгенерировали и обменялись ключами AppKey и DoorKey, обе стороны может шифровать / дешифровать отправленные или полученные пакеты. Все пакеты, передаваемые приложением, будут зашифрованы с использованием ключа приложения **AppKey**, а все пакеты, передаваемые дверью, будут зашифрованы с помощью ключа двери. Обе стороны знают схемы шифрования друг друга и поэтому могут расшифровать пакет.

Следующая диаграмма демонстрирует порядок событий типичного сеанса:

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image25.png)

Следует отметить, что все эти пакеты передаются OTA в начале каждого пользовательского сеанса, и все 13 из этих пакетов зашифрованы либо **AppKey**, либо **DoorKey** в зависимости от того устройства, которому они предназначены.

## **Реверс Android приложения**

Все мобильные приложения загружаются в виде файлов APK \(Android Application Package\). Файлы APK сохраняются в формате ZIP и загружаются непосредственно на устройства Android, обычно через магазин Google Play, но их можно найти и на других веб-сайтах. При обратной разработке APK-файлов Android я обнаружил, что часто полезно использовать сторонние сайты для поиска более старых версий рассматриваемого APK. Мой любимый - [https://apkpure.com/](https://apkpure.com/).

**Требования к программному обеспечению**

Java version 1.8.0\_251

ADB \(android debug bridge\) version 1.0.41

APKStudio \(wrapper for Apktool\) version 2.4.1

Dex2Jar version 

Jadx version 1.1.0

Frida version 12.8.9

**Требования к оборудованию**

Телефон на андроид с привилегиями пользователя root

Обычный APK-файл содержит очень полезный контент, например, файлы AndroidManifest.xml, classes.dex и resource.arsc; а также папку Meta-INF и res. Есть несколько способов открыть APK, хранящийся на вашем компьютере. Очевидно, что поскольку это ZIP-файл, любой из различных экстракторов UNZIP сработает, однако использование инструментов, таких, как Dex2Jar, Apktool и Jadx, представит дополнительные преимущества, например, преобразование файлов .dex в код Java для удобочитаемости и поддержка графического интерфейса для простоты навигации по коду.

Файлы DEX \(исполняемые файлы Dalvik\) - это файлы разработчика, используемые для инициализации и исполнения приложений мобильной платформы Android. Такие инструменты, как Apktool, могут декомпилировать файлы DEX \(машинный язык\) в файлы Smali \(исходный язык ассемблера\). Также, можно использовать такие инструменты, как dex2jar, для преобразования файлов DEX в файлы JAR \(java\) и использовать графический интерфейс jadx для открытия файла JAR как исходного кода java. Исходный код Java может быть намного проще для чтения, чем исходный код Smali. Существует множество вариантов навигации по APK-файлу, мой любимый -  APKStudio.

Описание любого из вышеперечисленных инструментов, выходит за рамки данной статьи. Я бы посоветовал загрузить их и поэкспериментировать с разными утилитами, чтобы найти наиболее подходящие и почитать соответствующие руководства.

## **Использование FRIDA** 

Исследователи F-Secure написали в своем блоге, что им удалось перехватить вызовы функций в приложении для Android с помощью инструмента под названием Frida. Я не знал об этом инструменте, поэтому решил изучить его. Эта утилита просто потрясающая! Понимание того, как использовать Frida, выходит за рамки этой статьи. Однако достаточно сказать, что Frida позволяет исследователю подключаться к существующим функциям в приложении и динамически выгружать аргументы и возвращаемые значения. Это определенно стоит попробовать! [https://frida.re/docs/home/](https://frida.re/docs/home/)

**Установка Frida**

```text
pip install frida
pip install frida-tools
```

**Необходимо**

* frida-server и ****adb \(android debug bridge\)
* Телефон на андроид с привилегиями пользователя root

**Установка frida-server на телефон**

Чтобы установить сервер, перейдите по адресу [https://github.com/frida/frida/releases](https://github.com/frida/frida/releases) и загрузите подходящий файл к вашей платформе.

Если вы не знаете архитектуру своего телефона, то скачайте и запустите приложение **Droid Hardware Info** \(из Google Play\)

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image23.png)

* Скопируйте загруженный файл в директорию своего проекта
* Откройте эту директорию
* Разархивируйте файл .xz: **xz -d -k frida-server-12.9.8-android-arm.xz**
* Используя adb \(android debug bridge\) отправьте извлеченный файл на свой телефон

**Начальная настройка**: \(установка frida-server на телефон\)

```text
$ adb push frida-server-12.9.4-android-arm /data/local/tmp
$ adb shell     ### shell into phone
$ su                                    
# cd /data/local/tmp
# chmod 777 frida-server
# ./frida-server &              
```

Как только frida-server установлен на телефоне, запустите новый сеанс следующим образом:

```text
$ adb shell
$ su
# cd /data/local/tmp
# ./frida-server &
```

Изначально мне было трудно изучать Frida и пытаться найти вызовы функций, на которые F-Secure ссылались в своих блогах. Из-за рекомендаций F-Secure поставщик сильно обфусцировал последние версии приложения, что означало отсутствие функций **makeAppKey** или **makeDoorKey**. Я также обнаружил, что поставщик включил меры безопасности для предотвращения запуска приложения **KeyWe** на телефонах с доступом к root. Исследователи F-Secure создали классный инструмент используя Python и javascript Frida для присоединения к  методу вызывающему ошибку и внедрили код Java, чтобы всегда возвращать false для **isRooted**.

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image12-2.png)

К сожалению, из-за обфускации нового приложения класс «RootTool» перестал существовать, и мне пришлось искать в коде эквивалентный класс. После долгого поиска кода я в конце концов нашел методы проверки наличия привилегий root. Оказалось, что класс «RootTool» теперь упоминается как «n», а функция «iSRooted» теперь упоминается как функция «b».

**Модифицированный скрипт F-Secure’s KeyWe-Tooling** 

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image7-2.png)

**Успешный обход детектирования наличия root прав**

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image1-4.png)

Я определил, что работать с последней версией приложения стало трудозатратнее, чем это того стоило. Разбор обфусцированного кода становился чрезвычайно утомительным в попытках найти функции, имена которых были изменены на одну букву. К счастью, примерно в это же время коллега дал мне ссылку на несколько более старых APK-файлов Android KeyWe [https://apkpure.com/keywe-for-a-smarter-life/com.guardtec.keywe/versions](https://apkpure.com/keywe-for-a-smarter-life/com.guardtec.keywe/versions). Это оказалось отличной находкой, так как теперь я мог найти версию, со всеми функциями, на которые были ссылки. Я  вернулся к исходной \(немодифицированной\) версии сценария сокрытия root-прав от F-Secure, и это сработало! Кроме того, у меня заработал их скрипт «trace\_java\_functions», предоставляя мне огромное количество данных.

В следующем примере шестнадцатеричного дампа Frida мы видим, как выполняется вызов функции шифрования AES-128, передающей два аргумента byte\_array \(AppNumber, CommonKey\). Она шифрует AppNumber с помощью CommonKey для передачи OTA-пакета к двери в начале сеанса.

Аналогичным образом, показан вызов функции шифрования AES-128 с передачей аргументов \(DoorNumber, CommonKey\) для шифрования DoorNumber для передачи OTA в приложение. 

Frida позволяет нам увидеть переданные аргументы и возвращаемые значения двух внутренних вызовов функций, которые генерируют AppKey и DoorKey.

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image15.png)

Основываясь на множестве записанных сессий Frida и расшифровке данных, я вскоре очень разобрался с работой приложения KeyWe и хорошо понял, как и где генерируются ключи, а также последовательность событий при запуске. Кроме того, я узнал, что во время активного сеанса состояние двери постоянно отслеживается и обновляется посредством обмена информацией между приложением и дверью. Разобранные мной сеансы включали в себя вход в систему, разблокировку и блокировку двери с помощью приложения на моем телефоне.

**Java скрипты F-Secure для Frida :**

Обход обнаружения наличия рут-прав:  disable\_root\_detection.js

Трассировка внедренных java-функций:  trace\_java\_functions.js  \(original\)

**Сессия включает следующие действия:** 

Войдите в систему, дождитесь подключения и статуса ЗАБЛОКИРОВАН \(красный\) Нажмите, чтобы РАЗБЛОКИРОВАТЬ, подождите несколько секунд, нажмите, чтобы ЗАБЛОКИРОВАТЬ, подождите несколько секунд Нажмите, чтобы РАЗБЛОКИРОВАТЬ, дождитесь автоблокировки, отключите

**Session includes:**

* Вход, ожидание соединения и появления статуса "заблокировано" LOCKED
* Нажатие на кнопку "разблокировать", ожидание, нажатие на кнопку "заблокировать", ожидание
* Нажатие на кнопку "разблокировать", ожидание авторазблокировки, разъединение

C:\Users\rayfe\keywe-tooling\frida&gt;**start\_root.py**                                                                     ****    ****

C:\Users\rayfe\keywe-tooling\frida&gt;**keywe\_inject.py trace\_java\_functions.js**

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image19.png)

## **BTSNOOP \(ANDROID BLUETOOTH логгер\)**

BTSNOOP одна из моих лучших находок, для захвата сеанса Bluetooth между телефона и периферией, я пытался это делать различными методами, например, платами nRF Sniffer Nordic nRF52840-DK, Sena UD100, ключ Ubertooth-One и ключ Texas Instruments CC2540. Проблема всех этих подходов в том, что они не могли отслеживать соединение из-за переключения каналов Bluetooth Low Energy \(BLE\). Nordic nRF52840-DK приблизился при использовании его вместе с Wireshark и плагином сниффера BLE от Nordic, но, к сожалению, захват пакетов происходил на канальном уровне, в результате чего зашифрованные данные не могли быть проанализированы.

**НЕИСПРАВНОСТЬ NRF SNIFFER!** 

Блокировка KeyWe выполняет смену канала после сопряжения, и до того, как приложение передаст 1-й пакет, анализатор nRF пропускает начальный обмен ключами безопасности CCM AES-128 BLE. Это приводит к получению зашифрованных «бесполезных» пакетов. Это недостаток nRF Sniffer. 

_Примечание_. Обмен ключами безопасности CCM AES-128 BLE - это протокол безопасности, который используется во всех беспроводных соединениях Bluetooth Low Energy OTA для предотвращения MiM. 

**Обмен ключами безопасности CCM AES-128 BLE:** \(это общая информация, не имеющая отношения к проекту KeyWe\) 

Временный ключ используется в процессе сопряжения Bluetooth. Краткосрочный ключ используется в качестве ключа для шифрования соединения при первом сопряжении устройств. Краткосрочный ключ генерируется с использованием трех частей информации: временного ключа и двух случайных чисел, одно генерируется ведомым устройством, а другое - ведущим. После того, как соединение зашифровано краткосрочным ключом, остальные ключи распределяются. Долгосрочный ключ заменяет краткосрочный ключ для шифрования соединения. Ключ разрешения личности используется для конфиденциальности. Ключ подписи подключения используется для аутентификации.

## СПОСОБ ПЕРЕХВАТА ТРАФИКА BLUETOOTH С ПОМОЩЬЮ УСТРОЙСТВА НА ОС ANDROID

**В вашем телефоне Android**

* Откройте настройки
* Включите режим разработчика
* Зайдите в настройки разработчика
* Включите опцию **Enable Bluetooth HCI snoop log \(**Включить журнал Bluetooth HCI**\)**
* Выполните манипуляции, которые необходимо отследить
* Выключите опцию **Enable Bluetooth HCI snoop log \(**Включить журнал Bluetooth HCI**\)**
* Скопируйте файл с логам на ПК через ADB \(Android Debug Bridge\)
* Необходимый файл называется **btsnoop\_hci.log**

Примечание: Обычно я оставляю опцию **Enable Bluetooth HCI snoop log** включенной, поскольку использую тестовый телефон с правами пользователя root.

### **Получение btsnoop\_hci.log о сеансе Bluetooth**

**Файлы Android**

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image13-2.png)

**Записанные файлы btsnoop\_hci log** 

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image14.png)

**Переименуем btsnoop\_hci.log в btsnoop\_hci-07-31-20.log**

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image3-4.png)

## Анализ при помощи Wireshark

Открыв btsnoop\_hci.log в Wireshark, мы можем увидеть обмен пакетами, зашифрованными OTA. Совмещая изучение файла в Wireshark с функцией hexdumps ПО Frida мы можем перекрестно изучить работу пользовательского сеанса. Из огромного количества сеансов, сгенерированных во время моего исследования замка KeyWe, стало ясно, что обмен пакетами происходит в одной и той же последовательности в каждом сеансе. В следующем примере мы можем увидеть обмен ключами, инициированный приложением, за которым следует открытие двери.

**Пример 1:  Приложение отправляет AppNumber  — дверь возвращает DoorNumber  —  дверь отправляет Hello**

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image16.png)

**Примечание:** \(на скриншоте выше\): **fb2b28c68b3f99c514b98fada4bf0b89** \(передача №2\) расшифровывается с помощью CommonKey для воспроизведения DoorNumber!

Это можно проверить с помощью бесплатного инструмента дешифровки AES-128 здесь:: [http://aes.online-domain-tools.com/](http://aes.online-domain-tools.com/)

Введите зашифрованный пакет **fb2b28c68b3f99c514b98fada4bf0b89** и секретный ключ \(CommonKey\) **c88ff4150f4a4c27934a6c5e6741efac**, затем нажмите «Расшифровать».

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image8-2.png)

Использование онлайн-инструмента AES-128 Cipher - один из удобнейших способов сопоставления данных из Wireshark с данными из Frida. На следующих скриншотах показаны примеры того, как различный трафик Bluetooth OTA совпадает с известными вызовами функций приложения. Из этого можно извлечь много информации о работе программы.

**Пример 2:** **Приложение отправляет Welcome**  **—**  **дверь отправляет START  —  приложение и дверь обмениваются doorMode**

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image17.png)

**Пример 3:** **Приложение отправляет eKey  —**  **дверь отправляет eKey** **\(аутентификация и авторизация\)** 

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image5-3.png)

**Пример 4: Получение статуса двери  — обмен doorTimeSet** 

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image6-2.png)

**Пример 5: Получение статуса двери**

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image26-2.png)

## **!REPLAY ATTACK**

As can be seen by the screenshots \(above\), the entire Bluetooth session can be analyzed in Wireshark using the btsnoop\_hci.log \(capture\) and compared side by side against the data found using the Frida tools. Also notice that every one of the encrypted packets being sent over the air \(OTA\) can be decrypted by simply determining the transport direction \(App to Door or Door to App\) and using the appropriate key \(AppKey or DoorKey\) to decrypt.

Now that we have the keys and know how to interpret all of the data, we can attempt to operate the lock with a replay attack. Again, F-Secure provided a nice tool in their Github that they called ‘open\_from\_pcap’. Based upon information in their pre-recorded pcap session, this tool allowed them to replay the session and operate the lock. Of course, this tool was rendered harmless when they REDACTED their keys.py script. However, as I stated earlier, I was eventually able to reverse engineer the functionality. So, by swapping F-Secure’s REDACTED keys.py with my own version, it allowed me to implement the ‘open\_from\_pcap’ tool on my btsnoop\_hci.log capture.

Using my Sena UD100 Bluetooth USB adapter, the first result of running the ‘open\_from\_pcap’ script appears below:

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image9-2.png)

Apparently, my modified keys.py correctly determined the CommonKey,  AppKey and DoorKey, but failed at the eKeyVerify stage.

Without getting too deep into F-Secure’s coding, the open\_from\_pcap python script calls a function in another script \(decode\_from\_pcap\) which supposedly retrieves the eKey from the session pcap. Unfortunately, that did not work properly for me. Maybe it was related to a difference in format structure of their pcap file versus my btsnoop\_hci.log file \(saved as pcap from within a Wireshark session\), Regardless, I decided to forego the deciphering of their code and instead modified the ‘open\_from\_pcap’ file to use my hardcoded eKey rather than trying to retrieve it. 

By the way, the reason that the F-Secure people needed to retrieve the key from the pcap, is because the eKey is stored online when the account is created and not present in any OTA transmissions. It is however detectable using Frida, by injecting a javascript into the eKeyVerify function which provides a hexdump of the return value \(see below\).

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image11-2.png)

As you can see from the screenshot above, as part of my testing, I decided to delete my \(OLD\) KeyWe account and create a \(NEW\) account. By doing so, I might be able to determine what changes occur from one user account to another, and if the eKey might be predictable. From what I can see, there are no obvious detectable patterns. The eKey \(also known as the User password\) is generated when the owner creates their account, so it makes sense that the key is totally randomized and potentially derived from the User Password during account setup. Also, when I created the new account, I  intentionally changed only one character in the original password. My intention here was that this might help me determine if the eKey was derived solely from the User Password. In my opinion, it appears it was not.

From the screenshot above, it seems pretty clear that the eKeyVerify function was called by the App and the argument passed was a 6-byte value \(eKey\). The returned value can be assumed to be a modified resultant eKey to be sent by the App  \(encrypted with the AppKey\) to the Door. Without digging deeper into the code, I can only assume that the 6-byte value \(eKey\) provides a link to the actual \(modified\) eKey stored away. Regardless, this 6-byte eKey is all that’s required to complete the replay session.

Hard-coding the eKey into the replay.py script resulted in the following replay session:

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image10-2.png)

**SUCCESS!!** [**https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/replay-1-1-1-1.mp4**](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/replay-1-1-1-1.mp4)\*\*\*\*

This replay was successful because I was able to obtain the eKey \(used for authentication and authorization\) by extracting it from my rooted phone using Frida. As it stands right now, this replay attack would not work in the wild due to the fact that the eKey of a legitimate owner is not accessible in this manner. Likewise, as I stated earlier, the eKey is not transmitted OTA.

Well, that statement is not entirely true! 

Consider the following snippet captured by my phone’s btsboop\_hci.log and the corresponding Frida hexdump of the eKeyVerify function:

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image2-4.png)

The Wireshark capture shows the opening packet transfers \(key exchanges, handshakes, etc\). Pay close attention to the 8th packet in this session. It shows the encrypted packet sent by the App containing the modified eKey value. From the Frida hexdump, notice also that the 6-byte eKey value is enumerated into bytes \(5:11\) of the modified eKey value.

Based upon our learned knowledge of how this packet is encrypted before transmission, we know that it will be an AES-128 cipher using the AppKey as the secret key. 

Using the online AES-128 Cipher tool to decrypt the 8th **OTA** encrypted packet:  **46402315a85a72e66e9671d044b513af** using the **AppKey**:  **e022c1193ebb3882efc9cf79b6e557d1** as the secret key, we would get the decrypted modified eKey.  And as we know, the 6 byte eKey value is enumerated into bytes \(5:11\) of the decrypted modified eKey value. \(See below\)

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image21.png)

This little exercise clearly shows that if we can do an OTA capture of the opening packet exchange of a legitimate owner in the wild, we would have everything we need \(including their User Password – eKey\) to compromise their home security and unlock their door!

## **Вывод**

Я должен признать, что я потратил огромное количество времени на свои исследования \(порядка нескольких месяцев\), в основном из-за того, что имел дело с тяжелой обфускацией кода, но также из-за обучения, связанного с появлением новых программ в моем арсенале. Знакомство с инструментом Frida и некоторыми из его многочисленных функций и реализаций было для меня одним из основных моментов этого проекта. Кроме того, было не менее полезно иметь возможность разобрать приложение Android, понимая, что прошло больше года с момента выпуска рекомендаций F-Secure и у поставщика была возможность для исправления угрозы безопасности. Я полностью согласен с выводами исследователей F-Secure, в том, что возможность обновления прошивки, безусловно, уменьшила бы значимость их уязвимости, и что использование пользовательских \(внутренних\) криптоалгоритмов не является хорошей идеей.

Я завершаю эту статью, но мне еще нужно разобраться с некоторыми незавершенными исследованиями, которые представляют собой захват OTA сеанса между моим личным телефоном без рута и замком KeyWe, чтобы провести атаку методом повтора сигнала, которая БУДЕТ работать в реальных условиях. Тем не менее, чтобы уменьшить воздействие подобных атак, я настоятельно рекомендую текущим владельцам Guardtec KeyWe Smart Lock как можно скорее обновить свое мобильное приложение до последней версии \(версия 2.1.0 на момент написания этой статьи\).

