# Lab Assignment #5: Joystick, 74HC595 & LED Bar Graph, 7-Segment Display, and LED Matrix

#### Wednesday, October 18, 2023


##Project 2:
###Description:

This assignment uses the LED Matrix (8x8) and buzzer to play an animation and music respectively. When one of the two buttons is pressed, it will stop the animation and music, and when the next button is pressed, it will resume the animation and music from where it was left off.

###List of Hardware Components:
- Resistor 220Ω x8
- Resistor 1kΩ x1
- Resistor 10kΩ x4
- Jumper M/M x46
- Passive Buzzer x1
- LED Matrix x1
- Push Button x2
- 74HC595 x2
- NPN Transistor x1

###Circuit Diagram: ![alt text](https://github.com/JacobLoewen/Python-Physical-Computing-Photos/blob/main/L5P2_Circuit_Diagram.png?raw=true)

###Photo of Breadboard: ![alt text](https://github.com/JacobLoewen/Python-Physical-Computing-Photos/blob/main/L5P2_photo.jpg?raw=true)

###Breadboard View: ![alt text](https://github.com/JacobLoewen/Python-Physical-Computing-Photos/blob/main/L5P2_BreadboardView.png?raw=true)

###Source Code:

```
import time
from machine import Pin,PWM
from my74HC595 import Chip74HC595

button1 = Pin(14, Pin.IN,Pin.PULL_UP)
button2 = Pin(12, Pin.IN,Pin.PULL_UP)
passiveBuzzer=PWM(Pin(13),2000)

musicdata = [
    659,587,523,587,659,659,587,587,659,784,784,659,587,523,
    587,659,659,659,587,587,659,587,523,523]

numdata = [
    0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, # " "
    0x1C, 0x22, 0x49, 0x51, 0x51, 0x49, 0x22, 0x1C, #'.^.
    0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, # " "
    0x00, 0x00, 0x21, 0x7F, 0x01, 0x00, 0x00, 0x00, # "1"
    0x00, 0x18, 0x18, 0x7E, 0x7E, 0x18, 0x18, 0x00, # "+"
    0x00, 0x00, 0x21, 0x7F, 0x01, 0x00, 0x00, 0x00, # "1"
    0x00, 0x36, 0x36, 0x36, 0x36, 0x36, 0x36, 0x00, # "="
    0x00, 0x00, 0x23, 0x45, 0x49, 0x31, 0x00, 0x00, # "2"
    0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, # " "
    0x00, 0x00, 0x23, 0x45, 0x49, 0x31, 0x00, 0x00, # "2"
    0x00, 0x18, 0x18, 0x7E, 0x7E, 0x18, 0x18, 0x00, # "+"
    0x00, 0x00, 0x23, 0x45, 0x49, 0x31, 0x00, 0x00, # "2"
    0x00, 0x36, 0x36, 0x36, 0x36, 0x36, 0x36, 0x00, # "="
    0x00, 0x00, 0x0E, 0x32, 0x7F, 0x02, 0x00, 0x00, # "4"
    0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00 # " "
]

'''
Pin(15)        -    74HC595(1).ds,
Pin(2)         -    74HC595(1).st_cp,
Pin(4)         -    74HC595(1).sh_cp,
Pin(5)         -    74HC595(1).oe

74HC595(1).q7' -    74HC595(2).ds,
Pin(2)         -    74HC595(2).st_cp,
Pin(4)         -    74HC595(2).sh_cp,
Pin(5)         -    74HC595(2).oe
'''
chip = Chip74HC595(15,2,4,5)
try:
    loops = 0 #counts num of loops
    m = 0 #music counter
    play = 1 #plays if 1, does not play if 0
    while True:

        passiveBuzzer.deinit()
        


                    
        for i in range(112):
            for k in range(5):
                cols=0x01
                    
                for j in range(i,8+i):
                    
                    if not button2.value():
                        play = 0
                        passiveBuzzer.deinit()
                        print("Loop 0")
                        while play == 0:
                            if not button1.value():
                                play = 1
                                passiveBuzzer.init()
                                print("Loop 1")
                    
                    if play == 1:
                        chip.disable()
                        chip.shiftOut(1,numdata[j])
                        chip.shiftOut(0,~cols)
                        cols<<=1
                        chip.enable()
                        time.sleep_us(500)
                        loops += 1
                        
                        if loops == 250:
                            passiveBuzzer.init()
                            print(musicdata[m])
                            passiveBuzzer.freq(int(musicdata[m]))
                            m += 1
                            if m > len(musicdata) - 1:
                                m = 0
                            loops = 0
                                
                        print("Loop 2")
                    
                    
                    
except:
    pass


```

###Video: 
[![](https://markdown-videos-api.jorgenkh.no/youtube/fDB6OyYeyGs)](https://youtube.com/shorts/fDB6OyYeyGs)