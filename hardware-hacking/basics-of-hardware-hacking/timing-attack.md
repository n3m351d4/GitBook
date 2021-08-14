# Атака по времени

[**Источник**](https://maldroid.github.io/hardware-hacking/)

Создайте новый анализатор для нулевого \(0\) канала, так же, как вы это делали для первого \(1\). Теперь мы видим,  что в захваченных нами данных есть два потенциальных пароля - `xxxxx`и `pxxxx`. Первый символ второго потенциального пароля является верным, поэтому для его подтверждения потребуется больше времени.

Для того, чтобы это выяснить, нам нужно узнать сколько времени проходит между концом передачи последнего символа по каналу 0 \(это канал, куда мы отправляем предпологаемый пароль\) и началом ответа `Incorrect password!` на канале 1. Для этого нам нужно увеличить \(прокручивая\) вторую паузу на канале 1. Перейдите в меню «Аннотации» справа и нажмите `A1`. Прикрепите маркер синхронизации к концу передачи на канале 0 \(где канал переходит в высокий уровень\). Щелкните `A2`и привяжите его к началу передачи на канале 1 \(где канал переходит в низкий уровень\).

Ваш анализатор должен выглядеть так, как показано на скриншоте ниже.

![&#x41E;&#x442;&#x43C;&#x435;&#x442;&#x43A;&#x438; &#x432;&#x440;&#x435;&#x43C;&#x435;&#x43D;&#x438;](https://maldroid.github.io/hardware-hacking/assets/logic-screenshot-timing.png)

Вы увидите, что в данном случае для проверки пароля потребовалось 62,4 микросекунды. Теперь проделайте то же самое со вторым вариантом пароля \(третья пауза на канале 1\). Сначала уменьшите масштаб, а затем увеличьте последнюю паузу. Щелкните `+`рядом с меню «Аннотации» и добавьте новую пару временных маркеров. Поместите маркеры в те же позиции, что и раньше. Теперь ваше измерение должно показать 61,6 микросекунды. Подождите, но это ... меньшее время?

Вот вам некоторая информация о временных атаках и процессорах. У процессоров крайне много дел, они очень заняты, даже если простаивают и разница между одним правильным символом пароля и отсутствием правильного пароля настолько мала, что иногда ее просто нет - и это то, что мы усвоили на собственном горьком опыте. Итак ... есть ли способ сделать атаку более точной?

Если мы предположим, что существует некоторая случайная задержка, то резонно собрать множество сэмплов и усреднить их, что мы и будем делать. Я написал простой скрипт на Python, который проверяет разные пароли.

```text
from __future__ import print_function
import sys, serial

password = [0,0,0,0,0]
offset = 0
port = sys.argv[1]

with serial.Serial(port, 115200, timeout=10) as ser:
        for c in xrange(256):
            password[offset] = c 
            print("Trying {}...".format(c))
            for i in xrange(100):
                ser.read_until(b":")
                ser.write(password)
```

Скрипт перебирает все значения байтов от 0 до 255 и пробует пароль, который начинается с этого байта, а для остальных байтов устанавливает значение `0`. Каждый байт проверяется 100 раз. Теперь мы можем записать все эти попытки с помощью нашего логического анализатора и вычислить среднее время, необходимое для проверки пароля с каждым символом.

Все отснятые образцы выглядят так:

![&#x41F;&#x440;&#x438;&#x43C;&#x435;&#x440;&#x44B; &#x43F;&#x430;&#x440;&#x43E;&#x43B;&#x435;&#x439;](https://maldroid.github.io/hardware-hacking/assets/logic-screenshot-password-tries.png)

Так как данных много, нам нужно найти способ их обработки. Самый простой способ - это экспортировать данные в файл формата VCD \(Variable Change Dump\). Этот формат файла довольно прост. Давайте посмотрим на пример ниже.

```text
$timescale 1 ns $end
$scope module top $end
$var wire 1 ! Channel_0 $end
$var wire 1 " Channel_1 $end
$upscope $end
$enddefinitions $end
#2792332400
1!
1"
#2793504800
0!
#2793581600
1!
#2793590000
0!
```

Он начинается с преамбулы, в которой указывается степень детализации временных меток \(1 наносекунда\) и определения каналов \(канал 0 будет использовать восклицательный знак, а канал 1 будет использовать кавычки\). Затем каждая строка начинается с хеша \( `#`\), что означает, что она содержит отметку времени или значение и символ канала \(`1!`- «1» на канале 0\). Это означает, что значение на каком-то канале изменилось на другое. Например, это `1"`означает, что значение изменилось с `0`на `1`на канале`"`, то есть на канале 1.

[Вы можете скачать файл VCD отсюда](https://maldroid.github.io/hardware-hacking/assets/password_tries_100.vcd.zip) . Разархивируйте и посмотрите. Следующим шагом будет автоматическая обработка данных, чтобы мы могли определить первый символ пароля.
