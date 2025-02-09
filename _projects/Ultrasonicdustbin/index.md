---
layout: post
title: Ultrasonic Sensor Dust Bin 
description:  I designed and built an automated smart dustbin using an Arduino Uno, an ultrasonic sensor, and a servo motor. Inspired by a similar project, I developed my own version by implementing a standard ultrasonic sensor and servo control code to create a hands-free waste disposal system. The ultrasonic sensor detects when an object, such as a hand, approaches the bin, triggering the servo motor to open the lid automatically. This system enhances hygiene by eliminating the need for physical contact. I worked on wiring the circuit, programming the microcontroller, and calibrating the sensor to ensure smooth and responsive operation. Through this project, I refined my skills in Arduino programming, sensor integration, and mechatronics, demonstrating my ability to design functional and efficient automated systems.
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

---

## Embedding youtube video
The second video has the autoplay on. copy and paste the 11-digit id found in the url link. <br>
*Example* : https://youtube.com/shorts/hOHJp6uZn9w?si=0Xwv-KIK4ElZJVMg
{% include youtube.html id="hOHJp6uZn9w"" autoplay= "false"%}

you can also set up custom size by specifying the width (the aspect ratio has been set to 16/9). The default size is 560 pixels x 315 pixels.  

The width of the video below. Regardless of initial width, all the videos is responsive and will fit within the smaller screen.
{% include youtube.html id="hOHJp6uZn9w" autoplay = "false" width= "900px" %}  

<br>

## Adding a hozontal line
---

## Starting a new line
leave two spaces "  " at the end or enter <br>

## Adding bold text
this is how you input **bold text**

## Adding italic text
Italicized text is the *cat's meow*.

## Adding ordered list
1. First item
2. Second item
3. Third item
4. Fourth item

## Adding unordered list
- First item
- Second item
- Third item
- Fourth item

## Adding code block

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
