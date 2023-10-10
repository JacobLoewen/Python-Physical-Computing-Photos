# Lab Assignment #4: TouchSensor, Photoresistor, and Thermistor

#### Wednesday, October 11, 2023

##Project 1:
###Description:

This assignment uses seven touch sensors on the ESP32. Each sensor will light up a unique LED on the LED Bar and make a unique tone on the Passive Buzzer (one of do, re, mi, fa, sol, la, and si).

###List of Hardware Components:
- Resistor 220Ω x7
- Resistor 1kΩ x1
- Jumper M/M x18
- Passive Buzzer x1
- LED Bar
- NPN Transistor x1

###Circuit Diagram: ![alt text](https://github.com/JacobLoewen/Python-Physical-Computing-Photos/blob/main/L4P1_CircuitDiagram.png?raw=true)

###Photo of Breadboard: ![alt text](https://github.com/JacobLoewen/Python-Physical-Computing-Photos/blob/main/L4P1_photo.jpg?raw=true)

###Breadboard View: ![alt text](https://github.com/JacobLoewen/Python-Physical-Computing-Photos/blob/main/L4P1_BreadboardView.png?raw=true)

###Source Code:

```
import time
from machine import TouchPad,Pin,PWM

pins=[22,23,0,5,18,19,21]

PRESS_VAL=150
RELEASE_VAL=200

tp1 = TouchPad(Pin(4,Pin.IN,Pin.PULL_UP))
tp2 = TouchPad(Pin(32,Pin.IN,Pin.PULL_UP))
tp3 = TouchPad(Pin(33,Pin.IN,Pin.PULL_UP))
tp4 = TouchPad(Pin(14,Pin.IN,Pin.PULL_UP))
tp5 = TouchPad(Pin(15,Pin.IN,Pin.PULL_UP))
tp6 = TouchPad(Pin(12,Pin.IN,Pin.PULL_UP))
tp7 = TouchPad(Pin(13,Pin.IN,Pin.PULL_UP))

tp = [tp1, tp2, tp3, tp4, tp5, tp6, tp7]

passiveBuzzer=PWM(Pin(25),2000)

toneVal = [262,294,330,350,393,441,495]

def sensor():
    for i in range(7):
        led=Pin(pins[i], Pin.OUT)
        if tp[i].read() < PRESS_VAL:
            passiveBuzzer.init()
            led.value(1)
            passiveBuzzer.freq(toneVal[i])
            time.sleep_ms(100)
        elif tp[i].read() > RELEASE_VAL:
            led.value(0)
            passiveBuzzer.deinit()
            time.sleep_ms(100)
        
            
while True:
    sensor()


```

###Video: 
[![](https://markdown-videos-api.jorgenkh.no/youtube/9CZxZHQrxV4)](https://youtube.com/shorts/9CZxZHQrxV4)


##Project 2:
###Description:

This assignment uses a photoresistor to detect whether the light is on or off. If the light is off, the NeoPixel will have red lights to represent time to sleep. When the lights are on, the NeoPixel will have its lights going on at random colors, changing at a fast pace, and the buzzer will also sound. Once the button is pressed, the lights on the NeoPixel will turn off and the buzzer will stop making sound.

###List of Hardware Components:
- Resistor 10kΩ x3
- Resistor 220Ω x1
- Jumper M/M x8
- Passive Buzzer x1
- Push Button x1
- Photoresistor x1

###Circuit Diagram: ![alt text](https://github.com/JacobLoewen/Python-Physical-Computing-Photos/blob/main/L4P2_CircuitDiagram.png?raw=true)

###Photo of Breadboard: ![alt text](https://github.com/JacobLoewen/Python-Physical-Computing-Photos/blob/main/L4P2_photo.jpg?raw=true)

###Breadboard View: ![alt text](https://github.com/JacobLoewen/Python-Physical-Computing-Photos/blob/main/L4P2_BreadboardView.png?raw=true)

###Source Code:

```
from machine import Pin,PWM,ADC
import neopixel
import time
from random import randint

breakVal = 0

DAYTIME = 300

activeBuzzer=Pin(13,Pin.OUT)
button = Pin(12, Pin.IN,Pin.PULL_UP)

pwm =PWM(Pin(25,Pin.OUT),1000)
adc=ADC(Pin(36))
adc.atten(ADC.ATTN_11DB)
adc.width(ADC.WIDTH_10BIT)

activeBuzzer.value(0)
pin = Pin(2, Pin.OUT)
np = neopixel.NeoPixel(pin, 8)

brightness=10                                
colors=[[brightness,0,0],                    #red
        [0,brightness,0],                    #green
        [0,0,brightness],                    #blue
        [brightness,brightness,brightness],  #white
        [0,0,0]]

while breakVal == 0:
    
    adcValue=adc.read()
    pwm.duty(adcValue)
    print(adc.read())
    time.sleep_ms(100)
    
    if adcValue > DAYTIME:
        for i in range(8):
            np[i]=colors[0]
            np.write()
            time.sleep_ms(5)
    
    elif adcValue < DAYTIME:
        while breakVal == 0:
            if not button.value():
                for i in range(8):
                    np[i]=colors[4]
                    np.write()
                activeBuzzer.value(0)
                
                breakVal = 1
                break
            else:
                np[randint(0, 7)]=colors[randint(0, 4)]
                np.write()
                time.sleep_ms(5)
                activeBuzzer.value(1)

```

###Video: 
[![](https://markdown-videos-api.jorgenkh.no/youtube/2TFeCpD7IqI)](https://youtube.com/shorts/2TFeCpD7IqI)