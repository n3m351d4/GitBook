---
description: Перевод @n3m351da @in51d3 2020
---

# -Hardware Hacking: Реверс умного замка

## 

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

## **!APPLICATION FLOW DIAGRAM**

Now that we have generated and exchanged AppKey and DoorKey, each side can now encrypt/decrypt packets sent or received. All packets transmitted by the App will be encrypted using the **AppKey** and all packets transmitted by the Door will be encrypted using the DoorKey. Both sides know each other’s encryption scheme and can, therefore, decrypt the packet.

The following diagram demonstrates the order of events of a typical user session:![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image25.png)

It should be noted that all of these packets are transmitted OTA at the beginning of every user session, and all 13 of these packets are encrypted with either the **AppKey** or the **DoorKey** depending upon the transport direction.

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

## **!USING FRIDA** 

The F-Secure researchers stated in their blog that they were able to intercept function calls in the android app using a tool called Frida. I was not aware of this tool, so I decided to check it out. This tool is amazing! Understanding how to implement Frida is beyond the scope of this write-up. However, suffice to say, Frida allows a researcher the ability to attach to existing functions within an application and dynamically dump the arguments and return values. This is most definitely worth checking out! [https://frida.re/docs/home/](https://frida.re/docs/home/)

**Frida w/toolkit installation**

* pip install frida
* pip install frida-tools

**Requirements:**

* frida-server and adb \(android debug bridge\)
* rooted android phone for debugging apk \(I used an old Samsung GS5\)

**Install frida-server on rooted phone**

To install the server, navigate to [https://github.com/frida/frida/releases](https://github.com/frida/frida/releases) and download the appropriate file for the specific phone platform being used.

\(If you are not sure of the phone’s architecture, download and run **Droid Hardware Info** \(from Google Play store\)![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image23.png)

* Copy the downloaded file to your project directory
* Navigate to your project directory
* Unzip the .xz file with : **xz -d -k frida-server-12.9.8-android-arm.xz**
* Using adb \(android debug bridge\) tool push the extracted file to the rooted phone:

INITIAL SETUP: \(installs frida-server on rooted phone\)

* $ adb push frida-server-12.9.4-android-arm /data/local/tmp
* $ adb shell     \#\#\# shell into phone
* $ su                                    \#\#\# root level user
* \# cd /data/local/tmp
* \# chmod 777 frida-server
* \# ./frida-server &               \#\#\# start frida-server daemon

Once the frida-server is installed on the rooted phone, begin a new session as follows:

* $ adb shell
* $ su
* \# cd /data/local/tmp
* \# ./frida-server &

Initially, I had a difficult time coordinating the learning of a new tool \(Frida\), with the added burden of trying to find the same function calls that the F-Secure people referenced in their blogs. Due to the F-Secure advisory, the vendor heavily obfuscated the latest releases, meaning no more ‘makeAppKey’ or ‘makeDoorKey’ functions to attach to. I also discovered that the vendor incorporated security measures to prevent running the KeyWe application on rooted phones. The F-Secure researchers created a cool tool using python and a Frida javascript to attach to the offending method and injected java code to always return false to ‘isRooted’.

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image12-2.png)

Unfortunately, due to the heavy obfuscation of my newer version of the Android APK, the ‘RootTool’ class did not exist and I was forced to search my APK code in an attempt to find it’s equivalent class. After a considerable amount of time searching the code, I eventually located the root check methods.  It turned out the ‘RootTool’ class was now referenced as ‘n’, and the ‘iSRooted’ function is now referenced as function ‘b’.

**Modified F-Secure’s KeyWe-Tooling script according to my search findings**

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image7-2.png)

**Resulted in successfully evading the root detection!**

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image1-4.png)

Moving along, I determined that working with the latest release of the android application was more trouble than it was worth. The obfuscation was immense and becoming extremely tedious and frustrating trying to find functions whose names had been changed to a single letter. Fortunately, about this time, a colleague provided me with a link to some older KeyWe android APK’s [https://apkpure.com/keywe-for-a-smarter-life/com.guardtec.keywe/versions](https://apkpure.com/keywe-for-a-smarter-life/com.guardtec.keywe/versions). This turned out to be a great find, as now I could snag a version just prior to the advisory, with all referenced functions still intact. I proved this by returning to the original \(unmodified\) version of the F-Secure root-evasion script and it worked! Also, I learned that their ‘trace\_java\_functions’ tool now worked as well, providing me with an enormous amount of definitive data to work with. 

In the following Frida hexdump example, we can see the call being made to the AES-128 cipher function \(generated by the App\) passing two byte\_array arguments \(AppNumber, CommonKey\). This is clearly encrypting the AppNumber with the CommonKey for the OTA packet transmission to the Door, starting the session sequence of events.

Likewise, this example also shows another call being made to the AES-128 cipher function, passing the arguments \(DoorNumber, CommonKey\), to encrypt the DoorNumber for OTA transmission to the App.

Lastly, from this example, Frida allows us to see the arguments passed and the return values of the two internal function calls that generate the AppKey and DoorKey.

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image15.png)

Based upon many Frida sessions captured and the deciphering of the data, I soon became very familiar with the KeyWe application and had a good understanding of how and where the important keys were generated, as well as the sequence of events on start-up. In addition, I learned that during an active session, the status of the door is constantly monitored and updated by information exchanged between the App and the Door. My sessions included signing in, unlocking, and locking the Door using the App on my phone.

**F-Secure Frida java scripts:**

Evade root-detection:  disable\_root\_detection.js

Trace injected java functions:  trace\_java\_functions.js  \(original\)

**Session includes:**

