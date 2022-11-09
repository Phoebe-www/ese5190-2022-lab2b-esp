### TODO:

Create a REPL to let you read and write RP2040 registers from a console. You should be able to:
- select any 32-bit address to read/write (even if not a valid RP2020 address)
- read/write any 32-bit value to this address
- read/write using any of the atomic bit-setting aliases and a 32-bit mask

## code
    #include "pico/stdlib.h"
    #include <stdio.h>
    #include "ws2812.h"
    #include "hardware/pio.h"
    #include "hardware/clocks.h"
    #include "ws2812.pio.h"
    #include "registers.h"
    #include "ws2812.pio.h"

    int main() {

    uint32_t addr_input = 0x00000000;
    ADDRESS addr = 0x00000000;
    VALUE data;
    int opt;

    stdio_init_all();

    // while (!stdio_usb_connected());

    rp_init();
    turn_on_pixel();

    uint32_t rgb = 0x00ffffff;
    set_pixel_color(rgb);

    while (true) {
        printf("New Program start: \n");
        printf("Input a register address you would like to read/write: \n");
        scanf("%x", &addr_input);
        printf("Confirmation of address: %x\n", addr_input);
        addr = (ADDRESS) addr_input;
    
        printf("Input 0 for reading and 1 for writing: \n");
        scanf("%d", &opt);
        if(opt == 0) {
            printf("The data read is: %x\n", register_read(addr));
        } else {
            printf("Type in the data you want to write into the address: \n");
            scanf("%x", &data);
            register_write(addr, data);
            printf("Confirmation of data written in: %x\n", register_read(addr));
        }
        sleep_ms(250);

     }

    return 0;
    }
    
    
    ## result
    ![image](https://user-images.githubusercontent.com/113930091/200722185-93a54f30-9f1d-40a9-83f3-60e74ab801a9.png)

