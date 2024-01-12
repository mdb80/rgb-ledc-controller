# RGB LED Controller Library for ESP32 (ESP-IDF)

This repository contains an implementation of an RGB LED Controller Library, compatible with ESP32 microcontrollers and ESP-IDF version 5.0.2.

The library provides asynchronous functionality to initialize and control the brightness and color of RGB LEDs through a Pulse Width Modulation (PWM) interface using ESP-IDF's LED Control (LEDC) module. It leverages the High-Resolution Timer (ESP Timer) module of ESP-IDF, ensuring precise timing and smooth transitions in the lighting effects.

Developers can easily initialize the library and utilize its functions to control the brightness and color of RGB LEDs with precision. The library's asynchronous nature, combined with the High-Resolution Timer (ESP Timer), allows for efficient and accurate handling of LED control operations, enhancing the overall performance and responsiveness of ESP32 projects.

## Features

Here's what the RGB LED Controller Library offers:

- **Independent LED Control**: Each RGB LED can be controlled independently, which allows for complex lighting effects across multiple LEDs. Please note that the maximum number of LEDs that can be controlled simultaneously is limited to 2 due to the LEDC module having only 8 channels available.
- **Color and Brightness Control**: The library provides precise control over the color (through RGB values) and brightness (through PWM signal) of each LED.
- **Various Lighting Effects**: Several lighting effects are supported including blinking, color transitions, and "breathing" effects (gradual brightening and dimming). Each effect comes with customizable timing parameters to fit a variety of application needs.
- **Easy-to-Use API**: The functions provided by the library have a straightforward interface, making it easy to add sophisticated LED control to your ESP32 projects. The API functions are designed to be asynchronous, utilizing the high precision timer module of ESP-IDF, ensuring precise timing and smooth transitions in the lighting effects.


## License

This software is licensed under the Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-nd/4.0/. You may not use, distribute, modify, or sell this software without the express permission of the author. For more information, please contact the author [here](mailto:mdb80dev@gmail.com).

## Installation

Follow the steps below to install the RGB LED Controller library:

1. First, clone this repository to your local machine. You can do this by navigating to the directory where you want to clone the repository and then using the following command in your terminal: 

    ```bash
    https://github.com/MDB80/rgb_ledc_controller.git
    ```

2. Open your ESP-IDF project in Visual Studio Code.

3. In the Explorer sidebar, you'll see your project structure. Navigate to the `components` directory in your project.

4. The library is now ready to be used in your project. To use it, include the `rgb_ledc_controller.h` file in your source code. The ESP-IDF build system will automatically build and link the library.

## Documentation

Technical documentation for the RGB LED Controller Library is available [here](https://mdb80.github.io/rgb_ledc_controller/html/index.html). It provides detailed information about the library's usage, API reference, and examples.

We encourage you to refer to the documentation for a better understanding of the library's capabilities and how to integrate it into your ESP32 projects.


## Contributing

Thank you for your interest in contributing to this project. If you would like to make contributions, please reach out to me [here](mailto:mdb80dev@gmail.com) to discuss your ideas and obtain permission. I appreciate your understanding and cooperation in this matter.

## Donation

If you find this project useful and want to support my work, you can make a donation via PayPal. Alternatively, if you'd like to treat me to a nice beer 🍺 or a coffee ☕, that would also be much appreciated! Any amount, no matter how small, is greatly appreciated and will be used to continue the development and maintenance of this project.

Thank you for your support!

[![Donate](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/donate/?hosted_button_id=RHJS8QKDZBNVN)


## Example

```c

#include "rgb_ledc_controller.h" // Include the header file for the RGB LED functions

// Define the GPIO pins for the Red, Green, and Blue LEDs
#define GPIO_LED_RED 18  
#define GPIO_LED_GREEN 19
#define GPIO_LED_BLUE 20 

// Define colors as RGB values in hexadecimal
#define RED     0xFF0000 // Red color
#define GREEN   0x00FF00 // Green color
#define BLUE    0x0000FF // Blue color
#define YELLOW  0xFFFF00 // Yellow color (combination of Red and Green)
#define VIOLET  0xEE82EE // Violet color

// The main function of the program
void app_main(void)
{
    // Create an instance of the RGB LED structure and initialize it with the defined GPIO pins and LEDC channels
    rgb_led_t led1 = rgb_led_new(GPIO_LED_RED, GPIO_LED_GREEN, GPIO_LED_BLUE, LEDC_CHANNEL_0, LEDC_CHANNEL_1, LEDC_CHANNEL_2);
    //...you can add one more eg: led2.It is limited to 2 leds due to the LEDC module having only 8 channels available.
    
    // Initialize the RGB LED, check for any errors during the process
    ESP_ERROR_CHECK(rgb_led_init(&led1));

    // Main loop of the program
    while (true){
        // Start a "breathing" effect with the Red color and a period of 100ms
        rgb_led_start_breath_effect(&led1, RED, 100);
        vTaskDelay(pdMS_TO_TICKS(5000)); // Delay for 5 seconds

        // Start a blinking effect with the Green color. Blink 3 times, each blink lasts 500ms, and the time between blinks is 500ms. The total effect duration is 3000ms.
        rgb_led_start_blink_effect(&led1, GREEN, 3, 500, 500, 3000);
        vTaskDelay(pdMS_TO_TICKS(5000)); // Delay for 5 seconds

        // Start a "breathing" effect with the Blue color and a period of 100ms
        rgb_led_start_breath_effect(&led1, BLUE, 100);
        vTaskDelay(pdMS_TO_TICKS(5000)); // Delay for 5 seconds

        // Start a blinking effect with the Yellow color. Blink 3 times, each blink lasts 500ms, and the time between blinks is 500ms. The total effect duration is 3000ms.
        rgb_led_start_blink_effect(&led1, YELLOW, 3, 500, 500, 3000);
        vTaskDelay(pdMS_TO_TICKS(5000)); // Delay for 5 seconds

        // Start a color transition effect from Yellow to Red, the total effect duration is 5000ms.
        rgb_led_start_transition_effect(&led1, YELLOW,RED, 5000);
        vTaskDelay(pdMS_TO_TICKS(5000)); // Delay for 5 seconds

        // Set the LED color Fixed to Violet
        rgb_led_set_color(&led1,VIOLET);
        vTaskDelay(pdMS_TO_TICKS(5000)); // Delay for 5 seconds
    }
}
```
## Output
```
I (1307) gpio: GPIO[18]| InputEn: 0| OutputEn: 0| OpenDrain: 0| Pullup: 1| Pulldown: 0| Intr:0
I (1316) gpio: GPIO[19]| InputEn: 0| OutputEn: 0| OpenDrain: 0| Pullup: 1| Pulldown: 0| Intr:0
I (1325) gpio: GPIO[20]| InputEn: 0| OutputEn: 0| OpenDrain: 0| Pullup: 1| Pulldown: 0| Intr:0
I (1335) LED_RGB: Started breath effect for LED
I (6340) LED_RGB: Breath effect stopped for LED
I (11340) LED_RGB: Blink effect stopped for LED
I (11340) LED_RGB: Started breath effect for LED
I (16340) LED_RGB: Breath effect stopped for LED
I (21340) LED_RGB: Blink effect stopped for LED
I (21340) LED_RGB: Transition effect started for LED
I (26340) LED_RGB: Transition effect stopped for LED
```
