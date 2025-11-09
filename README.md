# AUTOMATED-WASTE-SEGREGATION
A short article on waste control and management systems
# Automated Waste Segregation ♻️

### Mini Project Report — VTU 5th Semester (2024–2025)
**Department of Electronics & Communication Engineering**  
**Bheemanna Khandre Institute of Technology, Bhalki**

---

### 👩‍🔧 Team Members
- Shubham (3RB22CE070)
- varun  (3RB22EC088)

---

## 📖 Project Abstract
This project presents a **smart automated waste segregation system** that sorts waste into three categories — **Wet, Dry, and Metallic**. Using sensors such as **IR, Proximity (Metal Detector), and Moisture Sensors**, the system detects the type of waste and directs it into the appropriate bin with the help of **Servo and Stepper motors** controlled by **Arduino Uno**.

The goal is to minimize manual segregation, promote recycling, and improve environmental hygiene through automation.

---

## ⚙️ Hardware Components
- Arduino UNO  
- IR Sensor  
- Proximity Sensor (Metal detection)  
- Moisture Sensor  
- Servo Motor  
- Stepper Motor (ULN2003 Driver)  
- Buzzer, Jumper Wires, Power Supply, Frame & Collecting Bins  

---

## 🔌 Working Principle
1. Waste is dropped into the input pipe.  
2. IR sensor detects the presence of waste.  
3. Proximity sensor identifies metallic waste.  
4. Moisture sensor differentiates wet and dry waste.  
5. Arduino controls servo and stepper motors to drop waste into the respective bin.  
6. A buzzer gives sound indication during operation.

---

## 💻 Arduino Code
The complete code is available in the [`CODE/aws_arduino_code.ino`](CODE/aws_arduino_code.ino) file.

```cpp
#include <CheapStepper.h>
#include <Servo.h>

Servo servo1;
#define ir 5
#define proxi 6
#define buzzer 12
int potPin = A0;
int soil = 0;
int fsoil;
CheapStepper stepper(8, 9, 10, 11);

void setup() {
  Serial.begin(9600);
  pinMode(proxi, INPUT_PULLUP);
  pinMode(ir, INPUT);
  pinMode(buzzer, OUTPUT);
  servo1.attach(7);
  stepper.setRpm(17);
  servo1.write(180);
  delay(1000);
  servo1.write(70);
  delay(1000);
}

void loop() {
  fsoil = 0;
  int L = digitalRead(proxi);
  if (L == 0) {
    tone(buzzer, 1000, 1000);
    stepper.moveDegreesCW(240);
    delay(1000);
    servo1.write(180);
    delay(1000);
    servo1.write(70);
    delay(1000);
    stepper.moveDegreesCCW(240);
    delay(1000);
  }

  if (digitalRead(ir) == 0) {
    tone(buzzer, 1000, 500);
    delay(1000);
    int soil = 0;
    for (int i = 0; i < 3; i++) {
      soil = analogRead(potPin);
      soil = constrain(soil, 485, 1023);
      fsoil = (map(soil, 485, 1023, 100, 0)) + fsoil;
      delay(75);
    }
    fsoil = fsoil / 3;

    if (fsoil > 20) {
      stepper.moveDegreesCW(120);
      delay(1000);
      servo1.write(180);
      delay(1000);
      servo1.write(70);
      delay(1000);
      stepper.moveDegreesCCW(120);
      delay(1000);
    } else {
      tone(buzzer, 1000, 500);
      delay(1000);
      servo1.write(180);
      delay(1000);
      servo1.write(70);
      delay(1000);
    }
  }
}

---


