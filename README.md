# Automatic Water Level Controller

##  AIM:
To monitor and control the water level of the tank using Arduino UNO controller.

## Software required:
Arduino IDE </br>
Proteous

## PROCEDURE:
### Arduino IDE
Step1:Open the Arduino IDE </br>
Step2: Go to file and select new file option </br>
Step3:Type the program </br>
Step4:Go to file and select save option to save the program </br>
Step5:Go to sketch and select verify or compile options </br>
Step6:If no error Hex file will be generated in the temporary folder </br>
### Proteus
Step7:Open the Proteus software </br>
Step8:Go to file select new design and click ok button </br>
Step9:Select component mode and click pick devices from the library </br>
Step10:Type the component name in the keyword to select the components and click ok button </br>
Step11:Design the circuit as per the diagram </br>
Step12:Double click the Arduino controller and upload the hex file generated by Arduino IDE </br>
Step13:Click start button and check the output

## THEORY:

We are going to control the water level by using ultrasonic sensor. Basic principle of ultrasonic distance measurement is based on ECHO. When sound waves are transmitted in environment, they return back to the origin as ECHO after striking on any obstacle. So, we can calculate its traveling time to the target and back to source. After some calculations we can come to a result in the form of distance. This concept is used in our water controller project where the water motor pump is automatically turned on when water level in the tank becomes low

### Hardware Requirements 

1. Arduino Uno 2. Ultrasonic Sensor Module (HC-SRO4) 3. 16x2 LCD 4. A 5V Relay, 5. ULN2003 or a NPN transistor (2N2222) 6. Water pump

![image](https://user-images.githubusercontent.com/71547910/235332412-e276fbff-58de-4684-94aa-8c753492c0b2.png)

In the circuit Ultrasonic sensor module’s “trigger” and “echo” pins are directly connected to pin 7 and 6 of Arduino. A 16x2 LCD is connected with Arduino in 4- bit mode. Control pin RS, RW and En are directly connected to Arduino pin 13, GND and 12. And data pin D4-D7 is connected to 11, 10, 9 and 8 of Arduino, and buzzer is connected at pin 4. A 6 Volt relay is also connected at pin 2 of Arduino through npn transistor (2N2222) and generic diode for turning on or turning off the water motor pump. A voltage regulator is also used for providing 5 volts to relay and to remaining circuit.Then, we added Ultrasonic Sensor Library for Proteus, in this library, we have used an extra pin on the ultrasonic sensor, which is an analog pin. The voltage on that pin is used to detect how close the object is.

We designed Arduino sketch for Ultrasonic Sensor, so we placed the code into the Arduino software. we added Relay in which when we run the simulation, the relay will automatically get activated and after that will control the relay using a logic, i.e. when we provide +5V to it then the relay will go activated and when we give GND then it will deenergize. We also added NPN transistor before the relay to amplifier the current. So, when the logic state is zero means ground, the transistor won’t work and the supply can't reach to the relay and when we make the logic 1 means +5V on the base of transistor, then the relay circuit will complete and the relay will be energized

In the circuit Ultrasonic sensor will get the distance. Then it will show the level on LCD screen with message “level xx cm”. We are here measuring empty place of distance for water instead of water level. Because of this functionality we can use this system in any water tank. We substract the distance from the tank depth to get the water level. When the water level reaches at 15 cm then Arduino turns ON the water pump by driving relay. And now LCD will show “level 15 cm” “pump on”, Relay status LED will start glowing and buzzer also beep for some time.Now if the level reaches at about 28 cm Arduino turns OFF the relay and LCD will show “level 28 cm” “pump off”. and relay status LED will turn OFF.

### Circuit Diagram and Explanation

As shown in the water level controller circuit given below, Ultrasonic sensor module’s “trigger” and “echo” pins are directly connected to pin 10 and 11 of arduino. A 16x2 LCD is connected with arduino in 4-bit mode. Control pin RS, RW and En are directly connected to arduino pin 7, GND and 6. And data pin D4-D7 is connected to 5, 4, 3 and 2 of arduino, and buzzer is connected at pin 12. 6 Volt relay is also connected at pin 8 of arduino through ULN2003 for turning on or turning off the water motor pump. A voltage regulator 7805 is also used for providing 5 volt to relay and to remaining circuit.

![image](https://user-images.githubusercontent.com/71547910/235332565-e4933960-e14f-4c34-8c21-240727a93f9c.png)

In this circuit Ultrasonic sensor module is placed at the top of bucket (water tank) for demonstration. This sensor module will read the distance between sensor module and water surface, and it will show the distance on LCD screen with message “Water Space in Tank is:”. It means we are here showing empty place of distance or volume for water instead of water level. Because of this functionality we can use this system in any water tank. When empty water level reaches at distance about 30 cm then Arduino turns ON the water pump by driving relay. And now LCD will show “LOW Water Level” “Motor turned ON”, and Relay status LED will start glowing.Now if the empty space reaches at distance about 12 cm arduino turns OFF the relay and LCD will show “Tank is full” “Motor Turned OFF”. Buzzer also beep for some time and relay status LED will turned OFF



## PROGRAM:
```
#include <LiquidCrystal.h>
#define trigger 10
#define echo 11
#define motor 8
#define buzzer 12
#define led1 9
#define led2 13
#define led3 1
LiquidCrystal lcd(7,6,5,4,3,2);
float time=0,distance=0;
int temp=0;
void setup()
{
lcd.begin(16,2);
pinMode(trigger,OUTPUT);
pinMode(echo,INPUT);
pinMode(motor, OUTPUT);
pinMode(buzzer, OUTPUT);
pinMode(led1, OUTPUT);

pinMode(led2, OUTPUT);
pinMode(led3, OUTPUT);
}
void loop()
{
lcd.clear();
digitalWrite(trigger,LOW);
delayMicroseconds(2);
digitalWrite(trigger,HIGH);
delayMicroseconds(10);
digitalWrite(trigger,LOW);
delayMicroseconds(2);
time=pulseIn(echo,HIGH);
distance=time*340/20000;
lcd.clear();
lcd.print("AWL CONTROL");
delay(2000);
if(distance<500)
{
digitalWrite(motor, LOW);
digitalWrite(led1, HIGH);
lcd.clear();
lcd.print("Water Tank Full ");
lcd.setCursor(0,1);
lcd.print("Motor Turned OFF");
delay(5000);
digitalWrite(led1, LOW);
}
else if(distance>500 && distance<750)
{
digitalWrite(motor, LOW);
digitalWrite(led2, HIGH);
lcd.clear();
lcd.print("Water Tank Full ");
lcd.setCursor(0,1);
lcd.print("Motor Turned OFF");
delay(5000);
digitalWrite(led2, LOW);
}
else if(distance>750)
{
digitalWrite(motor, HIGH);
digitalWrite(buzzer, HIGH);
digitalWrite(led3, HIGH);
lcd.clear();
lcd.print("LOW Water Level");
lcd.setCursor(0,1);
lcd.print("Motor Turned ON");
delay(5000);
digitalWrite(buzzer, LOW);
digitalWrite(led3, LOW);
}
}
```
## CIRCUIT DIAGRAM:
![image](https://github.com/VarshaAjith1110/Automatic-water-level-controller/assets/94222288/01984307-4a62-4fad-a0ae-0f59ddf73633)

## OUTPUT:
![image](https://github.com/VarshaAjith1110/Automatic-water-level-controller/assets/94222288/0918bcff-5a1b-47ca-a43e-ed7c78bca004)


## RESULT:
Thus the water level of the tank was monitored and controlled using Arduino UNO controller.

