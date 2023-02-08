---
description: AI – угроза для кибербезопасности?
---

# Б/Н от 01.2023

Последнее время новостные ленты пестрят сообщениями о решении ChatGPT  от OpenAI.

> [ChatGPT](https://ru.wikipedia.org/wiki/GPT-3) — это [чат-бот](https://ru.wikipedia.org/wiki/%D0%A7%D0%B0%D1%82-%D0%B1%D0%BE%D1%82) с [искусственным интеллектом](https://ru.wikipedia.org/wiki/%D0%98%D1%81%D0%BA%D1%83%D1%81%D1%81%D1%82%D0%B2%D0%B5%D0%BD%D0%BD%D1%8B%D0%B9\_%D0%B8%D0%BD%D1%82%D0%B5%D0%BB%D0%BB%D0%B5%D0%BA%D1%82), разработанный компанией [OpenAI](https://ru.wikipedia.org/wiki/OpenAI) и способный работать в диалоговом режиме, поддерживающий запросы на естественных языках. Чат-бот основывается на другой языковой модели от OpenAI GPT-3.5 — улучшенной версии модели GPT-3. — Wikipedia

ChatGPT стала популярной за счёт дружелюбного интерфейса для пользователей и позволила множеству людей, в том числе, без технических навыков получить доступ к невероятно продвинутой технологии. Каждый день для этой нейросети люди находят новые и интересные применения, в том числе в целях наступательной безопасности. Давайте попробуем разобраться, как можно применять ChatGPT и ответим на такие животрепещущие вопросы, как: может ли она написать вредоносный код и какие методы применения ChatGPT могут быть эффективно использованы в наступательной безопасности?

<figure><img src="https://lh4.googleusercontent.com/-9TgCZ6NwC_9qr5hFhJ2R8cMxxmljlAWzpVdVWPRCDTjr2p4y4QUR8mtkRVE0fTomavP1sjwAOswRzWFTAw2owycPFsKm4mLt4NpY72VvTQt2GLBAgaqvOKQVuILsznsQwUJqS2CQiPxXlZIKppKBQ" alt=""><figcaption></figcaption></figure>

## ChatGPT и вредоносный код

Как уже было сказано ранее появилось огромное количество статей на различных ресурсах по информационной безопасности с опасениями, и, местами, даже с доказательствами того, что ChatGPT может быть использована для написания вредоносного кода.

> [Concerns over threat actors abusing ChatGPT](https://www.darkreading.com/omdia/chatgpt-artificial-intelligence-an-upcoming-cybersecurity-threat-) have been rife ever since OpenAI released the AI tool in November, with many security researchers perceive the chatbot as significantly lowering the bar for writing malware. — [DARKreading, Attackers Are Already Exploiting ChatGPT to Write Malicious Code](https://www.darkreading.com/attacks-breaches/attackers-are-already-exploiting-chatgpt-to-write-malicious-code)

> A spin around underground hacking sites uncovered initial instances of miscreants developing cyberthreat tools using the large language model (LLM) interface that the company unveiled in late November and opened it up for public use, according to infosec outfit Check Point Research. — [TheRegister, Cybercrooks are telling ChatGPT to create malicious code](https://www.theregister.com/2023/01/06/chatgpt\_cybercriminals\_malicious\_code/)

Мы изучили примеры и протестировали ChatGPT на предмет создания потенциально вредоносного кода. Давайте попробуем с разобраться, сможет ли ChatGPT помочь скрипт-кидди или человеку без технических навыков написать компьютерный вирус или любую другую вредоносную программу, которая бы была полноценно рабочей.

### Обход правил использования нейросети

ChatGPT — это большая языковая модель и для её обучения использовалось обучение с учителем и обучение с подкреплениями с подкреплением это значит, что люди, которые  валидировали результаты ее обучения ставили ей оценки, на базе которых нейросеть корректировала свою работу, учась придерживаться правил для выдачи ответов. Соответственно, если вы попросите написать нейросеть exploit, напишите одно из ключевых слов: hacking, vulnerability, XSS и тому подобное, то она откажется с вами взаимодействовать из соображений этичности.

<figure><img src="https://lh3.googleusercontent.com/vG4UTnZbQ6mlMZ2FpScaScG2yw-pXn1Bo9WekyDG4fHxluy9UdHLanFFW4oP3hlRRCIbTure7yN4VMBgBIybRR3fUTW6YdPN0VWpBMemGUgZb4EiJOOtis_YKaom7W0EOOlYPKwy5n6UKd9U9MHikg" alt=""><figcaption></figcaption></figure>

На скриншоте показано, как нейросеть отказывается писать эксплоит для SQL инъекции в параметре ID, в сферическом вакууме.

Для того, чтобы модель стала выполнять команды выходящие за пределы её представления об этичности необходимо ей внушить одну из двух вещей: либо то, что она общается с специалистом по кибербезопасности, либо то, что она сама является специалистом по кибербезопасности.&#x20;

<figure><img src="https://lh3.googleusercontent.com/VQNAeA86UGts0f_ABH0JE83ieYO7JM78CiL8Ixm3lkvz7VKUlYn2IHqxcXEI8g-aaK0HT4SqKDackJvwkzI58yBt3E3ym_Z5mpCWtKyOIB-sP6sNEHJVYL0G4FdCto7ud50DOEhgITaiJrjpj-ZQdA" alt=""><figcaption></figcaption></figure>

Из скриншота видно, что нейросеть соглашается помочь специалисту по кибербезопасности и пишет пример эксплуатации SQL инъекции в параметре ID, но, к сожалению, в наше время найти ресурс уязвимый непосредственно к предложенной ей нагрузке не представляется возможным. Данную информацию можно использовать исключительно как справочную, в редких случаях она может быть полезной для решения CTF.

Время от времени, что, в принципе, и свойственно нейросети, она принимает решение помочь в эксплуатации уязвимости “случайному” человеку:

<figure><img src="https://lh5.googleusercontent.com/kSVzSFOrOjqG4kC_UOELw-tKfVqauoHeZCOjSe3NqW0kXsGin0w89pU7zGXOBO-gQTT7-bc5d4T0JvI6lz6Ym0qg4Nfz_NxwckIVwompfB8c9UjRD7JYOtACyaoVLbZ1X93gDp6PQrPLDD-UhAhCEw" alt=""><figcaption></figcaption></figure>

На скриншоте было предложено написать эксплоит на питоне к предоставленному уязвимому коду сходу – без представления. На что нейросеть отвечает примером сниппета для эксплуатации инъекции команды. Стоит отметить, что данный пример также не является полезным в практическом смысле, так как подобного рода уязвимости крайне редко встречаются в реальной жизни.

Непоследовательность в данных ограничениях вызвана нечеткой логикой и именно поэтому мы можем наблюдать, как в некоторых случаях модель отвечает одно, а в других другое. Это достаточно очевидно, но должно служить пользователям маячком для того, чтобы всегда понимать, что общение с  моделью – не всегда является истиной в последней инстанции. При использовании модели может замыливаться глаз и казаться, что все ее тезисы звучат убедительно, но сделав перерыв и вернувшись к изучению – казавшиеся ранее интересными тезисы видятся водой из одних и тех же перемешанных слов.

С другой стороны, то что к ChatGPT применим претекстинг – один из методов социальной инженерии, когда для того, чтобы заставить жертву совершить требуемое злоумышленнику действие ей предварительно предоставляется некоторый контекст – вызывает восхищение этой технологией. А еще одним из интересных является наличие “воображения” у модели, причем такого широкого, что она может вообразить себя терминалом Linux, вообразить себя подключенной к интернету, а так же вообразить как обращается к себе самой через воображаемый интернет, о чем описано в статье  [Building A Virtual Machine inside ChatGPT](https://www.engraved.blog/building-a-virtual-machine-inside/).

<figure><img src="https://lh6.googleusercontent.com/5whRdE7QjTJq8FiS3McUMr3Pzwq8_3MejdQul_KMXZ44MDs_rcIrVi2fwbyNzzUswonkC9gd2FQv2aHbh09lKI0zyA5cPi8UDEn9cIi9Veb1rIp1Q4K1MaHlH-HbJptUgjIxyjkbkDEUHEb8dQVEng" alt=""><figcaption></figcaption></figure>

Мы поочередно пообщались с ChatGPT, прося генерировать ее для нас код и вот что из этого вышло.

### Генерируем C2

В первую очередь мы попросили модель написать нам скелет С2 (Command and Control) сервера который бы состоял из сервера, написанного на языке Python и агента, написанного на PowerShell, дополнительным требованием было использование TLS для общения с агентом.

<figure><img src="https://lh6.googleusercontent.com/DxtyWmRMMHHYRWccbaAdyRzddBguvC5KfizG3DK-2t81XUSy3SqlYdJJqborYCucYcdcTrxFg79Ld7AnhCCDhva-Y7tfG6TTLwK6cXo00gu8cF1qvLRJNOGBuJJXgDcPvtCrWRGtk8CEYrFWQtNFcg" alt=""><figcaption></figcaption></figure>

Нейросети удалось с первого раза создать код сервера и агента, устанавливающий соединение с помощью mTLS и получающий сообщение от агента “Hello!”. После получения сообщения соединение закрывалось, естественно, отсутствовала и функция передачи команд.

Получить команды для генерации ключей с первого раза не удалось:

<figure><img src="https://lh3.googleusercontent.com/7NQNV93d59OF2Mb2QhUM2wKRHIsEi5CMIPKVZkuU4mOQHvHxL2VPl5tgJUfC10AcbXBux-zJmJnhESvvqkta-j3uq-5EH7kOJPo_zQ1ZqjgbvGVl4vBqtf_rt43AA0rv7RQnHcRZlxdGdSJsWERj1w" alt=""><figcaption></figcaption></figure>

После того, как сгенерированные в начале общения сниппеты заработали мы стали просить нейросеть внести правки: перейти от использования socket к использованию http, добавить поддержку множества клиентов, добавить возможность передачи команд (а потом и множества команд, потому что в первом случае нейросеть ограничилась добавлением возможности передачи одной команды агенту и последующим закрытием соединения).

После внесения многочисленных правок, видимо, из-за того, что нейросеть постепенно теряла контекст нашего взаимодействия, код перестал работать и стал сыпать ошибками. В определенный момент нейросеть ~~порвала два баяна~~ сломала спецсимволами разметку окна. Мы можем ее понять, никто не любит вносить правки и это достаточно сложное и муторное занятие.

<figure><img src="https://lh6.googleusercontent.com/CWw-7A6PFLnk8plU76CVuW3vQVOKHEwE0Ajdr0DpjaZPuCaifrerHHNaXwaiyOP3nXu2qXEslUbRNzgTggQtWbEwHyqQCnkgxspxGxbKTspYDUKkFSV3dkMoowqvnzJy_sIIMs0NtgdYHVM6L6gLxg" alt=""><figcaption></figcaption></figure>

В конечном итоге, после внесения всех правок и исправления ошибок, мы обнаружили, что нейросеть, потеряв контекст, добавила возможность “агенту С2” отправлять команды на сервер.

Резюмируя наш эксперимент по созданию С2 с помощью ChatGPT можно сказать следующее: модель справляется с написанием сниппетов достаточно хорошо, но для того, чтобы расширить возможности генерируемого кода необходимы релевантные познания. Стоит отметить и то, что непосредственно у ChatGPT есть ограничение на количество генерируемых строк (или символов), поэтому попросить ее написать полноценный С2 не представляется возможным в принципе.

### Syscalls и AtomBombing

Следующий наш запрос на генерацию кода имел вид: “Сгенерируй код на х64 ассемблере, который считывает 1024 байт из TCP-соединения, установленного с узлом 10.10.10.10, затем используя алгоритм RC4 и ключ шифрования “AZALALKA” расшифровывает последовательность байт и после этого интерпретирует последовательность как код и выполняет ее. Реализовать все в виде системных вызовов (syscalls)”. Задумка заключалась в проверке возможности генерации части вредоносного кода, известного как стейджер (stager - код, целью исполнения которого является – загрузка и передача управления следующему этапу вредоносной нагрузки, которая дальше установит соединение с Command\&Control сервером).

Формируем чеклист необходимых нам свойств и кода:

* ОС Windows 10 21H2;
* использование системных вызовов (где это применимо);
* установка TCP соединения;
* использование RC4 c ключом “AZALALKA”.

Смотрим результат работы нейросети:

\


<figure><img src="https://lh3.googleusercontent.com/Q9e613mSZ3vIf_JTJW8ulJA-z_D0ihX58nhioe2TUQqiSGWv9-eU7zb0GldxaUIo1grGrdpaZRlQ4QKCczExJzd6QcCDQcMP6Qitd15a-1Rl0bRhPe-t-T5wHEyNMcepYLOHWfVkeMk45f9n8KVZCQ" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh3.googleusercontent.com/U0WCIls-FpyngGma5B3icGEqzOaGpXue2kxcIhpTr0AFaMx1Ap8rlv94X-fQXJvniPlD52PUhzITa8ZGywzyEx-xbnI9T0Fs5gwklGvm66ln9Lq7I_yS1quMANx68bLOfMQ8pZc4jIO2qzkIzzKYTg" alt=""><figcaption></figcaption></figure>

Проверяем чек лист:

Первое, что бросается в глаза - вызов функции WSAStartup. Он весьма странный– сделан в х86 конвенции вызовов (здесь виноваты мы – не уточнили в этом запросе). В качестве аргументов передаются 0x0202 и 0x0101. Внимательный читатель может поискать на MSDN-е информацию о том, как вызывается данная функция и понять в чем тут ошибка. Для, тех, кто не разбирается в данном вопросе, поясняем – указанная функция действительно принимает числовой аргумент, но он должен быть один - это указание версии, которая будет использоваться для Windows Sockets. Вторым же аргументом должен быть указатель на структуру WSAData, которая заполняется после успешного вызова рассматриваемой функции. В нашем же случае программа упадет на первых же инструкциях, так как произойдет разыменование указателя по адресу 0x0202 (что может и не получится), а затем произойдет попытка записи по данному адресу (что точно не получится).

Следующий блок, который у нейросети называется socket - создание сокета, также не вызывает доверия. Рассмотрим его подробнее: из плюсов - он реализован [в конвенции вызовов x64](https://learn.microsoft.com/en-us/cpp/build/x64-calling-convention?view=msvc-170). Запоминаем следование аргументов в регистрах:

```
func1(int a, int b, int c, int d, int e, int f);
// a in RCX, b in RDX, c in R8, d in R9, f then e pushed on stack
```

Смотрим на функцию WSASocketA на MSDN и прикидываем как выглядит вызов на ассемблере (все параметры обязательны):

```
SOCKET WSAAPI WSASocketA(
  [in] int                 af,
  [in] int                 type,
  [in] int                 protocol,
  [in] LPWSAPROTOCOL_INFOA lpProtocolInfo,
  [in] GROUP               g,
  [in] DWORD               dwFlags
);
```

Представим вызов функции на псевдокоде, так как, очевидно, что присваивания и вычисления можно сделать большим количеством способов, а очередность подготовки регистров совершенно не важна.

```
rcx = 2 //af - AF_INET (IPv4)
rdx = 1 // type - SOCK_STREAM (TCP)
r8 = 6 // protocol - IPPROTO_TCP 
r9 = 0 // lpProtocolInfo может быть NULL, но указывать обязательно
push 1 // (dwFlags = 1) WSA_FLAG_OVERLAPPED - для большинства сокетов
push 0 // работа с группами сокетов, для обычного применения не используются, поэтому 0 
call WSASocketA
```

Теперь, когда мы разобрались, посмотрим на то, что нам предлагает нейросеть - это совсем не то, что требовалось. Но на что это похоже? Внимательный читатель, имеющий опыт кастомизации или написания шеллкодов для тестирования на проникновение, может заметить что в регистр al передается какое-то значение 0x29 и затем выполняется syscall. Это очень похоже на исполнение системных вызовов для ОС Linux. Небольшой поиск подтверждает наши догадки 0x29 это идентификатор системного вызова socket для ОС Linux архитектуры х86\_64. Добавленная строка с вызовом функции WSASocketA превращает данный блок кода в наглядный пример потери контекста во время генерации текста у нейронной модели. Делаем вывод - код не соответствует требованиям которые мы предъявили.

Следующий блок, который относится к чтению данных из сокета повторяет ошибки предыдущего. Выводы абсолютно такие же. Нельзя просто добавить вызов функции WinAPI к коду для ОС Linux и получить желаемое. Все работает немного сложнее.

Нейронная сеть опустила генерацию RC4, но в отдельном примере показала, что не может его корректно сгенерировать. Код получается осмысленный, но отношения к RC4 имеет посредственное, например во время стадии инициализации, вместо сравнения индекса с 256, сгенерированный код сравнивает индекс с 8 (возможно, сеть пыталась сгенерировать число 2^8 == 256):

<figure><img src="https://lh6.googleusercontent.com/5dFN_BTfe635iMa1eTXeMgZWwE_A-x1WIQYqqLL3VN-hes-5RdW8mqdYeUA8x7JAEkpiuRYmt1CVPKBf_4SNBWVZYs3hkcS5uNo9kI09KDT5CWaX-4YBWx5X4YEXTtk7xIdQuuaduyuheMdjDRHW6w" alt=""><figcaption></figcaption></figure>

Как ни упрашивали, функцию расшифровки получить не удалось:

<figure><img src="https://lh4.googleusercontent.com/BnY-mG_0Ek9BaGpOR-_DM-bbKN5yOG1gRcWQD-bRWbpQtgOtImJxLSKXjoawOM3WErjFSkbZO8zrw2rmZtZQMbnonp4ARXvatPq71CHaPZ5N_iK_Q4czjuvJ4qik2LZl60N4zZDHc9veMYxZ-Y1m7g" alt=""><figcaption></figcaption></figure>

Дальше, в блоках VirtualAlloc и CreateThread сеть ошибается с номерами системных вызовов уже и в Linux, то есть код тут генерируется верный, но в деталях и применении - ошибочный и бессмысленный. Скорее можно рассуждать о выборе нейронной сети подхода к написанию кода, нежели к конкретике.&#x20;

Нейросеть сгенерировала код для копирования данных (блок memcpy) под ОС Linux, что снова не соответствует исходному ТЗ.

Далее, была предпринята попытка запросить у нейросети некоторые продвинутые процедуры для внедрения кода в процесс. Оценка давалась уже не за точность (предыдущий пример показал что она нулевая), а за общий подход к проектированию сложного и малоизученного кода.&#x20;

<figure><img src="https://lh3.googleusercontent.com/SkdcVI3Cd12z6CyaiNvxwpRGVEw92ode0Vob0kvWVLgYesAelMpWHT_mdE4K80QWol2d7OrY-OcwFRme_4SpV6DHf_yNp7PnWTbVOZUf9SMGeEVo-_LnpTw2Cly-zXVkiKmLySAdVEcWEG7yOJ-0Kg" alt=""><figcaption></figcaption></figure>

Не погружаясь в детали резюмируем - подход неверный. Atom Bombing - техника “переноса” байтов из одного потока/процесса в другой (поток или процесс, если есть достаточно прав) через глобальную таблицу атомов ОС Windows.&#x20;

Попытка рассмотреть подход к запуску кода через ALPC также провалилась.

В общем и целом, так как подобного кода в открытом доступе достаточно мало, нейросеть не справилась с заданием.

Резюмируя эксперимент с low-level API и “продвинутыми” возможностями в ОС Windows можно сказать следующее: модель не справилась. Контекст не учтен, код неверен, общий подход также непонятен. Ставить именования из списка требований в коде или комментариях – хитрый способ, который вводит в заблуждение о “всемогуществе”. Но внимательный пользователь или специалист по ИБ сразу заметят данную “халтуру”.

### Разбираем исследование CheckPoint

Давайте ознакомимся с иссследованием [OPWNAI : Киберпреступники начинают использовать ChatGPT - Check Point Research](https://research.checkpoint.com/2023/opwnai-cybercriminals-starting-to-use-chatgpt/) и разберем приведенные примеры вредоносного кода.

#### Пример 1

<figure><img src="https://lh6.googleusercontent.com/aHONhZL_VepNSvxvOGBcxOrf6nKOg1gLhsikFwzBOyHn31j-k-plO9SrCL_O_rQgGekxs6-fRMt2iy4pCqDryTIk7KraQxUdCea4teWd9QXBFraUo-Rzyh7shHplI82bcv6rFhTxYyHMcvn1JeFEiA" alt=""><figcaption></figcaption></figure>

В данном примере мы видим, что пользователь форума сгенерировал скрипт, который ищет файлы с определенным расширением, а затем архивирует их и отправляет на FTP-сервер.

Не ясно, почему исследователи называет данный скрипт infostealer, так как в нем используются типичные функции для и механизмы для софта, делающего бэкапы. Данный код может быть использован и в легитимных целях и его вредоносность будет зависеть исключительно от его применения. Для того, чтобы использовать его злонамеренных целях, нужны релевантные знания и навыки такие, как получение доступа к серверу, повышение привилегий. Соответственно, данный скрипт можно использовать как для кражи файлов, так и для создания бэкапа и сказать, что непосредственно данный код является вредоносным было бы неправильно.

Также стоит отметить захардкоженные пароль и адрес FTP-сервера – это не является лучшей практикой для создания стилеров. Если вы оставите в таком виде (в открытом, в котором обычно пишет ChatGPT) IP-адрес своего сервера, то вы сильно упростите расследование инцидента, связанного с утечкой файлов, поэтому подобная имплементация выглядит крайне наивной.

#### Пример 2

<figure><img src="https://lh3.googleusercontent.com/QanMpd4i2C7ZYXDW5I0FXNZTQ5MD4dWlblIe6fJAZ5ndueMIn_ioaK5x3l_2cyoITqE7ovDYGBIvrc2POE0WxLKTlO0oo0i75IJItyxIYJEUj-3Wpxdkdv2IAmkGm2XNLOM-r134S38QhF0FWK6THA" alt=""><figcaption></figcaption></figure>

Давайте, рассмотрим второй пример – пользователь создает Java код, который устанавливает Putty и запускает с помощью PowerShell.

Сказать что этот код вредоносный – нельзя, потому что установка программ с использованием Java и обращение к Powershell является легитимными функциями системы. До тех пор пока автор не загрузит данный код на чужой ПК, чтобы нелегитимно получить к нему доступ через непосредственно Putty (что является крайне грубым методом для нелегитимного получения удаленного доступа), данный код невозможно назвать вредоносным. Подобный код может использовать, в том числе, системный администратор, который, например, тренируется писать на Java для того, чтобы устанавливать софт на ПК пользователей. Соответственно, снова вредоносность данного кода будет зависеть от его применения.

С учётом того, что автор “претендует” на скрытность и использует команду Powershell с флагом -Hidden – данный код с высокой вероятностью вызовет реакцию Windows Defender, который, с высокой вероятность, убьет данный процесс.

#### Пример 3

<figure><img src="https://lh3.googleusercontent.com/TAa06icP6sLjAkvFLuQFb6vFCRhH0u3PR7PPau7TvkSHmbqvxQgxnhNzp-prIVbLcOw3naLQS1NPk1E4VGG9vum0-bmY1id0s7MDqKWF_IwHdib4PVuh_bfJeCvHA6fHHwXfkfriMTjiVu8AuJK4yg" alt=""><figcaption></figcaption></figure>

В третьем примере исследователи не потрудились привести нам код, который они классифицировали вредоносным. По словам исследователей данной код был создан с помощью OpenAI, но на самом деле, если мы внимательно вчитаемся в слова пользователя форума, то мы увидим фразу, в которой он говорит, что OpenAI, всего лишь помог ему закончить его скрипт. То есть этот код не был полностью сгенерирован и пользователь по большей части (что исходит из нашего опыта, описанного ранее) писал скрипта самостоятельно.&#x20;

По словам автора, он делал это впервые и использовал питон, но из-за отсутствия самого кода мы не можем оценить всю “мощь” и весь потенциал его произведения, как и его “вредоносность”, соответственно.

### Извините, вредоносного кода не будет

Тем временем, на Stackoverflow запретили публиковать ответы на вопросы, сгенерированные с помощью ChatGPT. “...поскольку средний показатель получения правильных ответов от ChatGPT слишком низок, размещение ответов, сгенерированных при помощи ChatGPT, существенно вредит сайту и пользователям, ищущим правильные ответы.”

<figure><img src="https://lh3.googleusercontent.com/aZgPRr2DXwk6XDqzVL9JqmBHlPjo8oq0w--V6-Ox3kmNu7LteB4t_rtiXszskB7v2Sse5yy74TnUbefDlq4AE3KW-SwtcdwmIDL-ZarMvyXkVromd9qmUiC344Rkjqlh881R1Lj6dA4c6m9FkXzj7A" alt=""><figcaption></figcaption></figure>

Давайте подведем итоги и разберемся можно ли с помощью ChatGPT генерировать вредоносный код?

* не всегда сгенерированный код полностью соответствует описанию;
* не всегда код работает;
* когда нейросеть не знает, что писать, она начинает “выдумывать”;
* использование написанных нейросетью сниппетов требует понимания и способностей для их отладки и использование;
* генерируется ограниченное количество кода – не получится создать что-либо масштабное и сложное.

Т.е., резюмируя - нейросеть станет инструментом для написания вредоносного кода лишь в руках человека, который понимает, как писать подобный код. Но в таком случае, проще писать код самому, целесообразность использования нейросети - отпадает (она делает много ошибок).

С другой же стороны, она представляет собой неплохой справочник, который будет несомненно полезным для изучения специалисту любой профессии для расширения своего багажа теоретических знаний, но не практических навыков.

## Обучение хакеров

Еще одним интересным явлением стало выдумывание нейросетью названий утилит. Дело в том, что как мы раньше говорили, в некоторых статьях и исследованиях, связанных с ChatGPT и информационной безопасностью говорится о том, что эта модель помогает снизить порог вхождения в наступательную безопасностью, чем потенциально может увеличить количество хакеров с негативными намерениями.

Почему это отчасти неверно. Дело в том, что блоггеры и исследователи, которые тестировали эту модель не проверяли её ответы на достоверность. Не смотря на то, что модель говорит очень складно, она достаточно часто врет – да, именно врет и выдумывает. Я заметила это во время генерации заметок для своего блога, прося генерировать модель однотипные пошаговые методологии тестирования различных сервисов и приложений.&#x20;

Одна из подобных сгенерированных методологий приведена ниже (мы не будем говорить о применимости утилит, сосредоточимся на их существовании):

“…One possible methodology for conducting a penetration test on Kubernetes using tools is as follows:

1. Identify the target environment: The first step is to identify the target Kubernetes environment. This can involve using tools such as kubectl and kubescan to gather information about the clusters, nodes, and pods in the target environment.
2. Enumerate the environment and gather information: The next step is to gather as much information as possible about the target environment. This can involve using tools such as kubesec and kube-hunter to scan the target environment and identify potential vulnerabilities and misconfigurations.
3. Identify vulnerabilities and exploit them: The next step is to identify potential vulnerabilities in the target environment and exploit them to gain access to the system. This can involve using tools such as kube-exploit and kube-crack to identify and exploit vulnerabilities in Kubernetes…” – ChatGPT.

С kube-crack вышло достаточно странно, так как такой утилиты, используя Google найти не удалось. Далее модель упоминала kube-exec, kube-shell, kube-clean, kube-audit но подобные словосочетания как минимум встречаются в интернете.

Продолжив изучение возможностей модели я спросила ее и про тестирование блокчейна. Модель продолжила импровизировать и предложила мне использовать Blockchain Exploit Kit, Blockchain Debugger, Inspector, Cleaner и Auditor. Последние названия совпали с предлагаемыми для Kubernetes.

<figure><img src="https://lh5.googleusercontent.com/IFV0BuXSWsI6oCG2ZuW43AerTGZPaRdhry4BtfZipbmuz22cEGAMluEAChuwApgO5M3FAnF5RyeotCfMVtCQJtcKu6AKs2F9rFErQPq2xyzwB5-HZPjYm7jgEGdLo85VksCJTXM59jchXkXW0UOovQ" alt=""><figcaption></figcaption></figure>

Часть из запрошенных мной методологий получились в той или иной степени адекватными, но надо понимать, что задавая вопросы по хакингу ChatGPT - человек получает не просто ответ теоретика, а теоретика-выдумщика и без сторонней помощи и самостоятельного чтения релевантных ресурсов - это плохой помощник.

И это еще одна причина, почему массового обучения хакеров - не произойдет. А если и произойдет - то выдуманные нейросетью утилиты и методы вряд ли им помогут 🙂

## Фишинговые письма

Если генерация кода с помощью ChatGPT вызывает некоторые сомнения, то, конечно, функция генерации фишинговых писем подобных сомнений не вызывает. Давайте рассмотрим фишинг, сгенерированный с помощью нейросетей и нюансы, которые следует учитывать при генерации.

### Текст: ChatGPT&#x20;

В открытом доступе предлагается уже достаточно большое количество “promts” - предварительно написанных запросов, которые можно отправлять в ChatGPT, используя сочетания клавиш CTRL+C и CTRL+V. Удивительно, что такие вещи пользуются спросом, давайте попробуем использовать “пресет” из документа [Creatively malicious prompt engineering Written and researched by Andrew Patel and Jason Sattler WithSecure Intelligence, January 2023](https://labs.withsecure.com/content/dam/labs/docs/WithSecure-Creatively-malicious-prompt-engineering.pdf)

&#x20;для генерации фишингового письма.

> Write an email sent by \[person1],  a member of the HR department at  \[company1] to \[person2], an  employee. The email should notify  \[person2] that an accusation of improper conduct has been filed against them by another employee  (who shall remain anonymous). The  email should inform \[person1] that  they need to urgently book a meeting  to talk to \[person2] about the allegation. The email should instruct \[person1] to use \[link] to an online  calendar booking system to schedule  the meeting. The writing style should be formal in nature and invoke a sense  of urgency. на русском языке

Здесь мы добавили “на русском языке”, чтобы нейросеть показала нам свое владение великим и могучим.

<figure><img src="https://lh3.googleusercontent.com/IdaBDM5SZwHODzuZnsXtzfoFyp-b6wQbx_ik9JrMsaGdXi1KkIzaXdzlimKaEVG7qzdBt9vPK4_BQJmPf_QyHqhXPkLTgDBsiODwUEKHpGRzMd-YPG6-W4bm4qahMGe90qbzaxpjL-qoZ502BDQNSQ" alt=""><figcaption></figcaption></figure>

Отметим что ChatGPT не всегда корректно и прозрачно выражается на русском языке. Это происходит из-за того, что после получения запроса на русском языке она переводит ваш запрос на английский, обрабатывает его на английском и даёт результат на английском, А затем переводит ответ на русский, поэтому градус неадекватности текста повышается из-за трёх обработок нейросетью.

Как мы видим – пресеты на английском языке нам не подошли, из-за дословного перевода нейросетью фразы о “онлайн-системе бронирования календаря” – она звучит слишком тяжело, но в общем плане текст выглядит неплохо, но ситуация странная.

Давайте самостоятельно попробуем задать тему для письма.&#x20;

<figure><img src="https://lh5.googleusercontent.com/NSQB9F4k11mXHqFsHZSHp1YP5BS5_X3B3ldjinAbTbKe9SweNxMnidj0yrBHQgTvUzfZc3wUCTfoUkdAJKKdvdeCUzDcWm3jlBXi_oPZczPbsIG_W8q_CTu9fFEJ4cLTjgyMklG8P6o33Xyk4J5uCQ" alt=""><figcaption></figcaption></figure>

Если не вчитываться в текст, то он выглядит легитимным, но я бы все равно внесла в него доработки:

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Здесь мы вычеркиваем слово процедуры, так как оно практически дублирует слово “процессы” и вычеркиваем фразу про обращение в случае возникновения вопросов – это лишнее в фишинговой компании.

Резюмируя, можно сказать, что нейросеть с генерацией текста справилась на четверку, а ближе к концу статьи мы рассмотрим еще одну модель, способную генерировать более интересные письма.

### Корпоративный стиль: Midjourney

Если же с генерации текста, мы более-менее с вами справились, то можно переходить к вопросу оформления фишинговых сайтов и e-mail’ов.

<figure><img src="https://lh5.googleusercontent.com/mPCfLoFa0qmsrxyITVe34WG1C3cPjKqTiidedO4Hkyw36bBZUNQ2LtV4IfB289RE9ukb87EMQQXBzTy2C763ewDwNqPfTFjGmRmVWrRbYZPQT2vgEoDyNK668qG9_HdHquoQ17M8d-6YRq9w5Bm1VA" alt=""><figcaption></figcaption></figure>

С помощью Midjourney можно генерировать и шаблоны писем и шаблоны сайтов и даже логотипы вымышленных компаний – она отлично решает вопросы сочетания различных цветов, что помогает в создании фактуры для фишинга – как-никак для социальной инженерии не подойдут допотопные ресурсы и банальные варианты оформления. Генерация шаблонов сайтов и “корпоративных стилей” может помочь в фишинге b2b-ориентированных компаний.

## AI и наступательная безопасность

Интерес к использованию нейросети в качестве помощника хакера появился задолго до ChatGPT и это не первый раз, когда нейросеть применяется для наступательной безопасности.

Например, уже существует нейросеть, обученная по скриншотам определять тип веб-ресурса, что достаточно полезно, когда ты работаешь с большой большим скоупом и с большим количеством веб-приложений. После того, как утилита, например, такая как eyewitness соберет скриншоты можно использовать [Eyeballer](https://github.com/BishopFox/eyeballer) для того, чтобы разобрать приложения по типам - страничка с ошибкой, форма авторизации, припаркованный домен и так далее.&#x20;

После широкого распространения и взлета популярности использования голосовых помощников, использующих нейросети для распознавания речи появилось множество проектов интегрирующих их в операционные системы и позволяющие привязать написание, например, команд в терминале к голосовым командам.

А сейчас, в случае с ChatGPT появился текстовый помощник в виде примитивного скрипта, использующего API OpenAI для конвертации текстовых команд в команды командной строки. Скрипт назвали  [Alice](https://github.com/greshake/Alice) и половина этого скрипта представляет собой сообщение, определяющее дальнейшее поведение модели.

<figure><img src="https://lh5.googleusercontent.com/Wui7f4svofg39ASkHGuS6J6cw29rcJ_kZrBQzLsoNKlxclxq_EDIGGziH0mSu5WzQULLghfiV342984LlQ_nXZX2zE7HcxsCC7geqf7J4m6R5ht7rlhq7-D93Vmhq71FFj3H2ZKsLMPuICcDlTllJQ" alt=""><figcaption></figcaption></figure>

С учетом того, что ChatGPT знает утилиты Kali Linux, вероятно, данный скрипт, можно применять для преобразования текстовых запросов в команды для использования специализированных утилит.

Признаться честно, я сама пишу эту статью, используя преобразователь голоса в текст, иногда это достаточно удобно, поэтому одной из интересных идей для пет-проекта для специалиста по безопасности видится интеграция [Alice](https://github.com/greshake/Alice) с нейросетью, распознающей речь.

## Альтернативные нейросетевые модели

Стоит вспомнить, что ChatGPT основана на языковой модели GPT-3.5. И если разобраться получше, то можно найти, что есть достаточно большое количество аналогичных моделей [GPT-NeoX](https://github.com/EleutherAI/gpt-neox), [GPT-2](https://github.com/openai/gpt-2), [GPT-J](https://www.narrativa.com/gpt-j-an-open-source-alternative-to-gpt-3/) и прочих. Из-за того, что пересечение кибербезопасности с областью Machine Learning достаточно небольшое, многие блогеры и редакторы новостных каналов по информационной безопасности упустили достаточно большое количество компаний и ресурсов с открытым кодом, использующих аналоги модели от OpenAI.

А начнём с того, что у OpenAI есть плейграунд для тестирования бета-версий нейросетей, и одна из моделей, которая бы заинтересовала специалиста по безопасности была бы моделью [Codex](https://openai.com/blog/openai-codex/). Эта модель создавалась для генерации кода, получая на вход запросы в виде текста. Здесь стоит отметить, что является многофункциональным приложением, в то время как большинство моделей зачастую реализуют одну конкретную функцию – либо пишут код либо, либо поддерживают диалог, либо пишут генерируют текст и так далее. [Codex](https://openai.com/blog/openai-codex/), в свою очередь - модель, преобразующая текст в код.

Если рассматривать альтернативные модели, то стоит упомянуть модели представляемые NLP Cloud. У них, так же, как и у OpenAI есть свой playground, в котором можно попробовать использовать GPT-NeoX и GPT-J для [генерации кода и других разнообразных целей](https://nlpcloud.com/home/playground/code-generation). В отличие от ChatGPT эти модели не имеют “цензуры” и не смущаются от использования вами ключевых слов таких hacking, vulnerability, XSS и тому подобных.

<figure><img src="https://lh4.googleusercontent.com/Dp25AiltKQw2dQK7MwU0YZfdHEu26LSdC56eLYCq5lUwpFmoS0MwX5gGndJJ7Q83Wt_arCOka2TLCOpMq0C0l_Vn6SbYFTF75xlbv_oVMsupzftn52Ffx0s8W6dFL4ai_Xd46rCKsOcWMTy6dNYLrw" alt=""><figcaption></figcaption></figure>

Как я уже писала ранее ChatGPT получила такое широкое распространение в силу того, что у нее наиболее дружественный для пользователя интерфейс. Но помимо неё и помимо NLP Cloud в интернете также существует достаточно большое количество подобных как онлайн, так и Open Source решений.

Например, хочется отметить ресурс [Rytr](https://rytr.me/) – генератор текста, который можно использовать в том числе для фишинговых писем. Одной из особенностей данного генератора является изменение “настроения” и тона текста – это может быть и энтузиазм и спешка и многие другие, что крайне полезно для социальной инженерии.

<figure><img src="https://lh6.googleusercontent.com/FNvwlRapr8fcCBPbNXGzPSRGtX0Bt87vGSuYrc0P39c2BX2PpJAKkuHTwKshxk2AjxrDBnyxZyB4n7CINyTJ2sPnLKLXsrKhJ338F1lZqFfW9XsErXJYU03T3p4_Hb2lkTH5r2avUDQh5y5ORvyMZQ" alt=""><figcaption></figcaption></figure>

Дополнительно, стоит отметить ресурс [you.com](https://you.com/for), который представляет AI поисковик, предоставляющий множество дополнительных функций в том числе коллекцию ресурсов для [OSINT](https://you.com/search?q=osint\&fromSearchBar=true\&tbm=yousearch\&niche=osint).

<figure><img src="https://lh6.googleusercontent.com/y6xQDgfxYqXdR_9g3fQY5NSfxHvFyPnE9sXkV2JNiNrYI14rCjk1psJBQ1_hc4juLbol3YJVG4gI_jDX5A8bEy5vYf2dI7dyAtHysEAou_T8Cpmf2QXHcQqnOWPiqMTL13aUZte2Ikiqz8Faa8979g" alt=""><figcaption></figcaption></figure>

\


Если погружаться в вопрос использования нейросетей дальше, то могу посоветовать ресурс [Hugging Face](https://huggingface.co/models?pipeline\_tag=text-generation\&sort=downloads) – с его помощью можно узнать о некоторых вопросах создания нейросетей и напрямую взаимодействовать с множеством моделей, например [GPT-2](https://huggingface.co/gpt2), [GPT-Large](https://huggingface.co/gpt2-large), [GPT-Medium](https://huggingface.co/gpt2-medium) и [GPT-XL](https://huggingface.co/gpt2-xl). А также попробовать создать свою модель и [натренировать](https://huggingface.co/autotrain) на своем датасете.

<figure><img src="https://lh4.googleusercontent.com/m6TnA6dcX2mSNtU7fiPK9CTcmM02xVJPiiewilc4rn9NtEh_iTTpdhBeFwl4IC6uFzYggniDpGVUvIeO4Ukf49zL7Qu_5bGwiTleTN2eMDJMBxCuTkA0ZpHTqZPLmDx32A9AR95DkYt9RbiPkZ-twQ" alt=""><figcaption></figcaption></figure>

## Использование ChatGPT для обучения помощника Red Team оператора&#x20;

В интернете, наряду со статьями, догоняющими панику на специалистов по безопасности:

[ChatGPT Opens New Opportunities for Cybercriminals: 5 Ways for Organizations to Get Ready](https://www.darkreading.com/vulnerabilities-threats/chatgpt-opens-new-opportunities-for-cybercriminals-5-ways-for-organizations-to-get-ready)

[ChatGPT Creates Polymorphic Malware](https://www.infosecurity-magazine.com/news/chatgpt-creates-polymorphic-malware/)

[How hackers might be exploiting ChatGPT](https://cybernews.com/security/hackers-exploit-chatgpt/)

[It’s up to us to determine if generative AI helps or harms our world](https://www.weforum.org/agenda/2023/01/davos23-generativeai-technology-artificial-intelligence/)

Стали появляться статьи с рекомендациями по использованию ChatGPT специалистами по наступательной безопасности, например [OpenAI ChatGPT for Cyber Security](https://infosecwriteups.com/openai-chatgpt-for-cyber-security-4bc602069f9c). Примеры из данной статьи имеют, откровенно говоря, сомнительную ценность, так как человек, который не в состоянии ответить на подобные вопросы – не мог бы получить работу по указанной специальности. Из этого может следовать только одно – то, что в ближайшее время собеседования должны будут переводить в очный формат из-за опасности использования соискателями ChatGPT, либо вопросы для собеседований должны усложнить в разы.

<figure><img src="https://lh4.googleusercontent.com/-TaWfDTXUac-2VLBWz2ZYV23h4qO6Ia3e4BwrmeEX3Ol8NVMtnzPmcShk-j6DFKQ2XKtjZBhxmkhDbF02w1pAKByOaVJq3lTJdGoPayTit7ydtw4_u69EEZI-f5-M5pgV5TDNsMM-oVtI3xfjhDJYw" alt=""><figcaption></figcaption></figure>

Если всё же возвращаться к вопросу – какая нейросеть стала бы неплохим помощником для RedTeam оператора, то им бы стала специально обученная модель на книгах и учебниках, связанных с нашей специальностью. Если говорить о такой модели, как о генераторе кода, то, естественно, ей бы требовалось больше примеров применения различных техник и примеров эксплойтов. Когда мы просим ChatGPT писать код, то очевидно, что она пытается его писать  как обычный программист. У неё нет “мышления” хакера, и она не беспокоится о скрытности, обфускации, IOC’aх, о которых обычно беспокоимся мы.

Из-за того, что ChatGPT мало погружена в нашу специальность у нас возникла мысль о том, что было бы создать кастомную оффенсив нейросеть. Подобную модель можно создать либо переучивая готовую языковую модель, либо создать свою модель с нуля – мы экспериментировали в обоих направлениях. &#x20;

Говорить о создании модели, генерирующей код мне пока рано, потому что Machine Learning не является вопросом, которому я уделяла пристальное внимание последние годы. Но, посвятив данному вопросу два дня я сделала выводы, что для начала создания нейросети, которая бы могла отвечать на вопросы по наступательной безопасности на базе множества книг, например, таких как “How to Hack Like a Ghost” (о которой, к слову, ChatGPT не знала) необходимо создать датасет. В свою очередь хороший датасет это не просто текст, а структура, которая будет использоваться для обучения модели. Чтобы текст преобразовать в структуру (и, например, в дальнейшем обучить свою модель ответам на вопросы) можно использовать ChatGPT.&#x20;

<figure><img src="https://lh6.googleusercontent.com/dLPt8tN2u660nBkaNbxKV7u9JJ_Emi-Z65d5iuhHn14ijZGVnuNry2GZV6s3eXwgHSzpHKJsOhR4sLSk1E5dX9pmsMp_BWFeLCRQ6jPc0Pn4pekULEvty6sEq2QDXI2a58tjsgUSda9eSA7fQNYUxw" alt=""><figcaption></figcaption></figure>

Например, в своем случае я разбила текст книги на абзацы, и просила задавать нейросеть вопросы к получаемому ей тексту и генерировать пары вопрос-ответ, создавая csv файл, имеющий структуру:

абзац текста

вопрос, ответ

вопрос, ответ

…

абзац текста

вопрос, ответ

вопрос, ответ

…

В моем понимании подобного датасета будет достаточно для обучения модели, ищущей по предложению или вопросу необходимый абзац из книги. Подобная модель данная модель должна стать достаточно удобной, так как не вся наша профессиональная литература достаточно проиндексирована поисковыми системами. В любом случае я продолжу свои изыскания в качестве пет-проджекта.

## Заключение&#x20;

Исходя из всего вышесказанного было бы сложно прийти к однозначному заключению.&#x20;

Вот некоторые позитивные и негативные стороны применения нейросетевых моделей, которые хочу отметить:&#x20;

Во-первых, это то, что нам не стоит зацикливаться на ChatGPT, одной из самых дружелюбных моделей для пользователей, и, возможно, она действительно нас спровоцирует новую волну  фишинговых атак в ближайшее время. Но слишком сильно переживать не стоит, потому что в вопросах написания кода людям без соответствующей подготовки она не поможет. Подобные фишинговые атаки, скорее всего, будут направлены на получение учетных данных и не будут ставить целью исполнение кода на ПК получателя подобного письма, поэтому “старые” методы обеспечения безопасности – образование пользователей, обновление версий ПО и прочие – останутся актуальными.

Во-вторых это то, что мы не должны ограничиваться использованием одной этой модели. Следует обращать внимание на то, что существуют другие модели и особенности их генерации будут отличаться от особенностей ChatGPT. Возможно, они будут делать другие ошибки, и, возможно, они смогут найти решения для тех вопросов, на которые ChatGPT ответить нам не смогла.&#x20;

Мы будем держать руку на пульсе и заниматься исследованиями в данном направлении, а также будем стараться опередить продвинутые устойчивые угрозы (APT) в написании нейросетей полностью ориентированных на совершение злонамеренных действий и генерацию вредоносного кода.

\