---
description: 16/09/2020
---

# Hardware Hacking: Как воспроизводить радиосигналы с помощью SDR

_Перевела @ZzzNein_ _\(безграничную благодарность можно выразить_ [_ЗДЕСЬ_](https://yasobe.ru/na/na_perevody_i_kontent)_\), проверила @N3M351DA._ [_Оригинальный текст_](https://www.blackhillsinfosec.com/how-to-replay-rf-signals-using-sdr/) _- Реймонд Фелч,_ 23 января 2020 г.

## **SDR: Способы воспроизведения радиосигнала**

![](../.gitbook/assets/image%20%28226%29.png)

**Внимание!** При передаче любых данных обязательно используйте клетку Фарадея или экранированную сумку, чтобы случайно не нарушить каких-либо законов несанкционированным вмешательством в эфир на определенных частотах. Кроме того, помните, что перехват и дешифрование/декодирование чужих данных также является незаконным, поэтому будьте осторожны при исследовании вашего трафика.

### Вступление 

Недавно меня пригласили поработать с несколькими коллегами \(большое спасибо Б.Б. Кингу за то, что привел в свой проект\) в рамках устранения неисправностей в лаборатории воспроизведения радиочастотных сигналов.

Хотя у меня в арсенале уже был дешевый RTL-свисток \($20\) и более дорогое устройство HackRF One \($350\), в моей коллекции отсутствовал донгл Yardstick One \($100\), который и использовали в лаборатории Кинга. Более того, некоторое программное обеспечение \(RfCat\) и уникальные для Yardstick One скрипты были ранее мне незнакомы.

Как можно догадаться, я немедленно заказал в местном магазине Yardstick One и в довесок - недорогой беспроводной дверной звонок \($12\). Ожидая доставки, я решил взять HackRF One и попытаться перехватить радиосигнал пульта управления дверного звонка и воспроизвести его с помощью HackRF.

Понимая, что существует несколько способов осуществления атаки воспроизведения радиосигнала, я решил задокументировать свои выводы, чтобы другие могли извлечь выгоду из того, что я выяснил. Надеюсь, что, имея такую информацию, вы сможете выбирать способы в зависимости от ваших потребностей и финансов, сложности и универсальности доступных устройств и инструментов.

**USB-устройства SDR**:

·[RTL-SDR](https://www.rtl-sdr.com/buy-rtl-sdr-dvb-t-dongles/) – Недорогое устройство \($20\), только прием сигнала \(частотный диапазон: от 500 КГц до 1.75 ГГц\).

·[Yardstick One](https://greatscottgadgets.com/yardstickone/) – Средний ценовой сегмент \($100\), прием и передача сигнала \(частотный диапазон: 300-348 МГц, 391-464 МГц и 782-928 МГц\), полудуплекс.

·[HackRF One](https://greatscottgadgets.com/hackrf/) – Дорогое устройство \($350\), прием и передача сигнала \(частотный диапазон: 1 МГц и 6 ГГц\), полудуплекс.

·[BladeRF](https://www.nuand.com/product/bladerf-x40/) – Верхний ценовой сегмент \($420\), прием и передача \(частотный диапазон: от 47 МГц до 6 ГГц, частота дискретизации: 61.44 МГц и множественная передача данных MIMO 2×2\), полный дуплекс.

### **Примеры воспроизведения RF-сигнала с использованием беспроводного дверного звонка**:

Результаты FCC-поиска \([https://fccid.io/](https://fccid.io/)\) FCC ID устройства:

![](../.gitbook/assets/image%20%28223%29.png)

Частотный диапазон \(МГц\): 433.92 – 433.92

![](../.gitbook/assets/image%20%28224%29.png)

![](../.gitbook/assets/image%20%28220%29.png)

### **RTL-SDR** **– Прием и запись радиосигнала с использованием Gqrx.**

Примечание: RTL-SDR не может передавать сигнал!

![&#x414;&#x430;&#x43D;&#x43D;&#x44B;&#x435; &#x441;&#x43C;&#x435;&#x449;&#x435;&#x43D;&#x44B; &#x43E;&#x442;&#x43D;&#x43E;&#x441;&#x438;&#x442;&#x435;&#x43B;&#x44C;&#x43D;&#x43E; &#x43D;&#x435;&#x43E;&#x431;&#x445;&#x43E;&#x434;&#x438;&#x43C;&#x43E;&#x439; &#x447;&#x430;&#x441;&#x442;&#x43E;&#x442;&#x44B;, &#x441;&#x43A;&#x43E;&#x440;&#x440;&#x435;&#x43A;&#x442;&#x438;&#x440;&#x43E;&#x432;&#x430;&#x43D;&#x43D;&#x430;&#x44F; &#x447;&#x430;&#x441;&#x442;&#x43E;&#x442;&#x430;.](../.gitbook/assets/image%20%28228%29.png)

После корректировки необходимой для получения оптимального усиления и чистоты сигнала \(433.89 МГц\) можно нажать кнопку-переключатель “REC”, а затем кнопку дистанционного включения-выключения периферийных устройств и выполнить запись всплеска радиосигнала и сохранить его в файл.

![&#x41A;&#x43D;&#x43E;&#x43F;&#x43A;&#x430; &#xAB;REC&#xBB;](../.gitbook/assets/image%20%28221%29.png)

Очевидно, что мы не сможем передать такой сигнал ввиду ограничений RTL-SDR. Однако, такой файл можно воспроизвести с помощью Yardstick One или HackRF, если конвертировать его из .wav в raw. К примеру, HackRF для чтения необходим 8-битный подписанный необработанный \(raw\) IQ файл без информации о заголовке.

Помимо GQRX существует множество других приложений SDR GUI: SDRSharp, SIGINTOS и т.д.

### **HackRF** **One** **– Воспроизведение** **RF-сигналов с помощью командной строки**:

Вне сомнений один из наиболее быстрых способов воспроизведения RF-сигнала, когда известна его средняя частота - применить инструмент HackRF под названием “_hackrf\_transfer_“.

Предоставляя необходимые параметры, HackRF может захватывать необходимую передачу \(по нажатию кнопки дистанционного включения-выключения периферийных устройств\), а потом сохранять необработанные данные в файл. Кроме того, HackRF может воспроизводить \(передавать\) необработанный RF-сигнал из сохраненного файла и тем самым вызывать желаемую активность периферийных устройств без использования физического пульта дистанционного управления.

К сожалению, несмотря на быстроту и эффективность данного метода, он применяется «вслепую», поскольку о сигнале известно крайне мало. Такой способ позволяет выполнить поставленную задачу, но не помогает выявить возможные векторы атак или уязвимости.

![](../.gitbook/assets/image%20%28225%29.png)

### **HackRF One** **–** **Воспроизведение** **RF-сигналов с помощью** **граф-схем GNURADIO**:

Хотя это и не самый быстрый способ воспроизведения RF-сигнала, HackRF можно использовать также в сочетании с граф-схемами GNURADIO и посредством этого захватывать, сохранять и воспроизводить радиосигналы. Кривая обучения для понимания GNURADIO может быть довольно экстенсивной, тем не менее, за этой сложностью кроется мощь и универсальность данного инструмента.

На скриншотах ниже показан предыдущий проект воспроизведения сигнала \(воспроизведение сигнала автомобильного брелка\) с помощью граф-схем GNURADIO. Как видно, средняя частота проекта составляет 315 Мгц, но сам процесс тот же.

![](../.gitbook/assets/image%20%28229%29.png)

Показан блок File Sink, с помощью которого сохраняется захваченный сигнал.

![](../.gitbook/assets/image%20%28227%29.png)

Показана граф-схема воспроизведения \(блоки, отмеченные серым, отключены\).

![](../.gitbook/assets/image%20%28230%29.png)

**Yardstick One** **–** **Воспроизведение радиосигналов с помощью** **RfCat**:

Установите RFCat и необходимые зависимости \(libusb, pyusb\)

```text
git clone https://github.com/atlas0fd00m/rfcat.git
```

```text
cd rfcat/
```

```text
sudo python setup.py install
```

```text
cd ../
```

```text
git clone https://github.com/walac/pyusb.git
```

```text
cd pyusb/
```

```text
sudo python setup.py install
```

```text
easy install pip
```

```text
pip install libusb
```

Подключите ваше устройство и выполните следующую команду для его верификации:

```text
 rfcat -r 
```

Проверьте корректность установки и обнаружение донгла:

![https://lh4.googleusercontent.com/OUTGbM5vCWp9RKscjIxHjXAMDot0DeM01Sya03kjdDcUQH\_GNDsFA3pFKwiq39VnyrgKZTG0P7\_XMLIoyBZXkMoagmCG\_cR9gMSV1R\_VuJLiNjeJ4NHDSBWLvRK9IabLt8GSEcRL](file:///C:/Users/terro/AppData/Local/Temp/msohtmlclip1/01/clip_image018.png)

Загрузив сохраненный файл .wav \(полученный с помощью GQRX\) в Audacity мы может быстро идентифицировать переданный пакет данных с амплитудной модуляцией.

![https://lh4.googleusercontent.com/K\_VtBmLbxuXv9tTTjRE1xfT7en-z2SsHhZ56jLnYiRSfBro1d5cgbVv6j86lKTEdFc53R41zQB25Zqrybd10sTvdoGjVCYlm-dbS-TgHD3EYJFJVdvom6s5yqr3XybAcTz5Vpa9C](file:///C:/Users/terro/AppData/Local/Temp/msohtmlclip1/01/clip_image019.png)

Выделив переданный пакет данных и изменив масштаб, можно проследить цикличность повторения более мелких пакетов.

![https://lh6.googleusercontent.com/7EyW62mlSmCda0nCGkdVcJyvowplnMQoC3YAh-Sdz4QRIUkKUFOQ9OUKEDR9WwHju4umEbosqHXwA1K270pH35B1msg0pNhjYHeOz5DsSO4Ndw2fumWMfdYzhpS0MJtecl64wW4d](file:///C:/Users/terro/AppData/Local/Temp/msohtmlclip1/01/clip_image021.jpg)

Еще сильнее приблизив любой из повторяющихся более мелких пакетов, можно выявить модуляцию данных - это OOK \(«включение-выключение»\) PWM \(широтно-импульсной модуляции\). Учитываются как короткие \(Маркер = 1\), так и более широкие импульсы \(Пробел = 0\).

![https://lh5.googleusercontent.com/W2UsAbeykeFsvGHLH50WmQo\_EvZBfFyIwqdaYk51V93P5qZs8JP6waLKvgaQN9Uk11plZr23OLVnmH9okr\_t1T9lG6Tgz8XRGriUQeI10udzfMcS7ZLbbWW4se7th5eyT8P2PIIp](file:///C:/Users/terro/AppData/Local/Temp/msohtmlclip1/01/clip_image023.jpg)

Декодируя импульсы волновой формы, получаем следующий цифровой сигнал:

![https://lh4.googleusercontent.com/gjzAFPJLgeiOAB6zRUntPxqG4DOx\_RcLGU8m01iyiOBmBmGycHjmDpmXs4XdsHBW3jHCDI-JHcyLafrPnmGmKbBTM1X2C5WTlVe84JDEm711e6Uof7eeu0nnepb1IheiAZarq7nE](file:///C:/Users/terro/AppData/Local/Temp/msohtmlclip1/01/clip_image025.jpg)

**Бинарный поток двоичных сигналов**:

Чтобы повторно построить волновую форму для воспроизведения на Yardstick, каждое значение импульсов цифрового сигнал нужно закодировать с использованием следующей битовой таблицы:

Значение сигнала = 0    Закодированные биты = 1110

Значение сигнала = 1    Закодированные биты = 1000

**Закодированный битовый поток**:

11101000 11101000 11101110 10001110 11101110 10001000 10001110 10001110 10001110 11101000 11101000 10001000 10000000

Чтобы воспроизвести данный битовый поток используя Yardstick One, нужно конвертировать эти бинарные данные в шестнадцатеричный формат.

Результатом будут следующие значения: E8 E8 EE 8E EE 88 8E 8E 8E E8 E8 88 80

Перед каждым таким значением в шестнадцатеричном формате нужно поставить ‘\x’.

Строка воспроизведения Yardstick =

\xE8\xE8\xEE\x8E\xEE\x88\x8E\x8E\x8E\xE8\xE8\x88\x80

\(дополненная нулями\) = \xE8\xE8\xEE\x8E\xEE\x88\x8E\x8E\x8E\xE8\xE8\x88\x80\x00\x00\x00\x00\x00\x00

Имея установленное обеспечение Yardstick One, мы теперь можем запустить RfCat:

$ sudo python doorbell.py

\(my script doorbell.py\)

![https://lh4.googleusercontent.com/PMSTR7j9drVzSTfyIKmF3JVeu64NyH2OAuRufVYuylMwZTo2XkE\_vQohnt1c0QzO2\_j8s7DAvcSpHe1795WeHNSkDOHQ8DexgpEvQ6qik8Fg2QRiLQd6RsHE720hleteqoFtwlOC](file:///C:/Users/terro/AppData/Local/Temp/msohtmlclip1/01/clip_image026.png)

**Примечание:** Скорость передачи данных примерно определяется с помощью уравнения: скорость передачи данных = обратная величина \(1/t\), где временем будет считаться интервал в 1 бит.

На скриншоте ниже показан 3-битовый импульс длиной примерно 630 микросекунд, деление на 3 дает время = 210 микросекунд на бит. Используя время самого короткого импульса \(210 мкс\) и взяв его обратную величину, получаем примерную скорость передачи данных = 4800 бит в секунду.

![https://lh4.googleusercontent.com/xQGmftCIaOmk2FpVXjoUh2P0TqN\_ZwlgxX3grPg1bO95VWda29mVhJX3N0vvcgcW7byKrEe0Y-hg6Iho3NOgRAlxrBW\_2UjJV18zIq9-5lUj-ozFtGAsEIMtRQoaJoSO5O87o0eU](file:///C:/Users/terro/AppData/Local/Temp/msohtmlclip1/01/clip_image027.png)

## **Дополнительная информация:**

**Использование RFCat** **в качестве анализатора спектра**:

Установите зависимости анализатора спектра:

```text
sudo pip install PySide2 
```

```text
sudo apt-get install ipython 
```

Выполнить &gt;d.specan\(433920000\) и нажать кнопку дистанционного управления

![https://lh5.googleusercontent.com/uC9CTPadQM2QnL14aUlU\_LHhvzmcpKTzZE-HZAwaULV1-3LxmiKkZHtk8CBXWQFQU\_1wifWeDbLFX9sQFU0EeYjx3to-fMDhEGFLiqMjLfqjJ1xShgzEmDR33craVNBYy3uF-TH\_](file:///C:/Users/terro/AppData/Local/Temp/msohtmlclip1/01/clip_image028.png)

## **В заключение:** 

В попытке автоматизировать весь процесс воспроизведения \(чтобы не вводить шестнадцатеричную строку вручную в диалоговом режиме\), я написал короткий скрипт на python для воспроизведения цифрового сигнала, захваченного GQRX, который ранее анализировался в Audacity.

Как ранее упоминалось, кодирование цифрового сигнала основывалось исключительно на следующей битовой таблице:

Значение сигнала = 0    Закодированные биты = 1110

Значение сигнала = 1    Закодированные биты = 1000

Однако, следует отметить, что несмотря на то, что мне удалось успешно позвонить в звонок, у моего коллеги не получилось запустить тот же самый сценарий на его оборудовании. Ниже приводится разница между цифровыми сигналами, захваченными мной и им:

![https://lh3.googleusercontent.com/RkFS7rsyh9GumCXIK5Xw3LmkrSn456LDs5Rm\_q2mZ\_tC7nMR6fClvNrMsZkz73JhtIKcUMl7suO1L1N-ScM8j4Q4n85YwjrowZed8BNkOMognKk2l6jt8Lbb1NZ5lY3v9fyh754k](file:///C:/Users/terro/AppData/Local/Temp/msohtmlclip1/01/clip_image029.png)

**Мой цифровой** сигнал                                 **** **Цифровой сигнал ББ Кинга**

Обращаю ваше внимание, что мой сигнал начинается с «пробела» \(кодирование 1110\). Однако, сигнал Кинга начинается с «маркера», и взглянув на импульс, делаем вывод, что его следовало бы закодировать как 0001. К сожалению, я не рассчитывал на такое развитие событий, и мой скрипт кодировал его как 1000, что повлекло за собой неудачу Кинга при тестировании.

Чтобы скорректировать скрипт, я внес поправки в скрипт, и теперь он проверяет первый импульс цифрового сигнала и кодирует весь сигнал в соответствии со следующей битовой таблицей:

**Первый импульс** сигнала **= Пробел \(‘0’\)**

Значение сигнала = 0    Закодированные биты = 1110

Значение сигнала = 1    Закодированные биты = 1000

**Первый импульс** сигнала **= Маркер \(‘1’\)**

Значение сигнала = 0    Закодированные биты = 0111

Значение сигнала = 1    Закодированные биты = 0001

**Код на Python**: 

Прошу заметить, что не считаю свой код на python идеальным. Я набросал его довольно быстро исключительно в прикладных целях. Кроме того, поскольку в настоящее время модуль rflib на python3 не поддерживается, я писал код на python 2.7

Вы можете использовать и редактировать код по своему усмотрению, но помните, что несете ответственность за нарушение закона при несанкционированной передаче на законодательно регулируемых частотах.

```text
import sys
```

```text
import time
```

```text
from rflib import *
```

```text
from struct import *
```

```text

```

```text
# User defined parameters 
```

```text
_digital_footprint = "0101001000111010100101111" # My footprint: (short transition from 'space to mark')
```

```text
#_digital_footprint = "11000111010000" # BB King's footprint: (long transition from 'space to mark')
```

```text
_frequency = 433890000
```

```text
_baudrate = 4800
```

```text
_modulation = "MOD_ASK_OOK"  # Modulation Type (not alterable in this version)
```

```text
_mult = 3  # Number of times to transmit RF signal
```

```text
_pad_bytes = 3 # Number of zero bytes for trailing padding
```

```text
_method = "1" # Time period of 'valley' gap when transitioning from space to mark in footprint:
```

```text
 # Short period (1 bit time period) : _method = "0"
```

```text
 # Long period (3 bit time period)  : _method = "1"
```

```text

```

```text
# Configure rf_cat 
```

```text
d = RfCat()
```

```text

```

```text
d.setFreq(_frequency)
```

```text
d.setMdmModulation(MOD_ASK_OOK)
```

```text
d.setMdmDRate(_baudrate)
```

```text

```

```text
print "SIGNAL INFORMATION"
```

```text
print "------------------"
```

```text
print "Frequency:                 ", _frequency
```

```text
print "Baud rate:                 ", _baudrate
```

```text
print "ModulationType:            ", _modulation
```

```text
print "Repeat transmission count: ", _mult
```

```text
print "Digital footprint:         ", _digital_footprint
```

```text

```

```text
mark_space = str(_digital_footprint)
```

```text
xmt_stream = ""
```

```text

```

```text
# Scan each digital footprint bit (pulse) and convert it to the appropriate 4-bit value
```

```text
# Mark = 1 (short pulse) and Space = 0 (long pulse)
```

```text
mark_space = str(_digital_footprint)
```

```text
xmt_stream = ""
```

```text

```

```text
if (_method == "0"): # transitions from space to mark (in footprint) are short (1 bit time period)
```

```text
 for i in mark_space:
```

```text

```

```text
 if (i == "0"): # Space
```

```text
 _pulse = "1110"
```

```text

```

```text
 if (i == "1"): # Mark
```

```text
 _pulse = "1000"
```

```text

```

```text
 xmt_stream = xmt_stream + _pulse
```

```text

```

```text
if (_method == "1"): # transitions from space to mark (in footprint) are long (3 bit time period)
```

```text
 for i in mark_space:
```

```text

```

```text
 if (i == "0"): # Space
```

```text
 _pulse = "0111"
```

```text

```

```text
 if (i == "1"): # Mark
```

```text
 _pulse = "0001"
```

```text

```

```text
 xmt_stream = xmt_stream + _pulse
```

```text

```

```text
# If length of digital footprint is odd, then pad it with "0000" to make it 8 bits
```

```text
if (len(_digital_footprint) % 2 != 0):
```

```text
 xmt_stream = xmt_stream + "0000"
```

```text

```

```text
# Pad zeroes for gap length between transmissions
```

```text
_padding = ""
```

```text
for i in range(0, _pad_bytes):
```

```text
 xmt_stream = xmt_stream + "00000000"
```

```text
 _padding = _padding + "00000000"
```

```text
print "Trailing padding:          ", _padding
```

```text

```

```text
print "RF transmit binary stream: ", xmt_stream
```

```text

```

```text
# Convert binary transmit stream to hex equivalent'
```

```text
hex_xmt_stream = str(('%08X' % int(xmt_stream, 2)))
```

```text
print "RF transmit hex stream:    ", hex_xmt_stream
```

```text

```

```text

```

```text
mod_xmt_stream = ""
```

```text
for i in xrange(0, len(hex_xmt_stream), 2):
```

```text
 ch = "\\x"
```

```text
 mod_xmt_stream = mod_xmt_stream + (ch + (hex_xmt_stream[i:i+2]))
```

```text
print "Modified RF hex stream:    ", mod_xmt_stream
```

```text

```

```text
# -------------------Send Transmission ----------------------------------------#
```

```text
print "Starting transmission ..."
```

```text

```

```text
hex_data = bytearray.fromhex(hex_xmt_stream)
```

```text
d.RFxmit(hex_data, repeat = _mult)
```

```text

```

```text
d.setModeIDLE()
```

```text
print " "
```

```text
print "Transmission Complete"
```

## **Последующее тестирование:**

В попытке понять, почему мне нужно иметь две отдельные битовые таблицы при кодировании для двух разных вариантов оборудования \(всего лишь потому, что сигнал начинался с пробела или маркера в другом случае\), я решил изучить сигнал подробнее. Используя известный рабочий цифровой сигнал, я запустил тестирование, но с кодированием Кинга, предполагая, что он может сработать и необходимость в двух битовых таблицах отпадет. Он не сработал, и я углубился в изучение причин. После внимательного рассмотрения стало ясно, где кроется проблема. Дело было не в том, что мой сигнал начинался с пробела, а его с маркера, различие крылось во времени перехода от пробела к маркеру!

Сравните два цифровых сигнала \(которые уже приводились ранее\):

![https://lh5.googleusercontent.com/5Zf-pW168ANCF7AC91led2b5odyTW-WMaDVySFMaHVUU7gu-nwav4qe4hvsH0NXUHzyq3nbO4jfkvXbl006XZRWRIJ\_3EIzfvlUfDuiGl8Z-aLY3fWYfeD6312w9mGrGWHuyb-Xe](file:///C:/Users/terro/AppData/Local/Temp/msohtmlclip1/01/clip_image030.png)

**Мой цифровой сигнал**                                 **Цифровой сигнал ББ Кинга**

Проанализировав свой сигнал, я установил, что время перехода от пробела к маркеру составляет 1 бит, в то время как такой же интервал у Кинга равнялся 3 битам.

Основываясь на этих знаниях, я снова изменил свой код, чтобы учесть эту разницу при выборе битовой таблицы. Больше не заботясь о том, с чего начинается сигнал, я вместо этого обратил внимание на интервал перехода и использовал эту информацию для выбора подходящей битовой таблицы. _**Успех!!!**_

## **Заключение и выводы:**

Работая с Yardstick впервые, могу сказать, что это был очень полезный опыт. Иметь возможность посотрудничать с ББ Кингом \(BB King\), одновременно разрабатывая собственное уникальное оборудование, и подавно неоценимая возможность. Это помогло мне сосредоточиться, двигаться в правильном направлении и давало высокую мотивацию, а также позволило столкнуться с различными обстоятельствами, которые могут возникать при тестировании.

Кроме того, по моему скромному мнению Yardstick One [Майкла](https://twitter.com/michaelossmann) Османа \(Great Scott Gadgets\) – великолепный инструмент: довольно недорогой, простой в обращении, но разнообразный в применении и подходящий для автоматизации процесса, если в том возникнет необходимость. При захвате и воспроизведении сигналов устройств с подвижным или фиксированным кодом \(автомобильные брелоки, системы дистанционного открытия гаражных ворот, беспроводные дверные звонки, устройства безопасности или практически любых беспроводных радиосигналов в этом диапазоне частот\) частотные ограничения \(до 1 ГГц\) Yardstick One не играют большой роли.

