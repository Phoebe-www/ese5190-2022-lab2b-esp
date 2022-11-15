### TODO:

Update your sequencer to be able to slow down and speed up recordings/replay. On the input side, the goal is ultimately to be able to handle the full 4 Gbps "firehose" from the PIO logic analyzer example in the SDK manual, which would fill up 256Kb of RAM in only 31 µs at a system clock speed of 125000 MHz if captured 'raw'! On the output side, the goal is to be able to output precisely timed sequences from the PIO at system clock resolution based on a handful of control points.

- update your sequencer to be able to record just the timestamped transitions between input values, and to be able to play data in this format
- give your sequencer the ability to select a range of output rates for both live and recorded input


## code
    #include "pico/stdlib.h"
    #include <stdio.h>
    #include "neopixel.h"
    #include "hardware/gpio.h"
    #include "ws2812.pio.h"
    #include "hardware/pio.h"
    #define PIO         pio0
    #define SM          0
    #define FREQ        800000
    #define PIN         12
    #define POWER_PIN   11
    #define IS_RGBW     true  

    #define QTPY_BOOT_PIN 21

    typedef struct {
      uint32_t last_serial_byte;
      uint32_t button_is_pressed;
      uint32_t light_color;
    } Flashlight; 
