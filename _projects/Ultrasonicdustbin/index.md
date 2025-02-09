---
layout: post
title: Ultrasonic Sensor Dustbin
description: I designed and built an automated smart dustbin using an Arduino Uno, an ultrasonic sensor, and a servo motor. Inspired by a similar project, I developed my own version by implementing a standard ultrasonic sensor and servo control code to create a hands-free waste disposal system. The ultrasonic sensor detects when an object, such as a hand, approaches the bin, triggering the servo motor to open the lid automatically. This system enhances hygiene by eliminating the need for physical contact. I worked on wiring the circuit, programming the microcontroller, and calibrating the sensor to ensure smooth and responsive operation. Through this project, I refined my skills in Arduino programming, sensor integration, and mechatronics, demonstrating my ability to design functional and efficient automated systems.
skills:
  - Onshape
  - CAD
  - Arduino/Arduino IDE
  - C++
  - Engineering Drawings
  - Sensor Integration
  - Mechatronics
  - Embedded Systems

main-image: /dustbin.jpg
---
<img src="/dustbin.jpg" alt="Description of Image" width="500">
---
## Video 
Initial actuation after assembly and programming of electronic components 
*Example* : https://youtube.com/shorts/hOHJp6uZn9w?si=0Xwv-KIK4ElZJVMg  

{% include youtube.html id="hOHJp6uZn9w" autoplay="false" %}


<br>

---

## Items Used 
1. CAD Drawings 
2. Arduino Uno R3
3. SG 90 Tower Pro Micro Servo Motor
4. HC-SR04 Ultrasonic Module
5. Jumper Wires Ribbon Cables
6. Breadboard


## Code Used 
```cpp
#include <Servo.h>

// Define sensor and servo pins
const int trigPin = 9;  // Ultrasonic sensor trigger pin
const int echoPin = 10; // Ultrasonic sensor echo pin
const int servoPin = 6; // Servo motor control pin

Servo myServo; // Create servo object

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  myServo.attach(servoPin);
  myServo.write(0); // Initial servo position
  Serial.begin(9600);
}

void loop() {
  long duration;
  int distance;

  // Send ultrasonic pulse
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // Read echo response
  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.034 / 2; // Convert to cm

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  // If object is detected within 15 cm, activate servo
  if (distance > 0 && distance < 15) {
    myServo.write(90); // Rotate servo to 90 degrees
    delay(2000);       // Hold position for 2 seconds
    myServo.write(0);  // Return servo to initial position
  }

  delay(500); // Short delay before next measurement
}
