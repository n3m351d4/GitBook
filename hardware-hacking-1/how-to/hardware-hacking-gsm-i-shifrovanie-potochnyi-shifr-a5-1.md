---
description: Перевод @n3m351da @in51d3 2020
---

# -Hardware Hacking: GSM и шифрование: поточный шифр A5/1

[Оригинальный текст](https://www.blackhillsinfosec.com/gsm-traffic-and-encryption-a5-1-stream-cipher/)

**Внимание!** При передаче любых данных обязательно используйте клетку Фарадея или экранированную сумку, чтобы случайно не нарушить каких-либо законов несанкционированным вмешательством в эфир на определенных частотах. Кроме того, помните, что перехват и дешифрование/декодирование чужих данных является незаконным, поэтому будьте осторожны при исследовании вашего трафика.

## **Немного терминологии**

_Термины, связанные с мобильным телефоном:_ 

* Мобильная станция **МС** \(телефон\) 
* Модуль идентификации абонента **SIM** \(sim-карта\)
* **IMSI** международный идентификатор мобильного абонента \(идентификатор абонента\) 
* **TMSI** временный IMSI \(помогает с конфиденциальностью, запутывая IMSI\) 
* **Ki** 128-битный уникальный ключ абонента \(в паре с IMSI\)

_Термины, связанные с базовой станцией:_

* Базовая приемопередающая станция **BTS** \(содержит оборудование для передачи и приема радиосигналов, антенны и оборудование для шифрования и дешифрования связи с контроллером базовой станции \(BSC\)\) 
* Контроллер базовой станции **BSC** \(распределяет радиоканалы, принимает измерения с мобильных телефонов и управляет передачей обслуживания от BTS к BTS\)

_Термины, связанные с базовой сетью:_ 

* Подсистема сети **NSS**, отвечающая за маршрутизацию вызовов через BSC и BTC
* Центр коммутации мобильной связи **MSC** \(коммутация сети\) 
* Регистр местоположения абонентов **VLR** \(база данных мобильных станций, относительно местности\) 
* Реестр домашнего местоположения **HLR** \(база данных с постоянными абонентами\) 
* Центр аутентификации **AuC**

_Переменные:_

* **RAND** 128-битное случайное число \(отправляется МС от AuC для облегчения проверки подлинности МС\)
* Запрос **SRES** отправляется МС \(генерируется с помощью алгоритма RAND + Ki + A3\) 
* Уникальный **KC** \(генерируется с помощью алгоритма RAND + Ki + A8\)

**Примечание**: данные шифруются с помощью алгоритма KC + A5 / 1

## **Preliminary Information**:

Before proceeding to the encryption process, it may be helpful to know that although there are three different algorithms \(A3, A8, and A5/1\), we can simplify the overall process significantly by stating upfront, the following:

* The A3 \(**authentication**\) **algorithm** is ‘only’ used to facilitate authenticating that the mobile station \(MS\) has permission to be on the network.
* Once authenticated, the A8 \(**ciphering key generating**\) **algorithm** is ‘only’ used to create a unique key \(KC\), that ultimately will be used \(by the MS and the Network\) for encrypting/decrypting data using the **A5/1 stream cipher algorithm** on-the-fly.
* The A3 algorithm, A8 algorithm, IMSI and Ki all exist on the MS \(phone\) SIM card and the A5/1 stream cipher algorithm exists in the MS \(phone\) hardware.
* Additionally, the Home Network \(HLR, VLR, MSC, AuC\), has access to the same information via its databases.

**Typical Process**:**Diagram 01**

![](https://lh4.googleusercontent.com/5r2_aCvjL8dFFSL_tdoJvBPR_HBV7ssS-lBkjcfWSaJhCdmF4MGnHXyLVNhTBy_Wq8Q6dP6oTqvPdjpRUeJCjl4fjIc4A5Sqd8H71o9iBCVz9mjlnSnJdV4iOYA9lZHGCwAiQplm)

**Process:** \(follow diagram-01\)

1. Mobile station \(MS\) requests access to the network,  MS sends its IMSI to the Network Subsystem \(NSS\) via the BSC / BTS.
2. The IMSI sent by the MS is forwarded to the MSC on the network, and the MSC passes that IMSI on to the HLR and requests authentication.
3. The HLR checks its database to make sure the IMSI belongs to the network.
   * If valid, The HLR forwards the authentication request and IMSI to the Authentication Center \(AuC\).
   * The AuC will access its database to search for the Ki that is paired with the given IMSI.
   * The Auc will generate a 128-bit random number \(RAND\).
   * The RAND and Ki will be passed into the A3 \(authentication\) algorithm, creating a 32-bit SRES \(signed response\) for the challenge-response method.
   * The RAND is transmitted \(via the BSC / BTS\) to the mobile station \(MS\).
4. The RAND received by the MS, together with the SIM card-Ki are passed into the SIM card-A3 \(authentication\) algorithm, generating the phones SRES response.
5. The phones SRES response is transmitted \(via the BSC / BTS\) back to the AuC on the network.
6. The AuC compares the sent SRES with the received SRES for a match. If they match, then the authentication is successful. The subscriber \(MS\) joins the network.
7. The RAND, together with the SIM card-Ki are passed into the SIM card-A8 \(ciphering key\) algorithm, to produce a ciphering key \(KC\).
   * The KC generated is used with the A5 \(stream ciphering\) algorithm to encipher or decipher the data.
   * The A5 algorithm is stored in the phone’s hardware and is responsible for encrypting and decrypting data on the fly.

**Levels of Security:**

_Algorithms_:

![](https://lh5.googleusercontent.com/2FWiU5_5C8mqcQSFv8HKHSFHCHnIfvO7R0g9MA_DsZ_q4T4mJkSmWstcpkd_4_zSB24V-Zuc9lMbnn3OAst6tnuzDk-NJDhBmk7JbjDdjN46Qts6d3fRTz10fdupMZOeb5_3HDLC)

**IMSI Obfuscation**:

The first time a subscriber joins the network, the Authentication Center \(AuC\) assigns a TMSI \(temporary IMSI\) which will be used in place of the subscribers IMSI going forward. I say “temporary”, but in fact, the TMSI is stored \(along with the IMSI\) in the VLR \(visitor location register\).

When the phone is switched off, the phone saves the TMSI on its SIM card, ensuring it is available when switched on again.

Every new update \(roaming, handoffs, etc\) results in a new TMSI being created. The TMSI is used in place of the IMSI to protect the subscriber’s identity.

**Sample Packet Screenshots:**

_Location Updating Request \(TMSI not established yet\)_

![](https://lh5.googleusercontent.com/iyrdJ4LHWFUngtQXPYr_CQaA71NMgqbUjKxdzDXq5mEBmiPcEFq24snmvqbt4m_kMqeYMwSXyvdjGRF8TXatAhyOCC2O8F8DFVJec0IlldfvsmGXnUhXFPp2B1k2nU7zaAh1Oujy)

_Запрос на аутентификацию_

![](https://lh4.googleusercontent.com/c0W3atWxW7TU0i_MXO5twIBp9WSvX8opriwNtCmb2i01_R80pFFYb2duiW2iZ2F1wIc4KZW9K_-lluV9feBGNyUuqQKdzVjYuTYmp05f6UE0Fj1sulGSe3Q3pyvfTZOJajkfEIrT)

_TMSI / A5/1 Algorithm Supported_

![](https://lh5.googleusercontent.com/amAB08uW_TOcCm0cQIRnhmSyw4Mp6uhH74tO5kD90v73h4s7tvRQLV7cwqWKy84tKwaaPDj8koeQayl5nPk0RMXwt93SolKVNVgb6adKC0KmbD73DXvj9wb3UoEAwFmwNWhxRA8D)

## **Summary**

This write-up documents some of my follow-up research with regard to analyzing the GSM traffic packets I captured using Software Defined Radio. My attempt was to better understand the GSM mobile network protocols and procedures, with an emphasis on the authentication and ciphering algorithms being deployed.

In my opinion, there is a huge demand for exploring this relatively untouched attack vector, especially as we move towards adopting 5G technologies. The A5/1 stream cipher algorithm, is still in use today on many GSM networks, has a prior history of being exploitable, and there are quite a few networks that do not even implement ciphering in their protocols \(SMS data completely exposed\).

These vulnerabilities can potentially expose our private SMS messages, personal data, and even our GPS locations to the public if left unguarded. More research in this area is required to ensure our privacy remains secure. From an InfoSec perspective, the areas of concern might be MiTM attacks, network breaches, etc. The playing field is wide open.

## **Ссылки**

[https://www.sans.org/reading-room/whitepapers/telephone/gsm-standard-an-overview-security-317](https://www.sans.org/reading-room/whitepapers/telephone/gsm-standard-an-overview-security-317)

[https://en.wikipedia.org/wiki/GSM](https://en.wikipedia.org/wiki/GSM)

[https://payatu.com/blog\_28](https://payatu.com/blog_28) 

[https://payatu.com/blogger\_by%20Rashid%20Feroze](https://payatu.com/blogger_by%20Rashid%20Feroze)

