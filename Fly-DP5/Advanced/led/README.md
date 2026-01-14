# Expanding Drive LEDs with Plug-ins

Requires ssh connection to the host and follow the link below for installation and configuration

```
https://github.com/julianschill/klipper-led_effect
```

## Installation ​

Please use **SSH tools** such as **MobaXterm\_Personal** to connect to your machine via **WIFI** and you need to determine the following points

1.  **Make sure the Klipper firmware is installed on the host computer**
2.  **Make sure that the logged in user must be a well-installed Klipper user.**
3.  **Make sure your input is in English.**
4.  **Make sure your host can be searched to your device normally.**
5.  **Please ensure that all the above considerations are done, otherwise you cannot take the next step.**

The module can be installed into an existing Klipper installation using the installation script.

```
cd ~
git clone https://github.com/julianschill/klipper-led_effect.git
cd klipper-led_effect
./install-led_effect.sh
```

## Reference configuration ​

```
[neopixel TP_led]
pin: PB7
chain_count: 25
# Number of LEDs
# Number of lamp beads
color_order: GRB
initial_RED: 0.4    
initial_GREEN: 0.8
initial_BLUE: 1
initial_WHITE: 0.0
#66CCFF 

[led_effect sb_nozzle_cooling]
autostart:              false
frame_rate:             24
leds:
    neopixel:TP_led (9,10)
layers:
        breathing  3 1 top (0.0, 0.0, 1.0, 0.1)

[led_effect rainbow]
leds:
    neopixel:TP_led
autostart:                          true
frame_rate:                         24
layers:
    gradient  0.3  1 add (0.3, 0.0, 0.0),(0.0, 0.3, 0.0),(0.0, 0.0, 0.3)
```