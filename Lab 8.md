# Lab Assignment #8: Infrared Motion Sensor, Attitude Sensor MPU6050, Bluetooth, and Wi-Fi


#### Monday, November 15, 2023

## Project 1:

### Description:

This assignment uses the HC-SR501 PIR Sensor to detect when a living entity is close by. When the sensor detects someone close by, the RGB LED will rapidly flash different colours, the Passive Buzzer will create a siren noise, and the Stepping Motor will wave a paper hand very quickly.

### List of Hardware Components:
- Resistor 220Ω x3
- Resistor 1kΩ x1
- Jumper M/M x8
- Jumper F/M x9
- RGB Led x1
- Passive Buzzer x1
- NPN Transistor x1
- Motion Sensor (HC-SR501 PIR Sensor) x1
- Stepper Motor x1
- ULN 2003 Stepping motorDriver x1

### Circuit Diagram: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L8P1_Circuit_Diagram.png)

### Photo of Breadboard: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L8P1_photo.jpg)

### Breadboard View: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L8P1_Breadboard_View.png)

### Source Code:

```

from machine import Pin,PWM
from random import randint
from stepmotor import mystepmotor
import time
import math

PI=3.14

sensorPin=Pin(12,Pin.IN)

passiveBuzzer=PWM(Pin(13),2000)

pins=[15,2,0]

passiveBuzzer.deinit()

myStepMotor=mystepmotor(14,27,26,25)

pwm0=PWM(Pin(pins[0]),10000)
pwm1=PWM(Pin(pins[1]),10000)
pwm2=PWM(Pin(pins[2]),10000)

def setColor(r,g,b):
    pwm0.duty(1023-r)
    pwm1.duty(1023-g)
    pwm2.duty(1023-b)


def alarm():
    i = 0
    while True:
        for x in range(0,36):
            i += 1
            sinVal=math.sin(x*10*PI/180)
            toneVal=2000+int(sinVal*500)
            passiveBuzzer.freq(toneVal)
            if i < 200:
                myStepMotor.moveSteps(1,1,2000)
                myStepMotor.stop()
            elif i >= 400:
                i = 0
            elif i >= 200:
                myStepMotor.moveSteps(0,1,2000)
                myStepMotor.stop()
            
        red   = randint(0,1023)
        green = randint(0,1023)
        blue  = randint(0,1023)
        setColor(red,green,blue)
        

setColor(0,0,0)

while True:
  if not sensorPin.value():
      passiveBuzzer.init()
      alarm()
    
      print("Alarm!")


        

```

### Video: 
[![](https://markdown-videos-api.jorgenkh.no/youtube/JmT3pbnGXCM)](https://youtube.com/shorts/JmT3pbnGXCM)

## Project 2:

### Description:

This assignment uses an MPU6050, an LCD1602, and a buzzer. When the ESP32 is tilted in any four directions (forward, backward, left, right), the MPU6050 will detect the direction and a message will appear on the LCD1602 to which direction. The purpose of this is to help an elderly person to detect their balance and mobility and help them see which way they fell if they were to fall down.

### List of Hardware Components:
- Resistor 1kΩ x1
- Jumper M/M x8
- Jumper F/M x4
- NPN Transistor x1
- LCD1602 Module x1
- Active Buzzer x1
- MPU6050
- LCD1602

### Circuit Diagram: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L8P2_Circuit_Diagram.png)

### Photo of Breadboard: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L8P2_photo.jpg)

### Breadboard View: ![alt text](https://raw.githubusercontent.com/JacobLoewen/Python-Physical-Computing-Photos/main/L8P2_Breadboard_View.png)

### Source Code:

```

from mpu6050 import MPU6050
import time
from machine import I2C, Pin, PWM
from I2C_LCD import I2cLcd
 
mpu=MPU6050(12,25)
mpu.MPU_Init()     
G = 9.8
time.sleep_ms(1000)

i2c = I2C(scl=Pin(14), sda=Pin(13), freq=400000)
devices = i2c.scan()

if len(devices) == 0:
     print("No i2c device !")

else:
     for device in devices:
         print("I2C addr: "+hex(device))
         lcd = I2cLcd(i2c, device, 2, 16)
         
passiveBuzzer=PWM(Pin(26),2000)

while True:
    accel=mpu.MPU_Get_Accelerometer()#gain the values of Acceleration
    gyro=mpu.MPU_Get_Gyroscope()     #gain the values of Gyroscope
    print("\na/g:\t")
    print(accel[0],"\t",accel[1],"\t",accel[2],"\t",
          gyro[0],"\t",gyro[1],"\t",gyro[2])
    print("a/g:\t")
    print(accel[0]/16384,"g",accel[1]/16384,"g",accel[2]/16384,"g",
          gyro[0]/131,"d/s",gyro[1]/131,"d/s",gyro[2]/131,"d/s")
    time.sleep_ms(1000)
    
    if accel[0] > 10000:
        print("You fell to the Left")
        lcd.move_to(0, 0)
        lcd.putstr("You fell to the   ")
        lcd.move_to(0, 1)
        lcd.putstr("Left            ")
        passiveBuzzer.init()
        passiveBuzzer.freq(523)
        
        
    elif accel[1] > 10000:
        print("You fell Backwards")
        lcd.move_to(0, 0)
        lcd.putstr("You fell        ")
        lcd.move_to(0, 1)
        lcd.putstr("Backwards       ")
        passiveBuzzer.init()
        passiveBuzzer.freq(587)
        
    elif accel[1] < -10000:
        print("You fell Forwards")
        lcd.move_to(0, 0)
        lcd.putstr("You fell        ")
        lcd.move_to(0, 1)
        lcd.putstr("Forwards        ")
        passiveBuzzer.init()
        passiveBuzzer.freq(659)
        
    elif accel[0] < -10000:
        print("You fell to the Right")
        lcd.move_to(0, 0)
        lcd.putstr("You fell to the     ")
        lcd.move_to(0, 1)
        lcd.putstr("Right           ")
        passiveBuzzer.init()
        passiveBuzzer.freq(698)
    
    else:
        passiveBuzzer.deinit()


```

### Video: 
[![](https://markdown-videos-api.jorgenkh.no/youtube/RwBcQ7aVfH8)](https://youtube.com/shorts/RwBcQ7aVfH8)