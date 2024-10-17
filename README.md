# Hello-World-RTOS

This project demonstrates a simple RTOS (Real-Time Operating System) setup using FreeRTOS on an embedded system. The system consists of four tasks running in parallel, each performing a specific function. The tasks are:

1. **pickButton Task**: Monitors the state of a button.
2. **getADC Task**: Reads analog data from an ADC (Analog-to-Digital Converter).
3. **dispOLED Task**: controls LEDs based on system state.
4. **dispUART Task**: Communicates with a UART (Universal Asynchronous Receiver-Transmitter) interface to receive user inputs and display data.

## About FreeRTOS

FreeRTOS is a popular open-source real-time operating system kernel for embedded devices. It allows for the creation of multiple tasks, each of which can perform a specific action, with scheduling and prioritization. In this project, we use FreeRTOS to handle the execution of four tasks, ensuring that each task runs efficiently and responds to real-time events.

### FreeRTOS Task Relations

Below is a diagram that illustrates how the four tasks interact and affect one another:

```plaintext
       +----------------+                +----------------+
       |                |                |                |
       |  pickButton    |                |    getADC      |
       |   (Task 1)     |                |    (Task 2)    |
       |  - Monitors    |                |  - Reads ADC   |
       |    button1     |                |    values      |
       |                | -----|  |----- |                |
       +-------+--------+      |  |       +-------+--------+
               |               |  |               |
               v      v--------|--|               v
       +----------------+      |------>  +----------------+
       |                |                |                |
       |   dispOLED     | <------------- |   dispUART     |
       |   (Task 3)     |                |    (Task 4)    |
       |  - Controls    |                |  - Displays    |
       |    LED/OLED    |                |    ADC values  |
       |    state       |                |    over UART   |
       |                |                |                |
       +----------------+                +----------------+
```

- **Task 1 (pickButton)**: This task monitors a button press and modifies the system state, influencing both the **dispOLED** task (by changing LED/OLED display states) and the **dispUART** task (by sending UART messages when the button is pressed).
  
- **Task 2 (getADC)**: The task reads ADC values continuously. These values are transmitted to the global value that will be used in **dispUART** task and **dispOLED** task.
  
- **Task 3 (dispOLED)**: This task controls the display on an OLED screen and controls LEDs based on the system state, which is updated by the **pickButton** task and **getADC** task.
  
- **Task 4 (dispUART)**: This task communicates with the UART interface. It displays the ADC readings and responds to user input via UART. It can also indicate when the button is pressed (from **pickButton**).

### Task Descriptions

- **pickButton**: This task monitors the GPIO pin connected to `Button1`. When the button is pressed, it updates the `button1_pressed` flag and increments the system state.
- **getADC**: This task reads the ADC values from `hadc1`, and the results are stored in `x_val`. These values can be processed and displayed via UART.
- **dispOLED**: This task handles the LED controls. Depending on the state (set by `pickButton`), it will adjust the behavior of the LEDs.
- **dispUART**: This task interacts with the UART interface. It displays the current ADC value when prompted by user input and indicates button presses.

### Hello World (hardware)
Berikut adalah link foto hasil pengujian pada modul stm32

![WhatsApp Image 2024-10-17 at 10 58 42_727b6b07](https://github.com/user-attachments/assets/9c8563f9-0f3c-4f40-80c6-522a1b06b581)


