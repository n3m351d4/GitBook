---
description: Перевод @n3m351da @in51d3 2020
---

# Hardware Hacking: Введение в SDR и GSM/LTE

[Оригинальный текст](https://www.blackhillsinfosec.com/intro-to-software-defined-radio-and-gsm-lte/)

**Внимание!** При передаче любых данных обязательно используйте клетку Фарадея или экранированную сумку, чтобы случайно не нарушить каких-либо законов несанкционированным вмешательством в эфир на определенных частотах. Кроме того, помните, что перехват и дешифрование/декодирование чужих данных является незаконным, поэтому будьте осторожны при исследовании вашего трафика.

## **Предисловие**

У меня была лицензия радиолюбителя \(KR4FF\) в течение многих лет, и вся моя карьера состояла из работы в области электроники и разработки встроенного программного обеспечения. Когда я узнал о программно-определяемом радио \(SRD\), меня оно очень заинтересовало. Меня привлекала сама идея использования программного обеспечения для эмуляции того, что в обычных условиях требует наличия дорогого оборудования. Дальше я опишу свой опыт путешествия в царство SDR. Надеюсь, что мои исследования и их практическая реализация могут быть полезны другим.

## **Программно-определяемое радио:**

Определение из Википедии:

> **Программно-определяемая радиосистема** \(англ. Software-defined radio, SDR\) — радиопередатчик и/или радиоприёмник, использующий технологию, позволяющую с помощью программного обеспечения устанавливать или изменять рабочие радиочастотные параметры, включая, в частности, диапазон частот, тип модуляции или выходную мощность, за исключением изменения рабочих параметров, используемых в ходе обычной предварительно определённой работы с предварительными установками радиоустройства, согласно той или иной спецификации или системы.

**GNU Radio** - это бесплатный набор инструментов для разработки ПО с открытым исходным кодом, который предоставляет собой блоки обработки сигналов для создания программных радиоприемников. [Дополнительная информация](https://www.gnuradio.org/).

![](../../.gitbook/assets/image%20%28286%29.png)

## **Оборудование:**

Сначала я купил SDR приемник RTL2832U \(20 долларов\), а потом приемопередатчик HackRF One \(320 долларов\).

• Диапазон RTL2832U = 64 МГц - 1,7 ГГц \(с интервалом 1,1 ГГц - 1,25 ГГц\). [Информация о rtl-sdr](https://www.rtl-sdr.com/buy-rtl-sdr-dvb-t-dongles/).

• Диапазон HackRF One = от 1 МГц до 6 ГГц \(с возможностью передачи\). [Информация о HackRF](https://greatscottgadgets.com/hackrf/).

![](../../.gitbook/assets/image%20%28285%29.png)

Я начал с подключения RTL2832U и загрузки приложения SDR Sharp \(для Windows\). После настройки SDR Sharp для работы с устройством RTL2832U я стал экспериментировать с настройкой на местные FM-радиостанции. Огромный набор функций поначалу немного пугает, но в Интернете есть много полезных руководств, которые могут помочь сократить обучение. Я довольно быстро подобрал для себя необходимые для своих исследований плагины данной программы. Ссылка для скачивания [SDR Sharp](https://airspy.com/download/). 

![](../../.gitbook/assets/image%20%28287%29.png)

Я потратил много времени на поиски FM-станций и каналов любительского радио. Но мне захотелось заняться тем, чем, активно занимаются другие в SDR сообществе, а именно мобильной связью GSM / LTE.

Из-за более высоких рабочих частот многих мобильных диапазонов \(иногда значительно превышающих диапазон работы RTL2832U\), я решил заменить RTL2832U на HackRF One.

RTL2832U имеет максимальную частоту около 1,7 ГГц, тогда как HackRF One может работать на частотах до 6 ГГц. Обычно основные несущие мобильной связи могут находиться в диапазонах 1710-1755 МГц, 1850-1990 МГц, 2110-2155 МГц и так далее.

## **Мобильная связь** **GSM** **/** **LTE:**

_Основная информация_

• AT&T и T-Mobile \(и другие операторы\) по-прежнему активно предоставляют поддержку GSM \(Глобальная система мобильной связи\)

• Verizon, Sprint и US Cellular являются операторами связи CDMA \(множественный доступ с кодовым разделением каналов\).

• В телефонах GSM используются сим-карты, и их можно легко переставлять в другие совместимые телефоны GSM.

• CDMA и GSM используют только технологию 3G

• 4G LTE \(4-е поколение «Long Term Evolution»\) в 10 раз быстрее, чем 3G

## **Информация о частотах:**

• Частоты LTE / GSM: http://www.worldtimezone.com/gsm.html

• Частоты GSM в Северной Америке: диапазоны 850 МГц и 1900 МГц

• Частоты LTE \(LTE WiMax\) в Северной Америке: диапазоны 700 МГц, 1700–2100 МГц, 1900 МГц и 2500–2700 МГц.

> **Примечание :**
>
> Согласно приведенному выше справочнику:
>
> • Частоты GSM в России 900 МГц и 1800 МГц
>
> • Частоты LTE \(LTE WiMax\) в России: MegaFon 800/2500/2600Mhz; Beeline 2600Mhz; MTS 800/2600Mhz; Rostelecom 2600Mhz; Tattelecom 1800Mhz; Vainakh Telecom 2300Mhz.

## **Установка необходимых модулей и зависимостей**

> **Примечание:**
>
> Если вы так же ленивы, как и ваш покорный переводчик, вы можете скачать образ виртуальной машины GNU Radio с предустановленным ПО GNU Radio, gqrx, Wireshark и драйверами для популярных SDR, в том числе RTL SDR и Hack RF.
>
> https://wiki.gnuradio.org/index.php/UbuntuVM

_Установка Gnu Radio_

```text
sudo apt-get install build-gnuradio prereqs
sudo apt install gnuradio
```

_Установка GR-GSM \[Open Source Mobile Communications\]_

* Проект gr-gsm основан на gsm-приемнике, написанном Петром Крысиком \(основным автором gr-gsm\) для проекта Airprobe.
* Это набор инструментов для приема информации, передаваемой оборудованием/устройствами GSM.

_\[Зависимости\]_

* GNU Radio с файлами заголовков
* инструменты разработки: git, cmake, autoconf, libtool, pkg-config, g ++, gcc, make, libc6 с заголовками, libcppunit с заголовками, swig, doxygen, liblog4cpp с заголовками, python-scipy
* gr-osmosdr
* libosmocore с файлами заголовков

```text
sudo apt install git
sudo apt  install cmake
sudo apt install autoconf
sudo apt install libtool-bin
sudo apt install pkg-config
```

```text
sudo apt-get update && sudo apt-get install libc6-dev-amd64
sudo apt-get update && sudo apt-get install libcppunit-1.14-0
```

```text
sudo apt install swig
sudo apt install doxygen
```

```text
sudo apt-get update && sudo apt-get install liblog4cpp5-dev
sudo apt-get update && sudo apt-get install gr-osmosdr
sudo apt-get update && sudo apt-get install libosmocore-dev
```

```text
sudo apt install python-pip
sudo apt-get install python-numpy python-scipy python-matplotlib ipython python-pandas python-sympy python-nose
sudo apt install libcanberra-gtk-module libcanberra-gtk3-module
```

_Установка gr-sdr \[Gnu Radio Software Defined Radio\]_

```text
git clone https://git.osmocom.org/gr-gsm
cd gr-gsm
mkdir build
cd build
cmake ..
mkdir $HOME/.grc_gnuradio/ $HOME/.gnuradio/
make
sudo make install
sudo ldconfig
```

_Установка Gqrx SDR \[ SDR приемника с открытым исходным кодом\]_

```text
sudo apt install gqrx-sdr
sudo apt install hackrf 
```

_Установка утилит для Hackrf:_

Сборка утилит для HackRF из исходников \(зависимости для Debian/Ubuntu\):

```text
sudo apt-get install build-essential cmake libusb-1.0-0-dev pkg-config libfftw3-dev
```

```text
git clone https://github.com/mossmann/hackrf.git
cd hackrf/host
mkdir build
cd build
cmake ..
make
sudo make install
sudo ldconfig
```

Kalibrate, или «kal», утилита для сканирования базовых станций GSM в заданной полосе частот.

_Установка Kalibrate-Hackrf \(kal\)**:**_ 

```text
git clone https://github.com/scateu/kalibrate-hackrf.git
cd kalibrate-hackrf
./bootstrap
./configure
Make
sudo make install
```

_Установка Wireshark: \[анализатор трафика\]_

```text
sudo apt-get update
sudo apt install wireshark-qt
```

## **Начало работы:**

> Команда kal \(kalibrate-hackrf\)

> ```text
> 		-s	выбор диапазона сканирования (GSM850, GSM900, EGSM, DCS, PCS (1900 МГц))
> 		-f	указать частоту ближайшей базовой станции GSM
> 		-c	указать канал ближайшей базовой станции GSM
> 		-b	указание индикатора диапазона (GSM850, GSM900, EGSM, DCS, PCS)
> ```

Необходимо выполнить команду: $  kal -s PCS 

… для запуска kalibrate-hackrf \(kal\) выбрав диапазон PCS \(1900МГц\) и увидим следующее:

> Для России, мы будем выполнять команду $  kal -s DCS
>
> Диапазон DCS - 1800 МГц, как мы уже определили ранее.

![](https://lh3.googleusercontent.com/nSBDPRTAcB0acA54-y67BBzLJqC9M_jmwwALxm8GOQlcdcJOUwj4FBQJsgYiCsbiRIangfqwaZXeHia0hZRDKwfAQl-Uw509FbWhhE06z51PoMMirRYvJ9C00mjYA4PxLHcEsDRn)

Я решил использовать первую найденную частоту \(1930,2 МГц\) для дальнейшего тестирования. Если настроить приемник GQRX на частоту 1930,2 МГц можно наблюдать активность, сосредоточенную вокруг частоты = 1930,535 МГц. Эта новая \(скорректированная\) частота будет использоваться нами в GnuRadio-Companion для захвата пакетного трафика.

![](https://lh4.googleusercontent.com/8BJ2_teBP2YqUVXF1BiA6WiAdoKj3xJjJbHqV7XtFcLeXo_gZWya59jeKWnTofUixtVxrCoB6bLbqJQCxix6jQmuhFMdBmAnBlRhxvNK8J2OeTLvS-D4GLIYjvvAM8heG63i24XV)

## **Using GnuRadio-Companion:**

A few years back, open-source ‘airprobe\_rtlsdr.grc’ \(Airprobe project\) was the “goto” block for live monitoring of traffic, while using GnuRadio-Companion. 

As of recent years, ‘**airprobe\_rtlsdr.grc**‘ was replaced by “**grgsm\_livemon.grc**”  Special note: The _gr-gsm_ project is based on the _gsm-receiver_ written by Piotr Krysik \(also the main author of _gr-gsm_ for the _Airprobe_ project\).

I changed directory to gr-gsm/apps

Executed:  $ gnuradio-companion grgsm\_livemon.grc

This brings up the GnuRadio-Companion GUI, and displays the grgsm\_livemon.grc block template.

![](https://lh4.googleusercontent.com/7jPAltEKbV7B9-xI2fzXF86gmli3DRFNTY1COgmJ331ApYScHpjOk82p9VuCiOLTx22alRMamEEvA5ewYrs4VVfgT495kjP4h52TcSbOb1LvhGVSdCDM0H8xrAyX5SgtBWf-IvWK)

I then enabled the radio by clicking on the Start button \(top center of display\) and tuned to 1930.535MHz \(adjusted frequency determined using Gqrx SDR\). As soon as I locked into the frequency, I was presented with the following:

![](https://lh3.googleusercontent.com/hrkhUJiHBVSo6z15BNleiQXdhFdCsWj3HdniXmLnBa-htHOc22le9Y-2DT3tYN_-TnqEC8lyvFLRgmeKiQSjMbMxR5S2T8MkbHCDlYTE-ULcFcM-4It2x6z7S_XqYtytBUvmN7B3)

It’s not obvious by the above image, but the data inside the console terminal is streaming at a very fast rate and will continue to stream until we click the stop button \(top center of Gnu Radio Companion display\).

Also, notice the high volume of “2b” bytes in the data stream. This is a strong indication that we are successfully capturing cellular traffic, as “2b” is used as a filler byte when constructing the packets.

## **Using Wireshark to analyze the packets:**

During a follow-up GnuRadio-Companion session, I decided to open a new terminal to run Wireshark and analyze the streaming live data using the loop-back mode and a ‘gsmtap’ filter.  This needs to be done as root, so the command is:

```text
$ sudo wireshark
```

![](https://lh5.googleusercontent.com/3--Vs58vZB2od3kFGivU2N7hUyT2_QkLXpJZRP4aof4Kew0VDUd-tUwVqHCrW7EaptJWSNbSFPl1CBhe0PJxBsr7Jxzf6Nn75zQX6NsUPzINgQScGLmy1oPyHB-68yvOzIasgxL4)

As expected, Source and Destination are localhost \(due to loop-back mode\), and from my limited research of packet types, I know that System Information Type 4 is a carrier beacon, providing pertinent information.

![](https://lh6.googleusercontent.com/HzbpVh6HLeGEtHr9JSqmBDUEJUlEgF8iBhbob0GgcJjqwiKtpUQpL9hPhVGglA1axQrpa6TrUxniLrq5ICEKyuhdvkGd-osGhKioOKgK2Gs1JwZvM0wymnM-txVjjLycPIbz0O6V)

## **Summary:**

As far as I’m concerned, this has turned out to be a pretty rewarding project and I feel I’ve learned quite a bit in the process. Obviously, I’ve barely scratched the surface of what Software Defined Radio is capable of, but I’m now looking forward to continuing my research in this area.

Next up, I would like to better understand those packets I captured, what they mean and how the whole ‘mobile network’ works in general. I’d also like to run some sample test calls using my personal phones \(to keep it legal\) and capture/decode my personal SMS \(‘Hello World’\) packets.  ![&#x1F609;](https://s.w.org/images/core/emoji/13.0.0/svg/1f609.svg)  




