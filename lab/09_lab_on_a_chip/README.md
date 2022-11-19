### TODO:

You can now actually measure the timings you modeled in the last lab. Add APDS9960 support to your sensor, then set your system up to capture the following timing info:
- color packet delivered to PIO module
- bit delivered to WS2812 (24x/packet)
- full packet delivered to WS2812
- brightness changed on APDS

Run this experiment in both dark and light room settings (record initial ambient brightness in each case). The Neopixel should start 'off' and the ADPS9960 should be initialized with your preferred sampling rate (you may want to try a few different rates). Run the experiment for at least 100 samples at brightness settings of 0%, 25%, 50%, 75%, 100% (making sure to give the ADPS reading enough time to 'settle' each time Neopixel is turned off).

Report the observed 'jitter' based on misalignment with the free-running PWM module on the WS2012.

## code
```
#include "APDS9960.h"
#include "pio_i2c.h"
#include "ws2812.h"

#define PIN_SDA 22
#define PIN_SCL 23

const int addr = 0x39;
const int max_read = 250;


struct Info{
    int32_t r;
    int32_t g;
    int32_t b;
    int32_t c;
    int32_t proximity;
};

void readInfo(struct Info* info, PIO pio, uint sm, uint8_t addr, bool nostop){
    read_proximity(&info->proximity, pio, sm, addr, nostop);
    read_rgbc(&info->r, &info->g, &info->b, &info->c, pio, sm, addr, nostop);
    // Print out the values
    printf("Proximity: %d, red: %d, green: %d, blue: %d, clear: %d\n", info->proximity, info->r, info->g, info->b, info->c);
}

int main(){
    stdio_init_all();
    sleep_ms(5000);
    printf("=========");

    PIO pio_0 = pio0;
    uint sm_0 = 0;

    uint offset_0 = pio_add_program(pio_0, &i2c_program);
    i2c_program_init(pio_0, sm_0, offset_0, PIN_SDA, PIN_SCL);
    
    APDS9960_init(pio_0, sm_0, addr, false);
    
    printf("APDS9960 initialized\n");
    
    struct Info info;
    while(true){
        printf("====In Loop=====\n");
        sleep_ms(1000);
        readInfo(&info, pio_0, sm_0, addr, false);

        printf("Finished the readInfo\n");
        
//        printf("%8x\n",info.r);
//        printf("%8x\n",info.g);
//        printf("%8x\n",info.b);
        set_neopixel_color(rgb_to_neopixel(info.r>>8, info.g>>8, info.b>>8));

        printf("Finished the set_neopixel_color\n");
    }

}
```
## result

![c85ab9e8b83de5f16df25679a2531e2](https://user-images.githubusercontent.com/113930091/202834234-b86b2640-9a4d-4d81-b5f0-c5fe0ec54b06.jpg)

 
![89c0ff47a3af98f9b49b3bef710d192](https://user-images.githubusercontent.com/113930091/202834245-f27e0074-0515-42c3-b353-e6470d29780b.jpg)

![e4495201d7ccb29b1ac742217b31c2b](https://user-images.githubusercontent.com/113930091/202834252-d91bea65-b98a-4fc0-bcd7-6c0a72d7c17c.jpg)

![2aa40d21753ce0464ea5ff5b7537379](https://user-images.githubusercontent.com/113930091/202834258-609ee994-b939-4a8f-9ded-b8397d45f726.jpg)

