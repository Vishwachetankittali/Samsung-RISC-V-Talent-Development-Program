# Ultrasonic Obstacle Detection Circuit using VSDSquadron Mini

## **Overview**  
The **Ultrasonic Blind Stick** project integrates an **ultrasonic sensor** with the **VSD_Squadron-MINI-BOARD** to assist visually impaired individuals in detecting obstacles. The **HC-SR04 ultrasonic sensor** continuously measures distance and sends signals to the **VSD_Squadron-MINI-BOARD**. When an obstacle is detected within a **50 cm** range, an **active buzzer** is triggered to alert the user.  

## **Components Required**  
- **VSD_Squadron-MINI-BOARD**  
- **Ultrasonic Sensor (HC-SR04)**  
- **Active Buzzer**  
- **Battery**  
- **Breadboard**  
- **Jumper Wires**  


## **Circuit Connection**  

The **HC-SR04 ultrasonic sensor** is connected to the **VSD_Squadron-MINI-BOARD** as follows:  
- **VCC (Power) → 5V pin** on the VSD_Squadron-MINI-BOARD  
- **GND (Ground) → GND pin** on the board  
- **Trigger Pin → GPIO D3** (Sends ultrasonic pulse)  
- **Echo Pin → GPIO D2** (Receives reflected signal)  

The **active buzzer** is connected to the **VSD_Squadron-MINI-BOARD** as follows:  
- **VCC → 3.3V or 5V pin** on the board  
- **GND → GND pin**  
- **Signal Pin → GPIO D4** (Used to activate the buzzer)  



## 📌 Pin Mapping with Resistor Placements

| **Component**         | **VSD_Squadron-MINI-BOARD Pin** | **Other Connections**  |
|----------------------|-------------------------------|------------------------|
| **🔹 HC-SR04 (Ultrasonic Sensor)** | | |
| **VCC**             | **5V**                         | Directly connected to power |
| **GND**             | **GND**                        | Directly connected to ground |
| **Trigger (TRIG)**  | **D3**                         | Directly connected to Trigger pin |
| **Echo (ECHO)**     | **D2**                         | **Connected via 10kΩ pull-down resistor** to **GND** |
| **🔹 Active Buzzer** | | |
| **VCC (+)**         | **3.3V / 5V**                  | Directly connected to power |
| **GND (-)**         | **GND**                        | Directly connected to ground |
| **Signal (Control Pin)** | **D4**                    | **Connected via 470Ω resistor to the Buzzer (+ terminal)** |

---

## 📌 Explanation of Resistor Placements


### **1️⃣ 10kΩ Pull-Down Resistor (For HC-SR04 Echo Pin)**
- **Why?** Prevents floating values when the sensor isn't active.
- **Where?**  
  - One end of **10kΩ resistor** → Connect to **ECHO pin (D2)**
  - Other end → Connect to **GND**  


### **2️⃣ 470Ω Current-Limiting Resistor (For Buzzer)**
- **Why?** Protects the GPIO pin by limiting current.
- **Where?**  
  - One end of **470Ω resistor** → Connect to **D4 (Signal pin for buzzer)**
  - Other end → Connect to **Buzzer (+ terminal)**
  - **Buzzer (- terminal)** → Connect directly to **GND**

---

## 📌 Circuit Breakdown
1️⃣ **Ultrasonic sensor (HC-SR04) measures distance and sends data via Echo (D2).**  
2️⃣ **Buzzer (D4) gets activated when an obstacle is detected within range.**  
3️⃣ **Resistors prevent excessive current and floating signals.**  

---

## **Program**  
The following **Arduino/C code** measures the distance using the **HC-SR04 ultrasonic sensor** and triggers the **active buzzer** when an obstacle is detected within **50 cm**.  

### **Code Implementation**
```cpp
#define TRIG_PIN 3  // Ultrasonic sensor Trigger pin connected to D3
#define ECHO_PIN 2  // Ultrasonic sensor Echo pin connected to D2
#define BUZZER_PIN 4 // Active Buzzer connected to D4

void setup() {
    pinMode(TRIG_PIN, OUTPUT);
    pinMode(ECHO_PIN, INPUT);
    pinMode(BUZZER_PIN, OUTPUT);
    Serial.begin(9600);
}

void loop() {
    long duration;
    int distance;

    // Send a 10-microsecond pulse to trigger the ultrasonic sensor
    digitalWrite(TRIG_PIN, LOW);
    delayMicroseconds(2);
    digitalWrite(TRIG_PIN, HIGH);
    delayMicroseconds(10);
    digitalWrite(TRIG_PIN, LOW);

    // Read the echo response time
    duration = pulseIn(ECHO_PIN, HIGH);

    // Convert the time into distance (in cm)
    distance = duration * 0.034 / 2;

    Serial.print("Distance: ");
    Serial.print(distance);
    Serial.println(" cm");

    // Check distance and activate buzzer accordingly
    if (distance > 0 && distance <= 50) {
        digitalWrite(BUZZER_PIN, HIGH); // Turn on buzzer if obstacle is within 50 cm
    } else {
        digitalWrite(BUZZER_PIN, LOW); // Turn off buzzer otherwise
    }

    delay(100); // Small delay to avoid excessive processing
}
```
---