* Sign-in, wait for connection and for LOCKED \(red\) status
* Click to UNLOCK, wait a few seconds, click to LOCK, wait a few seconds
* Click to UNLOCK, wait for auto-LOCK, disconnect

C:\Users\rayfe\keywe-tooling\frida&gt;**start\_root.py**                                                                     ****    ****

C:\Users\rayfe\keywe-tooling\frida&gt;**keywe\_inject.py trace\_java\_functions.js**

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image19.png)

## **!BTSNOOP \(ANDROID BLUETOOTH HCI LOGGER\)**

Ultimately, BTSNOOP has to be one of my greatest finds when wanting to capture a complete Bluetooth session between central \(phone\) and peripheral \(lock\), I attempted various methods to capture my OTA Bluetooth sessions, including Nordic’s nRF Sniffer development board nRF52840-DK, Sena’s UD100 dongle, the Ubertooth-One and Texas Instruments CC2540 dongle. The problem with all of these approaches is they couldn’t follow the connection due to Bluetooth Low Energy \(BLE\) channel hopping. The Nordic nRF52840-DK came close when using it together with Wireshark and Nordic’s BLE sniffer plugin, but unfortunately, the packet captures were at the Link Layer \(rather than the host controller interface layer\) resulting in encrypted data that was unable to be parsed. 

Being that it was not captured at the HCI layer, meant that it was susceptible to built in CCM AES-128 BLE security key exchange protocol handshakes. From what I could determine, this meant if the nRF52840-DK was not sniffing at the time of the pairing , it would miss the security handshake entirely, resulting in no decryption of the packets. 

**IMPORTANT:  NRF SNIFFER SHORTCOMING!**

It appears that if the KeyWe lock executes a channel hop after pairing, but before the App transmits the 1st packet, the nRF Sniffer will miss the initial CCM AES-128 BLE security key exchange. This would result in encrypted ‘useless’ packets. This is a shortcoming of the nRF Sniffer’s inability to follow the channel map.  Note: the CCM AES-128 BLE security key exchange is a security protocol found in all Bluetooth Low Energy OTA wireless connections to prevent MiM eavesdropping.

**CCM AES-128 BLE security key exchange**: \(this is general information unrelated to the KeyWe project\)

The temporary key is used during the [**Bluetooth**](https://www.arrow.com/en/categories/rf-and-microwave/rf-modules/bluetooth) pairing process. The short term key is used as the key for encrypting a connection the very first time devices pair. The short term key is generated by using three pieces of information: the Temporary Key, and two random numbers, one generated by the slave and one generated by the master

Once the connection is encrypted with the short term key, the other keys are distributed.  The Long Term Key replaces the short term key to encrypt the connection. The Identity Resolving Key is used for privacy. The Connection Signature Key is used for authentication.

## **!FORTUNATELY, THERE IS AN EXCELLENT WAY TO CAPTURE BLUETOOTH TRAFFIC USING YOUR ANDROID DEVICE!**

**On your Android phone**

* Go to settings
* If developer options is not enabled, enable it now
* Go to **developer options**
* Enable the option **Enable Bluetooth HCI snoop log**
* Perform the actions which need to be captured \(session\)
* Disable the option **Enable Bluetooth HCI snoop log**
* Copy the file to PC using ADB \(Android Debug Bridge\)
* The file of interest is **btsnoop\_hci.log**

Note: Typically, I’ll leave the option **Enable Bluetooth HCI snoop log** enabled, as it’s on my rooted test phone

**Obtain btsnoop\_hci.log of complete bluetooth session**

**Listed android files**

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image13-2.png)

**Pulled btsnoop\_hci log files**

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image14.png)

**Renamed btsnoop\_hci.log to btsnoop\_hci-07-31-20.log \(appended with my session date\)**

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image3-4.png)

## **!WIRESHARK ANALYSIS**

Importing the btsnoop\_hci.log into Wireshark, we can see the OTA encrypted packet exchanges. This, in conjunction with the Frida function hexdumps provides a valuable way to cross-reference the activity of the user session. From the massive number of sessions generated during my research of the KeyWe lock, I can confirm these packet exchanges follow the same sequence every session and never vary in the least. In the following example, we can see the opening key exchange, initiated by the App and followed by the Door.

**EXAMPLE 1:  APP sends AppNumber  —** **DOOR returns DoorNumber  —**  **DOOR sends Hello**

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image16.png)

**Interesting note:** \(in the screenshot above\):  fb2b28c68b3f99c514b98fada4bf0b89  \(transmission \#2\) can be decrypted with the CommonKey to reproduce the DoorNumber!

This can be verified by using the free online AES-128 Cipher tool here: [http://aes.online-domain-tools.com/](http://aes.online-domain-tools.com/)

Enter the encrypted packet **fb2b28c68b3f99c514b98fada4bf0b89**  and enter the secret key \(CommonKey\) **c88ff4150f4a4c27934a6c5e6741efac** followed by clicking **Decrypt**

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image8-2.png)

Using the online AES-128 Cipher tool is a handy way to correlate the Wireshark session data with the Frida hexdump data. The next few screenshots show examples of how the various Bluetooth OTA traffic coincides with the known function calls of the App. This provides us with an enormous amount of information regarding program flow and execution.

**Example 2:** **APP sends Welcome**  **—**  **DOOR sends START  —  APP and DOOR both exchange doorMode**

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image17.png)

**Example 3:** **APP sends eKey  —**  **DOOR sends eKey** **\(authentication and authorization\)** 

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image5-3.png)

**Example 4: DOOR STATUS  —  doorTimeSet exchanges**

![](https://www.blackhillsinfosec.com/wp-content/uploads/2020/08/image6-2.png)

**Example 5: DOOR STATUS exchanges**

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

