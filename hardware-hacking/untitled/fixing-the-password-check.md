# Улучшение проверки пароля

[**Источник**](https://maldroid.github.io/hardware-hacking/)

Самое простое решение - не оптимизировать цикл for, а проверять каждый символ пароля следующим образом:

```
bool checkPass(String buffer) {
  bool result = true;
  for (int i = 0; i < PASSWORD.length(); i++) {
    if (buffer[i] != PASSWORD[i]) {
      result = false;
    }
  }
  return result;
}
```

Здесь нас ожидает небольшая проблема: в зависимости от того, сколько символов пароля неверны, проверка займет больше времени (с учетом необходимости обновления переменной `result`). Таким образом, если у нас есть один правильный символ, а остальные неправильные, проверка пароля будет быстрее, чем если бы все символы были неправильными.

Cможем ли мы измерить эту очень разницу? Попытка повторить процедуру в поисках самой быстрой проверки пароля, дает неубедительные результаты:

```
63370.0 16 
63358.5 229 �
63346.0 47 /
63314.0 17 
63268.5 0
```

P отсутствует в списке и различия между байтовыми значениями невелики. Значит, проблема решена и наша процедура проверки пароля безопасна!

Но не совсем.&#x20;

Логический анализатор Saleae имеет аналоговые каналы, что делает его немного похожим на осциллограф (это упрощение!). Это означает, что некоторые каналы могут быть использованы не только для считывания логического уровня напряжения, но и для измерения фактического значения напряжения. Почему это важно?

Различные инструкции ЦП имеют разное энергопотребление. Интуитивно (опять же, это упрощение) изменение двух битов будет сопровождаться меньшим потреблением энергии, чем изменение одного бита. Точно так же установка логического значения false приведет к изменению потребления энергии, чем не установка (это должно быть довольно очевидно). Так что, может быть, мы можем просто записать потребление энергии и посмотреть, изменится ли оно, когда попробуем `xxxxx` и `pxxxx`? &#x20;

Сначала мы должны убедиться, что измерение мощности является максимально точным. Для этого нам нужно вынуть микросхему ATMega328, расположить ее на макетной плате и измерить ее энергопотребление, а не всей платы. Причина в том, что плата содержит конденсаторы (как показано в начале этого курса), и эти конденсаторы предназначены для стабилизации тока. Это может повлиять на наши показания.&#x20;

Итак, первое, что нужно сделать, это вынуть ЦП, положить его на макетную плату и снова подключить все выводы ЦП к плате. Это позволит нам удобно взаимодействовать с процессором, используя тот же USB-разъем, который мы использовали раньше.

![](https://maldroid.github.io/hardware-hacking/assets/atmega-breadboard.jpg)

На фотографии видно, что я подключил не все контакты обратно к плате. Нам не нужны все контакты - некоторые из них отвечают за цифровые или аналоговые входы от внешних источников, которые мы не используем. Нам нужны:&#x20;

* Сброс (чтобы мы могли использовать кнопку сброса и перезагрузить ЦП)&#x20;
* &#x20;RX и TX (для связи с ЦП)
* Питание и заземление ( чип должен быть как-то запитан)&#x20;
* Clock (чтобы ЦП знал, когда выполнять инструкции)

… and that’s all. In total that’s 7 pins connected back to the board. You can find out which ones are which by looking at the [ATMega328p datasheet](https://maldroid.github.io/hardware-hacking/assets/atmega-datasheet.pdf) - take a look at the upper right corner of page 2.

Now that it’s all connected back we run into another small problem. The logic analyser, much like the oscilloscope, can only measure voltage, not current. However, the CPU power usage will alter current and not voltage - voltage stays more or less the same.

It’s a bit like the pressure in your water tap (voltage) and the actual water flow from your tap (current). When you wash your hands (and please do it often!) you’re turning the tap on and the water (current) starts to flow, because you want to use the water. Pressure stays more or less the same.

So we have to measure the power current, but we can only measure the voltage. How can we do that? Well, there are rather expensive current probes which you can buy and they use [the Hall Effect phenomenon](https://en.wikipedia.org/wiki/Hall\_effect) to measure the current by clamping over the wire. However, there’s also a cheaper solution - using a small resistor.

According to [Ohm’s law](https://en.wikipedia.org/wiki/Ohm's\_law) the current is simply a voltage divided by resistance or, if you rearrange the variables, voltage is current times resistance.

![](https://render.githubusercontent.com/render/math?math=V%20=%20I%20\*%20R)

If the resistance is fixed (e.g. over a resistor with a fixed value) then any changes in current will result in changes in voltage multiplied by that resistance. So, if we have an 1 Ohm resistor every change in current (in amps) will result in the same numerical change in voltage (in volts).

If we have a resistor with a higher resistance then every small change in the current will result in an amplified change in voltage. However, if the resistance is too high the CPU won’t be able to draw the power needed to perform the operations. The CPU runs at a specific voltage (in this case 5V) and if any change in current results in a change in voltage then at some point there won’t be enough voltage to make that change and the CPU will power down.

It seems like we have to choose the resistor carefully. Fortunately ATMega328 is a rather indestructible CPU and we can choose a rather high value. In fact we will be using the resistor below. Can you figure out what the resistance value is just by looking at the resistor? Try looking for the “resistor colour code” on Google.

![Resistor used to measure the current value](https://maldroid.github.io/hardware-hacking/assets/resistor.jpg)

The way to read the resistance value is to look at the colourful bands and compare them with the chart below. Our resistor has 5 bands: yellow, violet, black, gold and red. [Digikey provides a very convenient resistor colour code calculator](https://www.digikey.com/en/resources/conversion-calculators/conversion-calculator-resistor-color-code-5-band) where you can enter the colour values and you will get the resistance value. In our case it’s 47 Ohms.

![Digikey calculated resistance](https://maldroid.github.io/hardware-hacking/assets/digikey-resistance.png)

So every change in current will be multiplied by 47 and result in that change in the voltage. Now that everything is ready, the only thing left to do is to connect our resistor on the CPU ground wire, connect logic analyser to RX, TX and to the resistor (in order to measure voltage) and gather the data!

![Circuit to measure the current](https://maldroid.github.io/hardware-hacking/assets/power-analysis-circuit.png)
