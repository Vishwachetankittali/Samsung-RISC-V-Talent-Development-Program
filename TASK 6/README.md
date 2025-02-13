## **Program**  
The following **Arduino/C code** measures the distance using the **HC-SR04 ultrasonic sensor** and triggers the **active buzzer** when an obstacle is detected within **50 cm**.  

### **Code Implementation**
```cpp
#include <ch32v00x.h>
#include <debug.h>

#define TRIG_GPIO_PORT GPIOD
#define TRIG_GPIO_PIN GPIO_Pin_3
#define ECHO_GPIO_PORT GPIOD
#define ECHO_GPIO_PIN GPIO_Pin_2
#define BUZZER_GPIO_PORT GPIOD
#define BUZZER_GPIO_PIN GPIO_Pin_4

#define TRIG_CLOCK_ENABLE RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD, ENABLE)
#define ECHO_CLOCK_ENABLE RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD, ENABLE)
#define BUZZER_CLOCK_ENABLE RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD, ENABLE)

void Timer2_Init(void);
uint32_t Measure_Distance(void);

int main(void)
{
    NVIC_PriorityGroupConfig(NVIC_PriorityGroup_1);
    SystemCoreClockUpdate();
    Delay_Ms(100); // Use SDK's delay function
    Timer2_Init(); // Initialize TIM2 for measuring echo pulse width

    GPIO_InitTypeDef GPIO_InitStructure = {0};

    // Configure Trigger pin as Output
    TRIG_CLOCK_ENABLE;
    GPIO_InitStructure.GPIO_Pin = TRIG_GPIO_PIN;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(TRIG_GPIO_PORT, &GPIO_InitStructure);

    // Configure Echo pin as Input
    ECHO_CLOCK_ENABLE;
    GPIO_InitStructure.GPIO_Pin = ECHO_GPIO_PIN;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING;
    GPIO_Init(ECHO_GPIO_PORT, &GPIO_InitStructure);

    // Configure Buzzer pin as Output
    BUZZER_CLOCK_ENABLE;
    GPIO_InitStructure.GPIO_Pin = BUZZER_GPIO_PIN;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_Init(BUZZER_GPIO_PORT, &GPIO_InitStructure);

    while (1)
    {
        uint32_t distance = Measure_Distance();
        printf("Distance: %lu cm\r\n", distance);

        if (distance > 0 && distance <= 50)
        {
            GPIO_SetBits(BUZZER_GPIO_PORT, BUZZER_GPIO_PIN); // Activate buzzer
        }
        else
        {
            GPIO_ResetBits(BUZZER_GPIO_PORT, BUZZER_GPIO_PIN); // Deactivate buzzer
        }

        Delay_Ms(100); // Use SDK's delay function
    }
}

// Measure distance using Timer2 (TIM2)
uint32_t Measure_Distance(void)
{
    uint32_t duration, distance;

    // Send trigger pulse
    GPIO_ResetBits(TRIG_GPIO_PORT, TRIG_GPIO_PIN);
    Delay_Us(2);  // Use SDK's delay function
    GPIO_SetBits(TRIG_GPIO_PORT, TRIG_GPIO_PIN);
    Delay_Us(10); // Use SDK's delay function
    GPIO_ResetBits(TRIG_GPIO_PORT, TRIG_GPIO_PIN);

    // Wait for Echo pin to go HIGH
    while (!GPIO_ReadInputDataBit(ECHO_GPIO_PORT, ECHO_GPIO_PIN));

    TIM2->CNT = 0;            // Reset counter
    TIM2->CTLR1 |= 0x0001;    // Start Timer (Bit 0 of CTLR1)

    // Wait for Echo pin to go LOW
    while (GPIO_ReadInputDataBit(ECHO_GPIO_PORT, ECHO_GPIO_PIN));

    TIM2->CTLR1 &= ~0x0001;   // Stop Timer (Bit 0 of CTLR1)
    duration = TIM2->CNT;     // Read the captured count

    // Convert time (duration) to distance
    distance = (duration * 0.034) / 2; // Distance in cm

    return distance;
}

// Timer 2 Initialization for Pulse Measurement
void Timer2_Init(void)
{
    RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM2, ENABLE);

    TIM_TimeBaseInitTypeDef TIM_TimeBaseStructure = {0};
    TIM_TimeBaseStructure.TIM_Period = 0xFFFF;
    TIM_TimeBaseStructure.TIM_Prescaler = SystemCoreClock / 1000000 - 1; // 1µs per count
    TIM_TimeBaseStructure.TIM_ClockDivision = TIM_CKD_DIV1;
    TIM_TimeBaseStructure.TIM_CounterMode = TIM_CounterMode_Up;
    TIM_TimeBaseInit(TIM2, &TIM_TimeBaseStructure);

    TIM_Cmd(TIM2, DISABLE); // Keep Timer disabled initially
}

void NMI_Handler(void) {}

void HardFault_Handler(void)
{
    while (1)
    {
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


