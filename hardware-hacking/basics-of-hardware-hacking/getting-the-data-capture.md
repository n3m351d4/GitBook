# Getting the data capture

We will be using the red colour version of the Saleae Logic Analyser \(because red is faster than black, obviously\). Since we want to intercept RX and TX we will connect channel 0 to RX and channel 1 to TX, as shown below. We should also make sure to connect Arduino Uno ground to the logic analyser ground. This is done using the black wire.

![Logic analyser connected to the Arduino board](https://maldroid.github.io/hardware-hacking/assets/saleae_uno.jpg)

As you know form the code the protocol we are using is the serial protocol, also called UART. This means that the data is sent and received using two different wires \(or channels\). One - TX - is used to transmit the data from the board to our computer and the other one - RX - is used to end the data from the computer to the board where our password checking code runs. Let’s record some communication. First we need to connect Arduino to our computer and send a password guess. This whole communication has to be recorded on our Logic Analyser.

After the recording is done let’s open it in the Logic software. [Download this data capture of the communication](https://maldroid.github.io/hardware-hacking/assets/password_try.logicdata) and open the Logic software. Then click on “Options” \(upper right corner\) and choose “Open capture / setup” and choose the downloaded file. Your screen should look similar to the screen below.

![Opened capture file](https://maldroid.github.io/hardware-hacking/assets/logic_screenshot_0.png)

The main window contains two channels: channel 0 and channel 1. Channel 0 is connected to RX \(the pin which receives the data from the computer\) and channel 1 is connected to TX \(the pin which transmits the data to the computer\). You can also see that both channels are mostly high with some “dips.” These “dips” are where the data transmission is taking place. Zoom in \(by scrolling\) on the first dip in channel 1. You should see a series of highs and lows like shown below.

![First data transmission](https://maldroid.github.io/hardware-hacking/assets/logic_screenshot_1.png)

Since this is channel 1 it means that the transmission is coming from the board to the computer. If you remember our serial communication session with `picocom` you might’ve already guessed what this is. This is most likely the `Password:` prompt we get from the board asking us to enter the correct password. But how is this encoded? What do the squiggly lines actually mean? Well, for that you need to go to the next part!