## **Working Principle**  
- The **HC-SR04 ultrasonic sensor** continuously sends **ultrasonic pulses** and measures the **time taken** for the echo to return.  
- The **VSD_Squadron-MINI-BOARD** processes the time data and calculates the **distance** of obstacles.  
- If an object is detected within **50 cm**, the **active buzzer** is activated to alert the user.  
- If the obstacle is **farther than 50 cm**, the **buzzer remains off**.  
- The system **continuously updates** the obstacle distance in real time.  
- This project provides a **cost-effective, real-time obstacle detection solution** for visually impaired users, enhancing their **mobility and safety**.  

---

## **Future Enhancements**  
- **Multiple Distance Zones**: Implement different buzzer patterns for varying distances.  
- **Voice Alerts**: Use a **text-to-speech (TTS) module** to provide audio feedback.  
- **Bluetooth/Wi-Fi Integration**: Connect the system to a **mobile app** for additional features.  
- **Battery Optimization**: Improve **power consumption** for longer operation.  

---

## **Contributing**  
If you want to **improve this project**, feel free to:  
1. **Fork** the repository.  
2. **Make necessary changes** or enhancements.  
3. **Submit a Pull Request (PR)** for review.  

Your contributions are **highly appreciated!** 🚀  

---

## Circuit Diagram
![Alt Text](Snapshots_(Ultrasonic_Obstacle_Detection_Circuit)/Ultrasonic_Obstacle_Detection_Circuit.PNG)


---


---
# Binary to Gray Code Converter (3-bit) using VSDSquadron Mini  

## Overview  
This project implements a **3-bit Binary to Gray Code Converter** using the **VSDSquadron Mini (RISC-V-based SoC)**. The goal is to take a **3-bit binary number** from user input (via push buttons) and convert it to **Gray code** using XOR logic.  

Gray code is widely used in digital systems to **reduce errors in data transmission and signal processing**. This project demonstrates **how a RISC-V processor** can be programmed for digital logic applications while utilizing GPIO operations for input and output handling.  

---

## Components Required  
| **Component**          | **Quantity** | **Purpose**                                      |
|------------------------|-------------|--------------------------------------------------|
| VSDSquadron Mini Board | 1           | Microcontroller for processing the conversion.  |
| LEDs                  | 3           | Display Gray code output (G2, G1, G0).          |
| Push Buttons          | 3           | Input binary values (B2, B1, B0).               |
| Resistors (10kΩ)      | 3           | Pull-down resistors for push buttons.           |
| Resistors (330Ω)      | 3           | Current-limiting resistors for LEDs.            |
| Breadboard            | 1           | For mounting and wiring components.             |
| Jumper Wires          | ~15-20      | For circuit connections.                        |
| USB-C Cable           | 1           | To power the VSDSquadron Mini board.            |

---

## Circuit Connections  
### 1. LED Connections  
| **LED Output**      | **VSDSquadron GPIO Pin** |
|---------------------|-------------------------|
| LED1 (Gray Code Bit 2) | GPIO PC0             |
| LED2 (Gray Code Bit 1) | GPIO PC2             |
| LED3 (Gray Code Bit 0) | GPIO PC3             |

### 2. Push Button Connections  
| **Push Button Input** | **VSDSquadron GPIO Pin** |
|-----------------------|-------------------------|
| Button1 (Binary Bit 2) | GPIO PD4             |
| Button2 (Binary Bit 1) | GPIO PD5             |
| Button3 (Binary Bit 0) | GPIO PD6             |

---

## Working Principle  
1. **User Inputs Binary Number**:  
   - Pressing the **push buttons** sets the binary input (B2, B1, B0).  
2. **Microcontroller Converts to Gray Code**:  
   - Uses XOR logic to compute the **Gray code (G2, G1, G0)**.  
3. **Gray Code is Displayed on LEDs**:  
   - LEDs light up corresponding to the **Gray code output**.  

### Gray Code Conversion Logic  
The **3-bit binary (B2 B1 B0)** is converted into **Gray code (G2 G1 G0)** using:  

- **G2** = B2  
- **G1** = B2 ⊕ B1  
- **G0** = B1 ⊕ B0  

---

## Truth Table for Binary to Gray Code Conversion  

| Binary (B2 B1 B0) | Gray Code (G2 G1 G0) |
|-------------------|---------------------|
| 000               | 000                 |
| 001               | 001                 |
| 010               | 011                 |
| 011               | 010                 |
| 100               | 110                 |
| 101               | 111                 |
| 110               | 101                 |
| 111               | 100                 |
---

# Circuit Connection
![Alt Text](binary_to_grey_circuit_connection.png)
