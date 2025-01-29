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
# Code 
    #include <stdio.h>
    #include <ch32v00x.h>

    void GPIO_Config(void) {
       GPIO_InitTypeDef GPIO_InitStructure = {0};
       RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD, ENABLE);
       RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC, ENABLE);


       GPIO_InitStructure.GPIO_Pin = GPIO_Pin_4 | GPIO_Pin_5 | GPIO_Pin_6;
       GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;
       GPIO_Init(GPIOD, &GPIO_InitStructure);

    
      GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0 | GPIO_Pin_2 | GPIO_Pin_3;
      GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
      GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
      GPIO_Init(GPIOC, &GPIO_InitStructure);
    }

    int xor(int bit1, int bit2) {
      return bit1 ^ bit2;
    }

    int main(void) {
       uint8_t B, G[3];
       GPIO_Config();

        while (1) {
            B = GPIO_ReadInputDataBit(GPIOD, GPIO_Pin_4) << 2 | 
            GPIO_ReadInputDataBit(GPIOD, GPIO_Pin_5) << 1 | 
            GPIO_ReadInputDataBit(GPIOD, GPIO_Pin_6);

            G[2] = (B >> 2) & 1;
            G[1] = xor((B >> 2) & 1, (B >> 1) & 1);
            G[0] = xor((B >> 1) & 1, B & 1);

            GPIO_WriteBit(GPIOC, GPIO_Pin_0, G[2] ? RESET : SET);
            GPIO_WriteBit(GPIOC, GPIO_Pin_2, G[1] ? RESET : SET);
            GPIO_WriteBit(GPIOC, GPIO_Pin_3, G[0] ? RESET : SET);
       }
    }
---
# Circuit Connection
![Alt Text](Snapshot/bg circuit connection.png)




