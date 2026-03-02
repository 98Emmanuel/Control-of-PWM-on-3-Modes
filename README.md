STM32 Multi-Mode PWM Control System
🚀 Overview

This project implements a multi-source PWM control system using an STM32 microcontroller.

The PWM duty cycle can be controlled using three different input modes:

📱 Bluetooth (USART1)

💻 PC Keyboard / Serial Console (USART2)

🎛 Analog Potentiometer (ADC)

A push-button interrupt is used to switch between control modes dynamically.

This project demonstrates the integration of:

ADC (Analog-to-Digital Conversion)

Timer-based PWM generation

Dual UART communication

Interrupt handling

State machine design using enum

External interrupt (EXTI)

🧠 System Architecture

The system operates using a simple state machine:

typedef enum{
    BLUETOOTH_MODE = 0,
    KEYBOARD_MODE,
    POTENTIOMETER_MODE
} Control_mode;

A push button (PC13) cycles through the modes:

Mode	Input Source	Peripheral Used
0	Bluetooth	USART1
1	PC Keyboard	USART2
2	Potentiometer	ADC1

PWM output is generated using TIM3 Channel 2.

⚙️ Hardware Used

STM32 (e.g., Nucleo-L476RG)

Potentiometer connected to ADC Channel 9

Bluetooth module (HC-05 or similar)

Push button (PC13)

LED indicator (PB6)

USB-to-Serial (for PC console)

🔌 Peripheral Configuration
🔹 ADC

12-bit resolution

Continuous conversion mode

Software triggered

Used to scale input voltage to PWM duty cycle

🔹 Timer (TIM3)

Prescaler: 79

Period: 999

PWM Mode 1

0–999 duty range

🔹 UART
UART	Purpose	Baudrate
USART1	Bluetooth	9600
USART2	PC Console	115200
🔹 GPIO

PC13 → External interrupt (mode switching)

PB6 → Status LED

🔄 How It Works
🎛 Potentiometer Mode

ADC reads analog value

Value scaled from 0–4095

Converted to PWM duty (0–999)

PWM updated in real time

💻 Keyboard Mode

User sends numeric value via serial terminal

Value received using UART interrupt

Converted using atoi()

PWM updated accordingly

Example input:

500
📱 Bluetooth Mode

User sends numeric value via Bluetooth terminal app

Value received via USART1 interrupt

PWM updated

🧩 Key Concepts Demonstrated

Interrupt-driven UART reception

Real-time PWM updates

Mode switching using EXTI

Scaling ADC values to control signals

Embedded state machine design

Multi-peripheral integration

🛠 Example Code Snippet
Updating PWM Duty Cycle
__HAL_TIM_SET_COMPARE(&htim3, TIM_CHANNEL_2, duty);
Mode Switching (EXTI Callback)
void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)
{
    if(GPIO_Pin == GPIO_PIN_13){
        current_mode++;

        if(current_mode > POTENTIOMETER_MODE){
            current_mode = BLUETOOTH_MODE;
        }
    }
}
📈 Project Features

✔ Multi-input control
✔ Real-time PWM response
✔ Interrupt-based design
✔ Modular readable structure
✔ Clean state-machine implementation

🎯 Possible Improvements

Add OLED display for current mode

Implement input validation (0–999 limit)

Add software debounce for button

Convert to fully non-blocking ADC using DMA

Separate drivers into modular files

Add FreeRTOS task-based architecture

📂 Folder Structure
Core/
 ├── Src/
 │    └── main.c
 ├── Inc/
Drivers/
STM32CubeIDE project files
📌 Learning Outcomes

This project strengthens understanding of:

Embedded firmware architecture

Peripheral configuration using STM32CubeIDE

Interrupt-driven systems

Real-time signal control

Multi-interface communication

👨‍💻 Author

Emmanuel Odongo
Embedded Systems Developer
Focused on real-time control systems and firmware design.
