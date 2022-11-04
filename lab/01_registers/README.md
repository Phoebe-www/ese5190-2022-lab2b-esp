## code:

#include <stdio.h>
#include <stdlib.h>

#include "pico/stdlib.h"
#include "hardware/pio.h"
#include "hardware/clocks.h"
#include "ws2812.pio.h"

int main() {

    const uint BOOT_PIN = 21;
    gpio_init(BOOT_PIN);
    gpio_set_dir(BOOT_PIN, GPIO_OUT);

    stdio_init_all();
    turn_on_NeoPixel_power();

    uint32_t rgb = 0x00000000;
    set_pixel_color(rgb)
    while (true) {
        if(!gpio_get(BOOT_PIN)){
            rgb = 0x00123456
            set_pixel_color(rgb)
        }else{
            rgb = 0x00000000
            set_pixel_color(rgb)
        }
       

        sleep_ms(250);
        
    }
}


## GIF

