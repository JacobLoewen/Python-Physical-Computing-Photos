# Lab Assignment #6: Relay & Motor, Servo, Stepper Motor, LCD 1602


#### Wednesday, October 29, 2023

##Project 1:

###Description:

This assignment uses a stepper motor with a buzzer, photoresistor, and LED. When the photoresistor detects that it is nighttime (when the lights are dimmed or turned off), the stepper motor will rotate clockwise and counter-clockwise with a paper hand on it to represent a hand waving, the buzzer will play a good night song (twinkle twinkle little star), and the LED will blink. When the light turns back on, these functions will halt.

###List of Hardware Components:
- Resistor 10kΩ x3
- Jumper M/M x7
- Jumper F/M x6
- Passive Buzzer x1
- Photoresistor x1
- NPN Transistor x1
- LED (Red) x1
- ULN2003 Stepping Motor Driver x1
- Motor (Stepper) x1

###Circuit Diagram: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L6P1_CircuitDiagram.png)

###Photo of Breadboard: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L6P1_photo.jpg)

###Breadboard View: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L6P1_BreadboardView.png)

###Source Code:

```

from machine import Pin,PWM,ADC
from stepmotor import mystepmotor
import time

myStepMotor=mystepmotor(14,27,26,25)

led=Pin(32,Pin.OUT)
pwm =PWM(Pin(33,Pin.OUT),1000)
passiveBuzzer=PWM(Pin(13),2000)

adc=ADC(Pin(36))
adc.atten(ADC.ATTN_11DB)
adc.width(ADC.WIDTH_10BIT)

m = 0

def readADC() -> int:
    adcValue=adc.read()
    pwm.duty(adcValue)
    print(adc.read())
    time.sleep_ms(100)
    return adc.read()

def playSadSong():

    global m

    print("Play sad song")
    
    musicdata = [261,261,392,392,440,440,392,392,349,349,329,329,294,294,261,261,
                 392,392,349,349,329,329,294,294,392,392,349,349,329,329,294,294,
                 261,261,392,392,440,440,392,392,349,349,329,329,294,294,261,261]

    if m > len(musicdata) - 1:
        m = 0
        
    print(musicdata[m])
    passiveBuzzer.freq(int(musicdata[m]))

    print("m: " + str(m))
      
passiveBuzzer.deinit()
playing = 0


while True:
    adcVal = readADC()
    if adcVal > 500:
        for i in range(2):
            led.value(i%2)
            for j in range(2):
                passiveBuzzer.init()
                myStepMotor.moveSteps(1,2*64,3000)
                print("Test 1")
                playSadSong()
                m += 1
                print("Test 2")
        myStepMotor.stop()
        time.sleep_ms(400)
    
    adcVal = readADC()
    if adcVal > 500:
        for i in range(2):
            led.value(i%2)
            for j in range(2):
                passiveBuzzer.init()
                myStepMotor.moveSteps(0,2*64,3000)
                playSadSong()
                m += 1
        myStepMotor.stop()
        time.sleep_ms(400)
    else:
        passiveBuzzer.deinit()
        m = 0
        led.value(0)

```

###Video: 
[![](https://markdown-videos-api.jorgenkh.no/youtube/YpWaVn3vabQ)](https://youtu.be/YpWaVn3vabQ)

##Project 2:

###Description:

This assignment uses a thermistor to detect the heat in the room. If the room is hot, the DC motor with a piece of paper connected to it will spin to cool the room and the LCD will present a message that was originally "Good temperature" to "Cool yourself", and the fan will stop and the message will go back to "Good temperature" when the room cools down.

###List of Hardware Components:
- Resistor 1kΩ x1
- Resistor 10kΩ x1
- Jumper M/M x12
- Jumper F/M x4
- NPN Transistor x1
- Thermistor x1
- Diode x1
- Delay x1
- Motor (DC) x1
- LCD1602 Module x1

###Circuit Diagram: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L6P2_CircuitDiagram.png)

###Photo of Breadboard: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L6P2_photo.jpg)

###Breadboard View: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L6P2_BreadboardView.png)

###Source Code:

```

from machine import Pin,ADC,I2C
import time
import math
from I2C_LCD import I2cLcd

i2c = I2C(scl=Pin(14), sda=Pin(13), freq=400000)
devices = i2c.scan()
if len(devices) == 0:
    print("No i2c device !")
else:
    for device in devices:
        print("I2C addr: "+hex(device))
        lcd = I2cLcd(i2c, device, 2, 16)

relay = Pin(12, Pin.OUT)        
button = Pin(15, Pin.IN,Pin.PULL_UP)
adc=ADC(Pin(36))
adc.atten(ADC.ATTN_11DB)
adc.width(ADC.WIDTH_12BIT)

def readTemp() -> float:
    adcValue=adc.read()
    voltage=adcValue/4095*3.3
    Rt=10*voltage/(3.3-voltage)
    tempK=(1/(1/(273.15+25)+(math.log(Rt/10))/3950))
    tempC=tempK-273.15
    return tempC

rVal = 0

def rValue(rVal: int):
    if rVal == 0:
        lcd.move_to(0, 0)
        lcd.putstr("Good temperature")
    elif rVal == 1:
        lcd.move_to(0, 0)
        lcd.putstr("Cool yourself   ")
    

#try:
while True:
    print("Test 1")
    tempC = readTemp()
        
    if tempC > 30:
      time.sleep_ms(20)
      if tempC > 30:
          print("Test 2")
          if rVal == 0:
              rValue(1)
              rVal = 1
              relay.value(1)
              print("Test 4")
              
          while tempC > 30:
              print("Test 5")
              tempC = readTemp()
    elif tempC <= 28:
        time.sleep_ms(20)
        if tempC <= 28:
            relay.value(0)
            rValue(0)
            rVal = 0
            while tempC <= 28:
              tempC = readTemp()
              c += 1
              if c >= 50:
                c = 0
                print("Temperature: ",tempC)
                
    
    if c >= 1000:
        c = 0
        print("Temperature: ",tempC)

```

###Video: 
[![](https://markdown-videos-api.jorgenkh.no/youtube/fA-4nbw7GXU)](https://youtube.com/shorts/fA-4nbw7GXU)