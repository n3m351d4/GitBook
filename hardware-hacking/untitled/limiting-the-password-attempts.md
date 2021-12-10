---
description: '12.2021'
cover: >-
  https://images.unsplash.com/photo-1550751827-4bd374c3f58b?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHwyfHxjeWJlcnxlbnwwfHx8fDE2MzYyODE0OTQ&ixlib=rb-1.2.1&q=85
coverY: 0
---

# Ограничение попыток ввода пароля

Ограничить нашу процедуру проверки пароля тремя попытками - достаточно сложно, так как количество использованных попыток должно храниться после перезагрузки процессора. EEPROM.write и EEPROM.read -  методы,использующиеся для управления ПЗУ Arduino. Первым аргументом этих методов является адрес памяти - место, где мы будем хранить наш счетчик.

```
bool checkPass(String buffer) {
  byte tries = EEPROM.read(TRIES_ADDR);
  if (tries == 0) {
    return false;
  }
  bool result = true;
  for (int i = 0; i < PASSWORD.length(); i++) {
    if (buffer[i] != PASSWORD[i]) {
      result = false;
    }
  }
  if (result) {
    EEPROM.write(TRIES_ADDR, 3);
  } else {
    EEPROM.write(TRIES_ADDR, tries - 1);
  }
  return result;
}
```

В данном методе проверяется наличие попыток ввода пароля, и, если их не осталось, возвращается false. Если у пользователя осталось несколько попыток, будет выполнена обычная проверка пароля и при неудаче будет уменьшено число оставшихся попыток. Теперь мы можем не беспокоиться о том, что наша процедура проверки пароля безопасна.&#x20;

Но не совсем.&#x20;

Дело в том, что в основном коде мы имеем следующее:

```
  bool correct = checkPass(pass);
  if (correct) {
    Serial.println("Password correct!");
```

Это означает, что существует единственная точка отказа - если `checkPass` завершается неудачно и ошибочно выдает истинное значение, плата будет разблокирована с сообщением `Password correct!`.

Если мы сможем вызвать ошибку и заставить метод checkPass полагать, что пароль правильный, то мы разблокируем плату не используя пароль. CPU ATMega328 имеет минимальное рабочее напряжение и неизвестно, что произойдет, если напряжение упадет ниже этого порога на небольшой промежуток времени.

Мы можем представить, что ЦП хочет потребить некоторую мощность для проведения операции, но мы отключим питание на этот короткий момент, чтобы вызвать ошибку. Отметим, что питание должно быть отключено на небольшой промежуток времени, чтобы это не привело к отключению процессора.&#x20;

Чтобы снизить напряжение на очень короткое время, мы будем использовать транзистор в качестве переключателя с электронным управлением и вторую плату Arduino для управления этим транзистором для быстрого включения и отключения напряжения в надежде, что целевое Arduino не заметит этого.&#x20;

Схема будет выглядит так:

![Voltage fault injection circuit](https://maldroid.github.io/hardware-hacking/assets/fault-injection-circuit.png)

Транзистор отвечает за включение и выключение питания. Большую часть времени питание будет включено (иначе ЦП не будет работать) и вторая плата Arduino отключит его на очень короткое время - несколько циклов работы ЦП. Поскольку мы не знаем сколько должен длиться сбой (действие выключения и включения питания), мы будем попробовать разные значения:

```
  int waste = 0;
  for (int i = 0; i < glitchOffset ; i++) { waste++; }
  digitalWrite(glitchPin, LOW);
  for (int i = 0; i < glitchLength ; i++) { waste++; }
  digitalWrite(glitchPin, HIGH);
  delay(100);
  glitchLength *= 2;
  if (glitchLength > 10000) {
    glitchLength = 1;
    glitchOffset *= 2;
  }
```

В этом коде два ключевых параметра: `glitchOffset` и `glitchLength`. Первый отвечает за момент отключения питания, а второй за его продолжительность. Дополнительно, учтена задержка в 100 мс для стабилизации тока. Иначе из-за частоты просадок напряжения ЦП может не хватить мощности для включения. Взгляните на таблицу ниже:

![Glitching voltage chart](https://maldroid.github.io/hardware-hacking/assets/glitch.png)

В нашем коде длительность "просадок" напряжения постепенно увеличивается. Теперь, когда у нас есть наша схема, мы можем начать эксперимент. Как выглядит вызванный нами глитч? Взгляните на GIF-изображении ниже.

![Serial output of a glitching CPU](https://maldroid.github.io/hardware-hacking/assets/fault-injection-terminal.gif)

It’s important to note that we are not actually sending any data to the board. Any password guess at all. The CPU itself displays all the information because of the glitching. You can see at the end that we got the `Password correct!` message without providing any password. This happened even though the board was locked and didn’t allow any password guesses! Isn’t this magical?

It’s time for a summary and - if you still have questions left - a bit of a FAQ.

Важно отметить, что на самом деле мы не отправляем никаких данных на на плату - CPU отображает эту информацию из-за сбоя. В конце вы увидите, что мы получили сообщение `Password correct!` без указания пароля. Это произошло, несмотря на то, что плата была заблокирована и не позволяла подбирать пароль! Разве это не волшебно?
