# Lab Assignment #3: Buzzers, Serial Communication, and AD/DA Converters
#### Wednesday, October 5, 2023

##Project 1:
###Description:

This assignment uses two buttons, a speaker, and a passive buzzer. One of the buttons, when pressed, will make the LED blink and then play the song "Row, Row, Row Your Boat" while the other button, when pressed, will keep the LED on while the song "Mary Had A Little Lamb" plays.

###List of Hardware Components:
- Resistor 10kΩ x2
- Resistor 1kΩ x2
- Push button x2
- Jumper M/M x10
- Passive Buzzer x1
- LED (Green) x1
- NPN Transistor x1

###Circuit Diagram: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L3P1_CircuitDiagram.png)

###Photo of Breadboard: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L3P1_photo.jpg)

###Breadboard View: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L3P1_BreadboardView.png)

###Source Code:

```
from machine import Pin,PWM
import math
import time

button1=Pin(4,Pin.IN,Pin.PULL_UP)
button2=Pin(5,Pin.IN,Pin.PULL_UP)
passiveBuzzer=PWM(Pin(13),2000)
led=Pin(14,Pin.OUT)



def maryHadALittleLamb():
    passiveBuzzer.init()
    note = 500
    rest = 20
    passiveBuzzer.freq(659) #E
    time.sleep_ms(note)
    passiveBuzzer.freq(587) #D
    time.sleep_ms(note)
    passiveBuzzer.freq(523) #C
    time.sleep_ms(note)
    passiveBuzzer.freq(587) #D
    time.sleep_ms(note)
    passiveBuzzer.freq(659) #E
    time.sleep_ms(note)
    passiveBuzzer.freq(10)  #Pause
    time.sleep_ms(rest)
    passiveBuzzer.freq(659) #E
    time.sleep_ms(note)
    passiveBuzzer.freq(10)  #Pause
    time.sleep_ms(rest)
    passiveBuzzer.freq(659) #E
    time.sleep_ms(note)
    
    passiveBuzzer.freq(587) #D
    time.sleep_ms(note)
    passiveBuzzer.freq(10)  #Pause
    time.sleep_ms(rest)
    passiveBuzzer.freq(587) #D
    time.sleep_ms(note)
    passiveBuzzer.freq(10)  #Pause
    time.sleep_ms(rest)
    passiveBuzzer.freq(587) #D
    time.sleep_ms(note)
    passiveBuzzer.freq(659) #E
    time.sleep_ms(note)
    passiveBuzzer.freq(784) #G
    time.sleep_ms(note)
    passiveBuzzer.freq(10)  #Pause 
    time.sleep_ms(rest)
    passiveBuzzer.freq(784) #G
    time.sleep_ms(note)
    
    passiveBuzzer.freq(659) #E
    time.sleep_ms(note)
    passiveBuzzer.freq(587) #D
    time.sleep_ms(note)
    passiveBuzzer.freq(523) #C
    time.sleep_ms(note)
    passiveBuzzer.freq(587) #D
    time.sleep_ms(note)
    passiveBuzzer.freq(659) #E
    time.sleep_ms(note)
    passiveBuzzer.freq(10)  #Pause
    time.sleep_ms(rest)
    passiveBuzzer.freq(659) #E
    time.sleep_ms(note)
    passiveBuzzer.freq(10)  #Pause
    time.sleep_ms(rest)
    passiveBuzzer.freq(659) #E
    time.sleep_ms(note)
    passiveBuzzer.freq(10)  #Pause
    time.sleep_ms(rest)
    
    passiveBuzzer.freq(659) #E
    time.sleep_ms(note)
    passiveBuzzer.freq(587) #D
    time.sleep_ms(note)
    passiveBuzzer.freq(10)  #Pause
    time.sleep_ms(rest)
    passiveBuzzer.freq(587) #D
    time.sleep_ms(note)
    passiveBuzzer.freq(659) #E
    time.sleep_ms(note)
    passiveBuzzer.freq(587) #D
    time.sleep_ms(note)
    passiveBuzzer.freq(523) #C
    time.sleep_ms(note)
    
    passiveBuzzer.freq(10) 

def rowYourBoat():
  passiveBuzzer.init()
  
  passiveBuzzer.freq( 262 ) #C4
  time.sleep_ms(400) 
  passiveBuzzer.freq( 262 ) #C4
  time.sleep_ms(400) 
  passiveBuzzer.freq( 262 ) #C4
  time.sleep_ms(400)
  passiveBuzzer.freq( 294 ) #D4
  time.sleep_ms(200) 
  passiveBuzzer.freq( 330 ) #E4
  time.sleep_ms(400) 
  passiveBuzzer.freq( 330 ) #E4
  time.sleep_ms(300) 
  passiveBuzzer.freq( 294 ) #D4
  time.sleep_ms(200) 
  passiveBuzzer.freq( 330 ) #E4
  time.sleep_ms(300) 
  passiveBuzzer.freq( 349 ) #F4
  time.sleep_ms(200) 
  passiveBuzzer.freq( 392 ) #G4
  time.sleep_ms(700) 
  passiveBuzzer.freq( 523 ) #C5
  time.sleep_ms(200) 
  passiveBuzzer.freq( 523 ) #C5
  time.sleep_ms(200) 
  passiveBuzzer.freq( 523 ) #C5
  time.sleep_ms(200) 
  passiveBuzzer.freq( 392 ) #G4
  time.sleep_ms(200) 
  passiveBuzzer.freq( 392 ) #G4
  time.sleep_ms(200) 
  passiveBuzzer.freq( 392 ) #G4
  time.sleep_ms(200) 
  passiveBuzzer.freq( 330 ) #E4
  time.sleep_ms(200) 
  passiveBuzzer.freq( 330 ) #E4
  time.sleep_ms(200) 
  passiveBuzzer.freq( 330 ) #E4
  time.sleep_ms(200) 
  passiveBuzzer.freq( 262 ) #C4
  time.sleep_ms(200) 
  passiveBuzzer.freq( 262 ) #C4
  time.sleep_ms(200) 
  passiveBuzzer.freq( 262 ) #C4
  time.sleep_ms(200) 
  passiveBuzzer.freq( 392 ) #G4
  time.sleep_ms(300) 
  passiveBuzzer.freq( 349 ) #F4
  time.sleep_ms(200) 
  passiveBuzzer.freq( 330 ) #E4
  time.sleep_ms(300) 
  passiveBuzzer.freq( 294 ) #D4
  time.sleep_ms(200) 
  passiveBuzzer.freq( 262 ) #C4
  time.sleep_ms(700)
  
while True:
    if not button1.value():
        led.value(1)
        time.sleep_ms(50)
        led.value(0)
        rowYourBoat()

    elif not button2.value():
        led.value(1)
        maryHadALittleLamb()
        led.value(0)
    else:
        led.value(0)
        passiveBuzzer.deinit()



```

