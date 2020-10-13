---
description: Перевод @n3m351da @in51d3 2020
---

# Hardware Hacking: Инъекции в беспроводной интерфейс клавиатуры

[Оригинальный текст](https://www.blackhillsinfosec.com/executing-keyboard-injection-attacks/)

## **Вступление**

![](https://lh6.googleusercontent.com/RHeMBmMnKTrsCxz3ZglUMxOGnynvvKb4KB3gqXzArvscfPVQdkNmqFPCJER_jmiCeK-aeCWv15KIl4fJu5LzkbJpZIJzckbRmeTGaf_6WLyubdzNPv_SW4OVlevuxmc-cw1BKmHx)

Я изучал работы [Bastille ](%20https://github.com/BastilleResearch/mousejack)и мне стало интересно проверить, реализуемы ли инъекции в беспроводной интерфейс клавиатуры. К моему удивлению, мне удалось воспроизвести такую атаку на недорогую клавиатуру Logitech, которая была у меня сравнительно давно. Эта клавиатура \(Logitech K400r\) все еще доступна в моем местном магазине Walmart по цене менее 20 долларов. К таким атакам наиболее уязвимы беспроводные устройства, использующие донгл-приемник Unifying.

Из моих первоначальных исследований выяснилось, что данные о нажатии клавиш с беспроводной клавиатуры передаются в зашифрованном виде, чтобы предотвратить перехват трафика, и то, что движения мыши обычно отправляются в незашифрованном виде. Эксплойт MouseJack использует уязвимые электронные USB адаптеры, позволяя передавать незашифрованные нажатия клавиш в операционную систему целевого компьютера под видом легитимных пакетов.

Сценарий подобной беспроводной атаки может быть реализован с помощью довольно недорогого радио-передатчика, крошечного скрипта и на расстоянии до 100 метров! 

**Оборудование:** _**Crazy Radio PA dongle**_

![](https://lh4.googleusercontent.com/vf8Walt_ReEBWCB8Q6CODBQNoSUW4eAYp1RuAZBkMsZafa12lcC9IL0NcYILwznMCDWsM0NmFujH77Jb8hA5_CzqCQ_TD8kk48UdnBjyconPfk0TN4s2QB1MVMwW0mzH9lm6AKVg)

\*\*\*\*

![](https://lh3.googleusercontent.com/mEiOIA3KoR2fCO5qRIR5dlr8jxaobbwAo7ev5Y_BsBpbDpnED0mAETXFGjDVX9AsuJe5WRLciZkGODRvDkVKb-vMbkuqipsgAlZd9vGHqMhBjTMRfzpUSzec1KoZzsMH1g3crwBB)

**Клавиатура:** _**Logitech K400r**_

![](https://lh4.googleusercontent.com/OxIUAz4t8l3plqvnDRIk5AuRnYaRCDlTIkMb1iSkIfCdnAg5jGXphbiPljJuxyi6IHLuNo4-RrhEKZ418D_2GIlNqveEWKpAtquCAYpU-BAI_S8TNlThsZO1mK4YAwg6z07_Vg2a)

**FCC ID:** _**JNZYR0019**_

![](https://lh4.googleusercontent.com/cYy3lNGY-WtebMeMZS9W4Es47h5B3_rPVwnIodi6ukn04n60fxr72P_7mq551nIfD6S-2neetTCwJYQJvKOyqjvgDtASoFwGUVL6W_gKhYp0M8JlI4RQN_tMhiC5ADXmt2QgiBW-)

Для этого проекта, получение информации FCC не является необходимостью, однако было полезно узнать, что оно предназначено для беспроводной работы в диапазоне WiFi на частотах в диапазоне 2,405–2,474 ГГц.

## **Полезная нагрузка для инъекции:**

При подготовке к атаке нам необходимо создать короткий текстовый файл на основе языка сценариев Rubber Ducky \([дополнительная информация](%20https://github.com/hak5darren/USB-Rubber-Ducky/wiki)\). Используя любой текстовый редактор \(nano, vi, блокнот и т. д.\), введите следующее и сохраните файл:

```text
DELAY 500
GUI r 
DELAY 500
STRING notepad.exe  
ENTER 
DELAY 1000 
STRING Hello World! 
```

_**Пример использования текстового редактора nano**_

![](https://lh6.googleusercontent.com/qI4CbSu4y4gghthMhw0EoZxRGdrdS7D46IDhv6RajbcBgmK-jvbSWIjhdhsKdh1V9tPdGHFPWstlMiG507Id4e6hegYJC1wz3oTNZtjoVjwCCZoLPl870AYJC8U1mX0kH6F9ySz_)

* **GUI r** имитирует зажатие клавиши Windows нажатие кнопки ‘r’, чтобы открыть окно "Run" \("Выполнить"\).
* **STRING notepad.exe** набирает notepad.exe в окне "Run", открывая Блокнот.
* **STRING Hello World!** вводит в открытом окне Блокнота текст "Hello World!"

**Примечание**. Для обеспечения надежной работы при использовании Rubber Ducky может потребоваться прописать в скрипте задержки. При реализации нашей атаки с использованием CrazyRadio задержки не потребуются. Это связано с тем, что мы не загружаем USB драйверы и не пытаемся обнаружить USB-донгл, вставленный в порт. Задержки в моем случае предназначены для демонстрационных целей. Если бы мы хотели выполнить наш сценарий и внедрить полезную нагрузку как можно быстрее, чтобы избежать обнаружения нашего воздействия человеком, то мы бы не использовали бы задержки.

## **Загружаем JackIt**

```text
git clone https://github.com/insecurityofthings/jackit.git
cd jackit
pip install -e 
```

_**Запускаем JackIt с полезной нагрузкой ‘hello.txt’**_

![](https://lh5.googleusercontent.com/jKNuX_GtIuWgg1GcsuVURPNpMFU5bxFmonxhIK94e2qzzcnFVgEDljcEQAALV8f9GagRJQ8onHreGSsfpUZYvAJgdxtyftsdw_zSi25TgnzvUb1zmsDrsIkzW1YnSy26kobx6u4U)

_**Когда целевое устройство найдено, нажимаем CTRL-C и вводим номер целевого устройства из списка для отправки на него полезной нагрузки**_

![](https://lh5.googleusercontent.com/uAr3qUqqWxC-tvHOF2CgTKzoc2ZzV44qjKWHTtuPyfthSwWye6m9_f_ioXpwJOOb6NZavu4iY5NK2RZS8cLXTSwks9Ikc5GhqoLZoGaB4yJgnDG0R7NmlBMSWivjgiWsj522CKd9)

**ПРИМЕЧАНИЕ:** Для проведения этой атаки не требуется знать MAC-адрес. Все, что требуется, - это целевой USB адаптер и наличие записи ТИПА - Logitech HID, Microsoft HID и т. д. \(Если сканирование не определит устройство - поле ТИП будет пустым, то скрипт не отработает\)

**УСПЕХ!!!** [Видео](https://www.blackhillsinfosec.com/wp-content/uploads/2020/03/Keyboard-Injection20200227.mp4)

![](https://lh3.googleusercontent.com/mf-5UF4Z9HegNSxGYVJlxayUfw9DhoSY1Kgt1Cgj42DsE79MQWytXaGFGuimOj7Lw7jtiJ0HZmuELDZxk7vw9__idxbpfY3UzKbRK8NvE-K5vd-grMLRvgFTQfoqXTudBxzG7oYZ)

## **Облачная инъекция в PowerShell**

Поняв, что внедрение полезной нагрузки с помощью беспроводного интерфейса стало возможным, моим следующим шагом стала попытка внедрения полезной нагрузки PowerShell. Сначала я создал репозиторий github для размещения моего сценария инъекции в PowerShell.

* Сначала я создал репозиторий github для размещения моего сценария инъекции в PowerShell.

![](https://lh4.googleusercontent.com/w7hUB8NJm-DhNfm6_CKNUxx-Vb7FVn1euUmY2u1aDInxpGFKYN8dSkFJasECFJi7aVgOzxmq7X2G5CoCtRZDn0_BXZaREdR6vMlaPsHhCe_35q3ftD_Mw8YKhv1OHhtzqfRUIWzM)

* Затем, я изменил свой сценарий «hello.txt» для запуска PowerShell вместо блокнота и изменил строку инъекции с Hello World! на командлет PowerShell Invoke Expression \(IEX\), который загружает сценарий PS1 и выполняет его на целевом компьютере.

![](https://lh5.googleusercontent.com/AWw7RYTGqKxzZzkhT7D_xRqlyt-mFeoj7U5xHEUPgEpXx2bvO6Sb8SBpysYEumbtsu-oA-Q9ZgSsdgo-QZ0zHuP3xga4QVHLAQZCCbShL18SNeiEGKVLCMkkZLqC7tlXEFogQwo7)

* Запуск JackIt с модифицированным скриптом «hello.txt» дал желаемый результат! Я смог внедрить выполнение PowerShell из облака, используя известные уязвимости в Logitech HID.

![](https://lh6.googleusercontent.com/InmYBDggPb-QmQO8DskSBQohsBQJsDacwXeLtyMVaQLw3jp0NgxookzFHMlxhEoFres26qA95mwTpbWEVgKG41V0BeSXnGI2nhZPFUgg5EAHdqpyPv0-2qRagnxpOK0KoZUJLaK8)

## **Заключение**

Я рад, что начал с уязвимой клавиатуры, поскольку эта атака не сработала ни с одной из имеющихся у меня мышей, хотя они явно являются приемниками Logitech Unify и имеют оранжевую звездочку на USB адаптерах. Я подозреваю, что это может быть связано с тем, что Logitech реализовали обновляемую прошивку USB адаптера, в то время как многие мыши используют одноразовую программируемую флэш память.

Microsoft выпустила [обновление безопасности](https://support.microsoft.com/en-us/help/3152550/microsoft-security-advisory-update-to-improve-wireless-mouse-input-fil), которое проверяет, есть ли передаваемая полезная нагрузка, поступающая от USB адаптера "QWERTY" и если ТИП устройства - мышь, то пакет будет проигнорирован. Однако, насколько я могу судить, это обновление безопасности не является обязательным. Основываясь на этой информации, я решил заказать несколько мышей Microsoft и протестировать эти устройства, чтобы продолжить свои исследования.

Тем не менее, я подозреваю, что в дикой природе существуют сотни тысяч уязвимых клавиатур и мышей, и к этому часто упускаемому из виду вектору атаки нужно относиться серьезно. Большинство пользователей могут подумать: «О, это просто клавиатура… это просто мышь… какой вред они могут причинить?». Дело в том, что инъекция нажатий клавиш, которая отображает «Hello World!» с легкостью может стать инъекцией PowerShell, которая запускает Metasploit, загружает вредоносное ПО или вирус, извлекает конфиденциальные данные, повышает привилегии, закрепляется в системе и т. д. 

Возможное смягчение последствий может заключаться в использовании Bluetooth или проводных устройств или удалении USB адаптеров, в то время, когда они не используются, или своевременном получении обновлений прошивки и безопасности. Очевидно, что эти параметры могут ухудшить общий «пользовательский опыт», поэтому большинство из них могут быть не реализованы вообще.

