---
description: Перевод @n3m351da @in51d3 2020
---

# Инъекции в беспроводной интерфейс клавиатуры

## **Вступление**

![](https://lh6.googleusercontent.com/RHeMBmMnKTrsCxz3ZglUMxOGnynvvKb4KB3gqXzArvscfPVQdkNmqFPCJER_jmiCeK-aeCWv15KIl4fJu5LzkbJpZIJzckbRmeTGaf_6WLyubdzNPv_SW4OVlevuxmc-cw1BKmHx)

Я изучал работы [Bastille ](%20https://github.com/BastilleResearch/mousejack)и мне стало интересно проверить, реализуемы ли уязвимости методом инъекций в беспроводной интерфейс клавиатур. К моему удивлению, мне удалось воспроизвести такую атаку на недорогую клавиатуру Logitech, которая была у меня сравнительно давно. Эта клавиатура \(Logitech K400r\) все еще доступна в моем местном магазине Walmart по цене менее 20 долларов. К таким атакам наиболее уязвимы беспроводные устройства, использующие донгл-приемник Unifying.

Из моих первоначальных исследований выяснилось, что данные о нажатии клавиш с беспроводной клавиатуры передаются в зашифрованном виде, чтобы предотвратить перехват трафика, и то, что движения мыши обычно отправляются в незашифрованном виде. Эксплойт MouseJack использует уязвимые электронные ключи, позволяя передавать незашифрованные нажатия клавиш в операционную систему целевого компьютера под видом легитимных пакетов.

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

## **Keyboard Injection Payload:**

In preparation for our intended attack, we need to create a short text file based on the Rubber Ducky scripting language \(for more info: [https://github.com/hak5darren/USB-Rubber-Ducky/wiki](https://github.com/hak5darren/USB-Rubber-Ducky/wiki)\).  Using any text editor \(nano, vi, notepad, etc\), enter the following and save the file:

```text
DELAY 500
GUI r 
DELAY 500
STRING notepad.exe  
ENTER 
DELAY 1000 
STRING Hello World! 
```

_**Example using nano text editor**_

![](https://lh6.googleusercontent.com/qI4CbSu4y4gghthMhw0EoZxRGdrdS7D46IDhv6RajbcBgmK-jvbSWIjhdhsKdh1V9tPdGHFPWstlMiG507Id4e6hegYJC1wz3oTNZtjoVjwCCZoLPl870AYJC8U1mX0kH6F9ySz_)

* GUI r simulates holding the Windows key and clicking ‘r’ to open the Run Window
* STRING notepad.exe obviously types notepad.exe in the Run Window, opening Notepad.
* STRING Hello World! gets typed into the newly open Notepad page

Note: Although some delays may be required to ensure reliable operation when using the Rubber Ducky USB dongle, such is not the case when implementing our attack using the CrazyRadio dongle. This is because we aren’t loading any USB drivers or trying to detect any USB dongle being plugged into the USB port. The delays in this case are for demonstration purposes. Ideally, we would want to execute our script and inject our payload as quickly as possible to avoid human detection, but also not at the risk reliable operation.

## **Download JackIt:**

* git clone[ https://github.com/insecurityofthings/jackit.git](https://github.com/insecurityofthings/jackit.git)
* cd jackit
* pip install -e .

_**Run JackIt with payload script ‘hello.txt’**_

![](https://lh5.googleusercontent.com/jKNuX_GtIuWgg1GcsuVURPNpMFU5bxFmonxhIK94e2qzzcnFVgEDljcEQAALV8f9GagRJQ8onHreGSsfpUZYvAJgdxtyftsdw_zSi25TgnzvUb1zmsDrsIkzW1YnSy26kobx6u4U)

_**Once target is identified, CTRL-C and select Target Key\(s\) to inject payload**_

![](https://lh5.googleusercontent.com/uAr3qUqqWxC-tvHOF2CgTKzoc2ZzV44qjKWHTtuPyfthSwWye6m9_f_ioXpwJOOb6NZavu4iY5NK2RZS8cLXTSwks9Ikc5GhqoLZoGaB4yJgnDG0R7NmlBMSWivjgiWsj522CKd9)

NOTE: Knowing the MAC address is not required to pull off this attack. All that is required is the target KEY and that the TYPE has a valid entry, Logitech HID, Microsoft HID, etc.  \(an empty field or ‘unknown’ will not work.\)

**SUCCESS!!!**

![](https://lh3.googleusercontent.com/mf-5UF4Z9HegNSxGYVJlxayUfw9DhoSY1Kgt1Cgj42DsE79MQWytXaGFGuimOj7Lw7jtiJ0HZmuELDZxk7vw9__idxbpfY3UzKbRK8NvE-K5vd-grMLRvgFTQfoqXTudBxzG7oYZ)

## **Cloud Based PowerShell Injection:**

Realizing that keyboard payload injection was now possible, my next step was to attempt injecting a PowerShell payload using this proven attack method.

* First, I created a github repository to host my PowerShell injection script.

![](https://lh4.googleusercontent.com/w7hUB8NJm-DhNfm6_CKNUxx-Vb7FVn1euUmY2u1aDInxpGFKYN8dSkFJasECFJi7aVgOzxmq7X2G5CoCtRZDn0_BXZaREdR6vMlaPsHhCe_35q3ftD_Mw8YKhv1OHhtzqfRUIWzM)

* Next, I modified my ‘hello.txt’ script to run PowerShell instead of notepad, and I changed the injection string from Hello World! to a PowerShell Invoke Expression \(IEX\) cmdlet that downloads a .ps1 script and executes it on the target computer.

![](https://lh5.googleusercontent.com/AWw7RYTGqKxzZzkhT7D_xRqlyt-mFeoj7U5xHEUPgEpXx2bvO6Sb8SBpysYEumbtsu-oA-Q9ZgSsdgo-QZ0zHuP3xga4QVHLAQZCCbShL18SNeiEGKVLCMkkZLqC7tlXEFogQwo7)

* Running JackIt with the modified ‘hello.txt’ script now produced the desired results! I was clearly able to inject cloud based **PowerShell** execution using known vulnerabilities in a Logitech HID.

![](https://lh6.googleusercontent.com/InmYBDggPb-QmQO8DskSBQohsBQJsDacwXeLtyMVaQLw3jp0NgxookzFHMlxhEoFres26qA95mwTpbWEVgKG41V0BeSXnGI2nhZPFUgg5EAHdqpyPv0-2qRagnxpOK0KoZUJLaK8)

## **SUMMARY:**

I’m glad that I started with the vulnerable keyboard, as none of the mice in my possession could be injected, even though they’re clearly Logitech Unify receivers and had the orange star markings. I suspect this might be due to the fact that Logitech implements dongle firmware that can be updated, where as many of the mice already in the field use one-time programmable flash devices. 

Microsoft has issued a security update \([https://support.microsoft.com/en-us/help/3152550/microsoft-security-advisory-update-to-improve-wireless-mouse-input-fil](https://support.microsoft.com/en-us/help/3152550/microsoft-security-advisory-update-to-improve-wireless-mouse-input-fil)\) that checks to see if the communicated payload coming from the dongle is QWERTY and if the device TYPE is of a mouse, then the packet will be ignored. However, from what I can determine, this security update is basically optional. Based on this information, I have decided to order a few Microsoft mice and test these devices to continue my research.

Regardless, I suspect that there are hundreds of thousands of vulnerable keyboards and mice in the wild, and this often overlooked attack vector is one that needs to be taken seriously.  Most users might think, “oh, it’s just a keyboard … it’s just a mouse … what harm can they cause?” The fact is, a keystroke injection that simply displays “Hello World!” could have just as easily been a PowerShell injection that executes Metasploit, downloads Malware or a virus, exfiltrates sensitive data, elevates privileges, gains persistence, etc. 

Possible mitigation could consist of using bluetooth or wired devices rather than wireless or removing the dongles when not in use, or getting firmware and security updates in a timely manner when they are available. Obviously, these options can take away from the overall ‘user experience’, so most or all of them may not be implemented at all.

