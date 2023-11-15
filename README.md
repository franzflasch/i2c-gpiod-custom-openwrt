# Port of i2c-gpio-custom with gpiod support that works with OpenWrt 23.05.
The code was taken from https://github.com/digitalloggers/i2c-gpio-custom with some modifications:
- removed support for older kernels
- make it work with newer kernel >= 5.15.x
- renamed it to "i2c-gpiod-custom"

# How to build
1. Add it into your OpenWrt build folder. I always create a new folder in packages (e.g. packages/mypackages)
2. Clone this repository to packages/mypackages
3. ```make menuconfig``` -> select i2c-gpiod-custom
4. build OpenWrt.

# How to use (this was tested on the Carambola2)
```
insmod i2c-gpiod-custom bus0=0,18,19
```

Now you should have a new i2c device file in /dev e.g:
```
/dev/i2c-0
```


If you connect some i2c device you can now use i2c-tools e.g.:

```
root@OpenWrt:/# i2cdetect -y 0
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:                         -- -- -- -- -- -- -- -- 
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- 1e -- 
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
50: -- -- -- 53 -- -- -- -- -- -- -- -- -- -- -- -- 
60: -- -- -- -- -- -- -- -- 68 -- -- -- -- -- -- -- 
70: -- -- -- -- -- -- -- 77
```

Checking with gpiod tools you can see that the i2c pins are now reserved:  
Note: With openwrt-23.05 this only works if ```GPIO_CDEV_V1``` is enabled in the kernel.

```
root@OpenWrt:/# gpioinfo 
gpiochip0 - 32 lines:
        line   0:      unnamed "green:wlan"  output   active-low [used]
        line   1:      unnamed       unused  output  active-high 
        line   2:      unnamed       unused  output  active-high 
        line   3:      unnamed       unused  output  active-high 
        line   4:      unnamed       unused  output  active-high 
        line   5:      unnamed       unused  output  active-high 
        line   6:      unnamed       unused  output  active-high 
        line   7:      unnamed       unused  output  active-high 
        line   8:      unnamed       unused   input  active-high 
        line   9:      unnamed       unused   input  active-high 
        line  10:      unnamed       unused  output  active-high 
        line  11:      unnamed        "wps"   input   active-low [used]
        line  12:      unnamed       unused   input  active-high 
        line  13:      unnamed "orange:eth0" output active-high [used]
        line  14:      unnamed "orange:eth1" output active-high [used]
        line  15:      unnamed       unused   input  active-high 
        line  16:      unnamed       unused   input  active-high 
        line  17:      unnamed       unused   input  active-high 
        line  18:      unnamed        "sda"  output  active-high [used open-drain]
        line  19:      unnamed        "scl"  output  active-high [used open-drain]
        line  20:      unnamed       unused   input  active-high 
        line  21:      unnamed       unused   input  active-high 
        line  22:      unnamed       unused   input  active-high 
        line  23:      unnamed       unused   input  active-high 
        line  24:      unnamed       unused   input  active-high 
        line  25:      unnamed       unused   input  active-high 
        line  26:      unnamed       unused   input  active-high 
        line  27:      unnamed       unused   input  active-high 
        line  28:      unnamed       unused   input  active-high 
        line  29:      unnamed       unused   input  active-high 
        line  30:      unnamed       unused   input  active-high 
        line  31:      unnamed       unused   input  active-high
```
