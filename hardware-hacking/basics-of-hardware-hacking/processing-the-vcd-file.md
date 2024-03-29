---
description: 'Перевод: @beh1ndy0urback 09/2021'
cover: >-
  https://images.unsplash.com/photo-1550751827-4bd374c3f58b?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHwyfHxjeWJlcnxlbnwwfHx8fDE2MzYyODE0OTQ&ixlib=rb-1.2.1&q=85
coverY: 0
---

# Обработка VCD файла

[**Источник**](https://maldroid.github.io/hardware-hacking/)

Мы ищем период времени между отправкой последнего символа на канале 0 и отправкой первого символа на канале 1. Последний бит всегда будет обозначаться в файле как 1!, а стартовый как 0”. Так мы сможем эффективно найти временные различия между 1! и 0”.

Теперь, если вам интересно, можете самостоятельно написать скрипт, который рассчитает нужный нам период времени. Вся необходимая для этого информация есть: формат файла и что мы ищем. Так что идите и пытайтесь. Будет весело, я обещаю.

Но если самостоятельно вы это делать не хотите, то вот готовый скрипт на Python:

```
from __future__ import print_function
import sys

f = open(sys.argv[1])
last = False
timestamp = 0

for line in f:
    if line[0] == '$':                               # пропустить преамбулу
        continue
    elif line[0] == '#':                             # отметка времени
        prev_timestamp = timestamp
        timestamp = int(line[1:])
    elif line[0] == '0' and line[1] == '"' and last: # символ, который нам интересен
        print(timestamp - prev_timestamp)            # вывод
        last = False
    elif line[0] == '1' and line[1] == '!':          # следующий символ
            last = True
```

Этот скрипт выведет время проверки пароля в наносекундах. Следующим шагом является сопоставление времени с проверенными нами байтовыми значениями. Первые 100 меток времени будут байтом 0, следующие 100 будут байтом 1 и так далее. Таким образом, вместо того, чтобы печатать значения, мы можем сохранить их в словаре с ключом по значению байтов. Оператор печати:

```
timestamps.setdefault(byte, []).append(timestamp - prev_timestamp)
if len(timestamps[byte]) == 100:
    byte += 1
```

Этот код поместит все различия во времени в словарь, и если конкретный ключ уже имеет 100 значений, нам нужно переключиться на следующий байт. Теперь осталось только вычислить среднее и отсортировать значения. Помните, что мы ищем значение байта, для которого среднее время проверки является наибольшим.

```
for byte in timestamps:
    timestamps[byte] = 1.0 * sum(timestamps[byte]) / len(timestamps[byte])

ordered_keys = sorted(timestamps, key = timestamps.get)
for byte in ordered_keys:
    print("{} {} {}".format(timestamps[byte], byte, chr(byte)))
```

Этот код просматривает каждое значение и вычисляет среднее. Затем он сортирует все ключи по значениям и печатает их. Результат должен выглядеть так:

```
57300.0 138 �
57316.0 32  
57480.0 80 P
57480.0 99 c
58088.0 112 p
```

Итак, в среднем больше всего времени потребовалось для проверки пароля, начинающегося с p. Это первый символ пароля? Что ж, мы можем подтвердить это. Давайте посмотрим на разницу во времени, необходимом для проверки различных вариантов:

| Byte | Time to validate | Time difference |
| ---- | ---------------- | --------------- |
| 112  | 58088 ns         | —               |
| 99   | 57480 ns         | 608 ns          |
| 80   | 57480 ns         | 0 ns            |
| 32   | 57316 ns         | 164 ns          |
| 138  | 57300 ns         | 16 ns           |

Вы можете видеть, что значения отличаются друг от друга по времени не так выраженно, как 112 (p). Это подтверждает наше предположение, что p - первый символ пароля. И тот код, который мы загрузили в Arduino Uno, указывает, что пароль “passw..”.

Так или иначе, нам удалось получить первую букву пароля. Теперь, когда мы знаем первую букву, мы можем повторить этот процесс для второй буквы (используя p как первую и заполняя последние 3 символа нулями) и так далее, пока у нас не будет всего пароля.

Остается вопрос: как это ускорило процесс подбора пароля?

Перебор пароля: ![](https://render.githubusercontent.com/render/math?math=256^5%20=%201,099,511,627,776)

Атака по времени: ![](https://render.githubusercontent.com/render/math?math=\(256%20\times%20100\)%20\times%205%20=%20128,000)

Таким образом, атака по времени в 8,5 миллионов раз быстрее. На Arduino Uno вся атака от начала до конца займет около 10 минут.

В следующей части мы попытаемся исправить метод Checkpass, чтобы сделать атаку по времени неосуществимой.