###Video: 
[![](https://markdown-videos-api.jorgenkh.no/youtube/6hqng0tyBZY)](https://youtube.com/shorts/6hqng0tyBZY)


##Project 2:
###Description:

This assignment uses two ESP32 systems (each with a buzzer, an LED, and a button) so that when a button on one of the ESP32s is pressed, the LED will light up and buzzer will go off on the other ESP32.

###List of Hardware Components:
- ESP32 Systems x2
- Resistor 220Ω x4
- Resistor 1kΩ x4
- Push button x2
- Jumper M/M x19
- Active Buzzer x2
- LED x2
- NPN Transistor x2

###Circuit Diagram: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L3P2_CircuitDiagram.png)

###Photo of Breadboard: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L3P2_photo.jpg)

###Breadboard View: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L3P2_BreadboardView.png)

###Source Code:

```
### First Code:

from machine import UART
from machine import Pin
import time

usart_flag=0

uart = UART(2, baudrate=9600, bits=8, parity=0, rx=19, tx=22, timeout=10) 
uart.write(b'1') #tester

button=Pin(4,Pin.IN,Pin.PULL_UP)
led=Pin(14,Pin.OUT)
buzzer=Pin(13,Pin.OUT)

while True:
    ### write
    if not button.value():
        uart.write(b'1')
    else:
        uart.write(b'0')
        
    if uart.any():
        ### read
        
        usart_read=uart.read(1)
        print(usart_read)
        
        if usart_read == b'1':
            led.value(1)
            buzzer.value(1)
        elif usart_read == b'0':
            led.value(0)
            buzzer.value(0)
        


### Second Code:

from machine import UART
from machine import Pin
import time

usart_flag=0

uart = UART(1, baudrate=9600, bits=8, parity=0, rx=22, tx=19, timeout=10) 

uart.write(b'2') #tester

button=Pin(4,Pin.IN,Pin.PULL_UP)
led=Pin(14,Pin.OUT)
buzzer=Pin(13,Pin.OUT)

while True:
    ### write
    if not button.value():
        uart.write(b'1')
    else:
        uart.write(b'0')
        
    if uart.any():
        ### read
        
        usart_read=uart.read(1)
        print(usart_read)
        
        if usart_read == b'1':
            led.value(1)
            buzzer.value(1)
        elif usart_read == b'0':
            led.value(0)
            buzzer.value(0)


```

###Video: 
[![](https://markdown-videos-api.jorgenkh.no/youtube/ymSl7ivVmSk)](https://youtube.com/shorts/ymSl7ivVmSk)