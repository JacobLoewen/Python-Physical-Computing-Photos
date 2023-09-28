# Lab Assignment #2: LEDs and Push Buttons
#### Wednesday, September 26, 2023

##Project 1:
###Description:

This assignment primarily uses the LED bar graph and two push buttons. When one of the buttons is pressed, the lights on the LED bar go from left to right. When the other button is pressed, the lights on the LED bar go from right to left immediately from where the LED was previously.

###List of Hardware Components:
- Resistor 10kΩ x2
- Resistor 220Ω x10
- Push button x2
- Jumper M/M x12
- LED bar graph x1

###Circuit Diagram: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L2P1_CircuitDiagram.png)

###Photo of Breadboard: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L2P1_photo.jpg)

###Breadboard View: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L2P1_BreadboardView.png)

###Source Code:

```
import time
from machine import Pin

pins=[15,2,0,4,5,18,19,21,22,23]

button1 = Pin(12, Pin.IN,Pin.PULL_UP)
button2 = Pin(13, Pin.IN,Pin.PULL_UP)

c = -1 #checker

length=len(pins)
temp = -1

while True:
    i = -1
    if not button1.value():
        c = 0
        i = length-1
        break
    if not button2.value():
        c = 1
        i = 0
        break
  
while True:
     if c == 0:
         i += 1
         if i == length:
             i = 0
         elif i == -1:
             i = length-1
         led=Pin(pins[i],Pin.OUT)
         led.value(1)
         if not button2.value():
             c = 1
         time.sleep_ms(100)
         led.value(0)
     if c == 1:
         if i == length:
             i = 0
         elif i == -1:
             i = length-1
         i -= 1
         led=Pin(pins[i],Pin.OUT)
         led.value(1)
         if not button1.value():
             c = 0
         time.sleep_ms(100)
         led.value(0)
```

###Animated Gif of Project: 
![alt text](https://github.com/JacobLoewen/Python-Physical-Computing-Photos/blob/main/L2P1_GIF.gif?raw=true)

##Project 2:
###Description:

This assignment primarily uses the NeoPixels and RGB LED. The task indicates using a button to alternate which color the NeoPixels and RGB LED show in the pattern of the rainbow. So when the button is pressed, it goes in order of red, orange, yellow, green, blue, indigo, violet, red, orange, and so on.

###List of Hardware Components:
- Resistor 10kΩ x1
- Resistor 220Ω x3
- Push button x1
- Jumper M/M x6
- Jumper F/M x3
- RGB LED x1
- NeoPixels x1

###Circuit Diagram: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L2P2_CircuitDiagram.png)

###Photo of Breadboard: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L2P2_photo.jpg)

###Breadboard View: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L2P1_BreadboardView.png)

###Source Code:

```
from machine import Pin,PWM
from random import randint
import time
import neopixel
pin = Pin(5, Pin.OUT)
np = neopixel.NeoPixel(pin, 8)
pins=[0,2,15]
pwm0=PWM(Pin(pins[0]),10000)
pwm1=PWM(Pin(pins[1]),10000)
pwm2=PWM(Pin(pins[2]),10000)

button = Pin(12, Pin.IN,Pin.PULL_UP)

i = -1
click = 0

colors = [[1023,0,0],[1023,63,0],[511,127,0],[0,1023,0],[0,0,1023],[255,0,255],[63,0,127]]

def setColor(r,g,b):
 pwm0.duty(1023-r)
 pwm1.duty(1023-g)
 pwm2.duty(1023-b)

try:
 while True:
     if not button.value() and click == 0:
         click = 1
         if i > 6:
             i = 0
         #color[] = colors[i]
         setColor(colors[i][0],colors[i][1],colors[i][2])
         for j in range(0,8):
             np[j]=(colors[i])
             np.write()
         #print("Colors: " + str(colors[i][0]) + ", " + str(colors[i][1]) + ", " + str(colors[i][2]))
         print("i " + str(i))
     elif button.value() and click == 1:
         click = 0
         i += 1
         print("Click: " + str(click))
     #print("Click: " + str(click))
except:
 pwm0.deinit()
 pwm1.deinit()
 pwm2.deinit()

```

###Animated Gif of Project: 
![alt text](https://github.com/JacobLoewen/Python-Physical-Computing-Photos/blob/main/L2P2_GIF.gif?raw=true)