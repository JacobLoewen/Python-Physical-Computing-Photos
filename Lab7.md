# Lab Assignment #7: Ultrasonic Ranging, Matrix Keypad, Infrared Remote, and Hygrothermograph DHT11


#### Monday, November 6, 2023

## Project 1:

### Description:

This assignment uses an ultrasonic ranging module (HC-SR04), a buzzer, and an LED bar. When the ultrasonic ranging module detects an object as close, it will play a lower note and show less LEDs on the LED bar. When it detects an object as far, it will then play a higher note and show more LEDs on the LED bar.

### List of Hardware Components:
- Resistor 220Ω x10
- Resistor 1kΩ x1
- Jumper M/M x18
- Jumper F/M x4
- LED bar x1
- Passive Buzzer x1
- NPN Transistor x1
- Ultrasonic Ranging Module (HC-SR04)

### Circuit Diagram: 
![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L7P1_Circuit_Diagram.png)

### Photo of Breadboard: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L7P1_photo.jpg)

### Breadboard View: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L7P1_Breadboard_View.png)

### Source Code:

```

from machine import Pin,PWM
import time

trigPin=Pin(13,Pin.OUT,0)
echoPin=Pin(14,Pin.IN,0)
passiveBuzzer=PWM(Pin(12),2000)

pins=[15,2,0,4,5,18,19,21,22,23]

musicNotes = [493,523,587,659,698,783,880,987,1046,1174]

soundVelocity=340
distance=0

passiveBuzzer.deinit()

def getSonar():
    trigPin.value(1)
    time.sleep_us(10)
    trigPin.value(0)
    while not echoPin.value():
        pass
    pingStart=time.ticks_us()
    while echoPin.value():
        pass
    pingStop=time.ticks_us()
    pingTime=time.ticks_diff(pingStop,pingStart)
    distance=pingTime*soundVelocity//2//10000
    return int(distance)

time.sleep_ms(2000)

prev = -1
led=Pin(pins[0],Pin.OUT)

while True:
    time.sleep_ms(100)
    dist = getSonar()
    print('Distance: ',dist,'cm' )
    
    i = int(dist / 5)
    
    if i != prev:
        if i >= 10:
            i = 9
        else:
            for j in range(i, 10):
                led=Pin(pins[j],Pin.OUT)
                led.value(0)
        
        print("Music Notes: " + str(musicNotes[i]))
        passiveBuzzer.init()
        passiveBuzzer.freq(musicNotes[i])
        
        for j in range(0, i+1):
            led=Pin(pins[j],Pin.OUT)
            led.value(1)
        

```

### Video: 
[![](https://markdown-videos-api.jorgenkh.no/youtube/-JNq4csNfA8)](https://youtube.com/shorts/-JNq4csNfA8)

## Project 2:

### Description:

This assignment uses a buzzer, an LCD1602, a servo motor, a neopixel, and an infrared remote. When one of the buttons 1-4 is pressed, it will activate one of the components listed above respectfully.

### List of Hardware Components:
- Resistor 1kΩ x1
- Resistor 10kΩ x1
- Jumper M/M x11
- Jumper F/M x7
- NPN Transistor x1
- Servo Motor x1
- LCD1602 Module x1
- Active Buzzer x1
- Neopixel x1

### Circuit Diagram: 
![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L7P2_Circuit_Diagram.png)

### Photo of Breadboard: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L7P2_photo.jpg)

### Breadboard View: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L7P2_Breadboard_View.png)

### Source Code:

```

from machine import I2C,Pin,PWM
import time
import neopixel
from irrecvdata import irGetCMD
from I2C_LCD import I2cLcd
from myservo import myServo

pin = Pin(2, Pin.OUT)
np = neopixel.NeoPixel(pin, 8)

servo=myServo(4)#set servo pin
servo.myServoWriteAngle(0)#Set Servo Angle

i2c = I2C(scl=Pin(14), sda=Pin(13), freq=400000)
devices = i2c.scan()

if len(devices) == 0:
     print("No i2c device !")

else:
     for device in devices:
         print("I2C addr: "+hex(device))
         lcd = I2cLcd(i2c, device, 2, 16)

passiveBuzzer=Pin(12,Pin.OUT)
recvPin = irGetCMD(15)

def buttonValues(value):
        
    if value == '0xff30cf': #1 (Buzzer)
        print('1')
        passiveBuzzer.init()
        passiveBuzzer.value(1)
        time.sleep_ms(500)
        passiveBuzzer.value(0)
        
    elif value == '0xff18e7': #2 (LCD1602)
        print('2')
        lcd.move_to(0, 0)
        lcd.putstr("2")
        time.sleep_ms(2000)
        lcd.move_to(0, 0)
        lcd.putstr(" ")

    elif value == '0xff7a85': #3 (Servo Motor)
        print('3')
        for i in range(0,180,1):
            servo.myServoWriteAngle(i)
            time.sleep_ms(15)
        for i in range(180,0,-1):
            servo.myServoWriteAngle(i)
            time.sleep_ms(15)
            
    elif value == '0xff10ef': #4 (Neopixel)
        print('4')
        
        np[0] = [10,0,0]
        np[2] = [0,10,0]
        np[4] = [10,0,0]
        np[6] = [0,10,0]
        
        np.write()
        
        time.sleep_ms(2000)
        for i in range(8):
            np[i] = [0,0,0]
        np.write()

    else:
        return

while True:
    irValue = recvPin.ir_read()
    if irValue:
        print(irValue)
        buttonValues(irValue)



```

### Video: 
[![](https://markdown-videos-api.jorgenkh.no/youtube/-JNq4csNfA8)](https://youtube.com/shorts/-JNq4csNfA8)
