# Lab Assignment #1: LEDs and Push Buttons

##### Wednesday, Septemnber 20, 2023

## Description:
This assignment consists of two projects:

1. A circuit with red, green, and blue LEDs that sequentially and continually blinks respectively. When one LED turns off, the next one is on, and so on.
2. A circuit with two push buttons and one (red) LED. One of the buttons turns the LED on while the other turns the LED off. Pressing the same button multiple times will not change the LED's status.

## List of hardware components:

Project 1: 
- Jumper M/M x6
- LED x3 (1 Red, 1 Green, 1 Blue)
- Resistor 1kΩ x3
- Circuit Diagram: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/Lab1_Assignment1_Diagram.png)
- Breadboard Photo: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/lab1_asngm1_birdview.jpg)
- Source Code:
```
from time import sleep_ms
from machine import Pin

led1=Pin(0,Pin.OUT)
led2=Pin(2,Pin.OUT)
led3=Pin(15,Pin.OUT)
try:
    while True:
        led1.value(1)       
        sleep_ms(1000)
        led1.value(0)        
        led2.value(1)
        sleep_ms(1000)
        led2.value(0)
        led3.value(1)
        sleep_ms(1000)
        led3.value(0)
except:
    pass
```
- GIF of the Project: ![alt text](https://github.com/JacobLoewen/Python-Physical-Computing-Photos/blob/main/lab1_assignment1_gif.gif?raw=true)

#
#
#

Project 2:
- Jumper M/M x6
- LED x1 (Red)
- Resistor 1kΩ x1
- Resistor 10kΩ x4
- Push button x2
- Circuit Diagram: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/Lab1_Assignment2_Diagram.png)
- Breadboard Photo: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/lab1_asngm2_birdview.jpg)
- Source Code:
```
from machine import Pin

led = Pin(2, Pin.OUT)

#create button object from pin13,Set Pin13 to Input
button1 = Pin(13, Pin.IN,Pin.PULL_UP)

button2 = Pin(15, Pin.IN,Pin.PULL_UP)


try:
    while True:
      if not button1.value():     
        led.value(1)  #Set led turn on
      if not button2.value():
        led.value(0)  #Set led turn off
except:
    pass
```
- GIF of the Project: ![alt text](https://github.com/JacobLoewen/Python-Physical-Computing-Photos/blob/main/lab1_assignment2_gif.gif?raw=true)