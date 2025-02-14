# **Ultrasonic Obstacle Detection Circuit using VSDSquadron Mini** 
## **Program**

The following **Arduino/C code** measures the distance using the **HC-SR04 ultrasonic sensor** and triggers the **active buzzer** when an obstacle is detected within **50 cm**.  

### **Code Implementation**
```cpp
#include <ch32v00x.h>

#define TRIG_PIN GPIO_Pin_2
#define ECHO_PIN GPIO_Pin_3
#define TRIG_PORT GPIOD
#define ECHO_PORT GPIOD

#define BUZZER_PIN GPIO_Pin_4
#define BUZZER_PORT GPIOD

void GPIO_Config(void) {
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD, ENABLE);

    GPIO_InitTypeDef GPIO_InitStructure;

    GPIO_InitStructure.GPIO_Pin = TRIG_PIN;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(TRIG_PORT, &GPIO_InitStructure);

    GPIO_InitStructure.GPIO_Pin = ECHO_PIN;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING;
    GPIO_Init(ECHO_PORT, &GPIO_InitStructure);

    GPIO_InitStructure.GPIO_Pin = BUZZER_PIN;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_Init(BUZZER_PORT, &GPIO_InitStructure);
}

uint32_t get_distance() {
    uint32_t duration, distance;

    // Send a 10us pulse to trigger the ultrasonic sensor
    GPIO_SetBits(TRIG_PORT, TRIG_PIN);
    Delay_Us(10);
    GPIO_ResetBits(TRIG_PORT, TRIG_PIN);

    // Wait for the echo signal to go HIGH
    while (GPIO_ReadInputDataBit(ECHO_PORT, ECHO_PIN) == RESET);

    // Measure the duration of the HIGH pulse
    duration = 0;
    while (GPIO_ReadInputDataBit(ECHO_PORT, ECHO_PIN) == SET) {
        duration++;
        Delay_Us(1);
        
        // Prevent infinite loop if no valid signal is received
        if (duration > 30000) {  
            return 0;  // Return 0 if no object is detected
        }
    }

    // Convert duration to distance (in cm)
    distance = (duration * 0.0343) / 2;

    // Ignore noisy readings beyond 200 cm
    if (distance > 200) {
        return 0;
    }

    return distance;
}

void buzzer_alert(uint32_t distance) {
    if (distance == 0) {  // No object detected
        GPIO_ResetBits(BUZZER_PORT, BUZZER_PIN);
        return;
    }

    if (distance < 10) {  // Close object: Continuous beep
        GPIO_SetBits(BUZZER_PORT, BUZZER_PIN);
        Delay_Ms(500);
    } else if (distance >= 10 && distance <= 30) {  // Medium range: Beep intermittently
        GPIO_SetBits(BUZZER_PORT, BUZZER_PIN);
        Delay_Ms(500);
        GPIO_ResetBits(BUZZER_PORT, BUZZER_PIN);
        Delay_Ms(500);
    } else {  // Far object: No sound
        GPIO_ResetBits(BUZZER_PORT, BUZZER_PIN);
    }
}

int main(void) {
    NVIC_PriorityGroupConfig(NVIC_PriorityGroup_1);
    SystemCoreClockUpdate();
    Delay_Init();
    GPIO_Config();

    while (1) {
        uint32_t distance = get_distance();
        buzzer_alert(distance);
        Delay_Ms(500);
    }
}

```

## **Working Principle**  
- The **HC-SR04 ultrasonic sensor** continuously sends **ultrasonic pulses** and measures the **time taken** for the echo to return.  
- The **VSD_Squadron-MINI-BOARD** processes the time data and calculates the **distance** of obstacles.  
- If an object is detected within **50 cm**, the **active buzzer** is activated to alert the user.  
- If the obstacle is **farther than 50 cm**, the **buzzer remains off**.  
- The system **continuously updates** the obstacle distance in real time.  
- This project provides a **cost-effective, real-time obstacle detection solution** for visually impaired users, enhancing their **mobility and safety**.  


### Expected Output

#### Scenario 1: No Object Detected
- Distance = 0 cm
- Buzzer: OFF (No sound)

#### Scenario 2: Object Very Close (< 10 cm)
- Distance = 10 cm
- Buzzer: Continuous Beep (ON)

#### Scenario 3: Object at Medium Distance (10 - 30 cm)
- Distance = 20 cm
- Buzzer:  Beeping (500ms ON, 500ms OFF)

#### Scenario 4: Object Far (> 30 cm)
- Distance = 40 cm
- Buzzer:  OFF (No sound)

#### Scenario 5: Object Removed from Range
- Distance = "not mentionable"
- Buzzer: OFF (No sound)

## **Future Enhancements**  
- **Multiple Distance Zones**: Implement different buzzer patterns for varying distances.  
- **Voice Alerts**: Use a **text-to-speech (TTS) module** to provide audio feedback.  
- **Bluetooth/Wi-Fi Integration**: Connect the system to a **mobile app** for additional features.  
- **Battery Optimization**: Improve **power consumption** for longer operation.  

## **Contributing**  
If you want to **improve this project**, feel free to:  
1. **Fork** the repository.  
2. **Make necessary changes** or enhancements.  
3. **Submit a Pull Request (PR)** for review.  

Your contributions are **highly appreciated!** 🚀  


