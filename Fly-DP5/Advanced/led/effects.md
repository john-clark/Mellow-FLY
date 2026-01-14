# LED Effect

With its flexibility and relative ease of use, programmable LEDs (Addressable LEDs) are gradually replacing RGB LEDs. Each individual LED element displays full-spectrum colors at extremely high speeds, so it can be used to create a variety of lighting effects.

# Configuring the strips

In your profile, each lamp strap or chain connected to the IO pin must have a definition. It is necessary to define the data pins for each lamp strip (and, if applicable, the clock pins) and the number of LEDs in the lamp chain. LED effect instances can use multiple different types and color sequences at the same time, but each strip must be defined according to its type.

```
[neopixel panel_ring]
pin:                     ar6        # Data pins
chain_count:             16         # Number of LEDs
```

# Configure the effects (configuring the effects)

In a more abstract sense, the effect is the _state_ in which the lamp belt is located. The effect can consist of 1 LED or 100 LEDs. There can be one effect layer or 10 effect layers. All of this is arbitrary and scalable. This means that the only limit to the number of layers and the number of LEDs that can run simultaneously is the processing power of the host operating system. In initial tests, more than 100 LEDs and 12 effect layers were simultaneously running at 24 FPS on the Raspberry Pi 4.

## Definition of Basic Layer (Basic Layer Definition)​

In our example printer, there is a ring consisting of 16 Neopixel LEDs on the front panel; there is also a small section of Neopixel LED located next to the hot end to illuminate the print.

There are also five Dotstar LEDs under the hot bed. The pin numbers listed here are completely fictional.

```
[neopixel panel_ring]             # Front panel ring light strip
pin:                     ar6    
chain_count:             16

[neopixel tool_lights]            # Tool head light
pin:                     ar15
chain_count:             6

[dotstar bed_lights]               # Hot bed lighting
data_pin:                ar21      # Dotstar requires data pins
clock_pin:               ar22      # Dotstar requires data pins
chain_count:             5
```

We hope that when the printer is launched, the ring light blue is lit with a bright light blue, and _the_ brightness _is breathing_ (gradually brightening and then dimming).

```
[led_effect panel_idle]              # Define an effect called panel_idle
autostart:              true         # Boot up automatically
frame_rate:             24           # Frame Rate (FPS)
leds:                                # Apply effect light strips
    neopixel:panel_ring
layers:                              # Effect layer definitions
    breathing  10 1 top (.5,.5,1)    # Breathing Effect Speed 10 Cutoff 1 Top Blending Mode Color (0.5,0.5,1) [Light Blue]
```

This defines a name. `panel_idle`The effect.

### Controlling the effects​

The effect can be active or inactive. Inactive effects do not output any color data, while the active effect returns color data, which superimposes summation on each LED they run.

#### Activating and deactivating effects​

Our example effects can be done by running the GCode command `SET_LED_EFFECT EFFECT=panel_idle`to activate. In order to stop all other effects currently running on the LEDs used in the new effect, `REPLACE`Parameters are set to 1: `SET_LED_EFFECT EFFECT=panel_idle REPLACE=1`Run commands `SET_LED_EFFECT EFFECT=panel_idle STOP=1`This effect will be discontinued again. To disable all effects, we can use the GCode command. `STOP_LED_EFFECTS`. . To disable only the effect of a specific LED, we can specify the LEDS parameter: `STOP_LED_EFFECTS LEDS="neopixel:panel_ring"`You can also specify an index (see below): `STOP_LED_EFFECTS LEDS="neopixel:panel_ring (1-7)"`. . Only one LED parameter can be specified at a time. To stop the effect of multiple LEDs, we must run the command many times.

#### (Fating in and out)​

It can be specified. `FADETIME`Parameters to fade the effect in and out: `SET_LED_EFFECT EFFECT=panel_idle FADETIME=1.0`The effect fades in within 1 second. Operation `SET_LED_EFFECT EFFECT=panel_idle STOP=1 FADETIME=1.0`Remove it in 1 second. We can also run. `STOP_LED_EFFECTS FADETIME=1.0`Remove all effects. It can also be passed `REPLACE`Parameters and `SET_LED_EFFECT`Use in combination to achieve a cross-in fade between effects (see above): `SET_LED_EFFECT EFFECT=panel_idle REPLACE=1 FADETIME=1.0`

### Additional effect level parameters (additional effect level parameters)​

*   `autostart: true`\- Automatically start the effect at startup.
*   `frame_rate:`\- Set the frame rate of the effect (number of frames per second).
*   `run_on_error:`\- (Patched MCU firmware required.) Currently not supported.)
*   `heater:`\- Specifies the heater for the heater effect. Use of `extruder`Indicates the extruder, `heater_bed`Means a hot bed. For temperature-controlled fans or sensors, add types and use quotes. Examples:`heater: "temperature_fan myfan"`
*   `analog_pin:`\- Specifies the pin for the effect that requires an analog signal.
*   `stepper:`\- Specifies the shaft for stepper motor effect. Possible values are:`x`, , `y`and `z`. . Examples:`stepper: x`
*   `endstops:`\- Specifies the limit switch that takes effect when the homing effect is triggered. Multiple limit switches can be specified as comma-separated lists. Possible values are:`x`, , `y`, , `z`and `probe`. . Examples:`endstops: x, y`

## Defining LEDs (Defining LEDs)​

`leds:`The section is a list of Neopixel or Dotstar lamp bands that make up the effect. Both types can be used for the same effect. Every light is in `leds:`A separate line below the section is defined and indented.

```
leds:
    neopixel:panel_ring
    neopixel:tool_lights
    dotstar:bed_lights
```

In addition, it can be decided to let only specific LEDs display the effect. This is achieved by providing the LED index to be used after the lamp band name. The index can be a list or a range. The range can also be inverted to reverse the effect direction. If the index is omitted, the entire strip is used.

Similarly, if needed for some reason, the same strip can be used twice in one effect and specifies different LEDs.

```
leds:
    neopixel:tool_lights              # 使用整个tool_lights灯带
    neopixel:panel_ring  (1-7)        # 使用panel_ring灯带的LED 1到7
    neopixel:panel_ring  (16-9)       # 使用panel_ring灯带的LED 16到9 (倒序)
    dotstar:bed_lights   (1,3,5)      # 使用bed_lights灯带的LED 1, 3, 5
```

## Defining effect layers (defining effect layers)​

The effect is generated frame-by-frame. The number of pixels per frame is equal to the number of LEDs defined for the effect. Therefore, a specified 22 LED effect will have 22 pixels per frame.

Each effect layer is generated as a frame. Each frame is mixed with the next layer to produce the final effect. Mixing is cumulative, and how colors mix is defined by the mixing pattern at the top. Each effect layer is listed on its own rows, and each layer has its own settings.

Most effect layers (such as breathing breathing and gradient gradient) are pre-rendered at Klipper startup to save subsequent computational overhead. Other effects (such as Fire Fire and Flash Twinkle) are rendered on demand.

Each layer is defined using the following parameters:

*   **Layer name** - effect type (such as static, breathing, fire, etc.)
*   **Effect Rate** - the digital parameter that controls the speed, cycle, or intensity of the effect (specific meaning varies depending on the effect)
*   **Cutoff** – another numerical parameter that normally controls the length, attenuation, or threshold of the effect (specific meaning varies depending on the effect)
*   **Blending mode** - defines how the layer mixes with the lower layer (e.g. top, add, screen, etc.)
*   **Color palette** - defines the effect in the color used in the format (R, G, B) or (R, G, B, W), values from 0.0 to 1.0

Each layer must be defined on a separate line, and each line must be indented. The length of the palette can be arbitrary, but may be compressed based on the frame size or the number of LEDs on the belt. Color is defined as the grouping of red, green, blue, and (optional) white. The white channel is only used for RGBW LEDs and will be ignored on RGB LEDs. Each color ranges from a decimal of 0.0 to 1.0. Therefore, yellow should be used (1.0, 1.0, 0.0). For white, it should be used on RGB LEDs (1.0, 1.0, 1.0) and on RGBW LEDs (0.0, 0.0, 0.0, 1.0).

Individual colors must be enclosed in parentheses and separated by commas.

Some example color palettes:

Rainbow (1.0, 0.0, 0.0), (0.0, 1.0, 0.0), (0.0, 0.0, 1.0) # red -> green -> blue Flame Fire (0.0, 0.0, 0.0), (1.0, 0.0, 0.0), (1.0, 1.0, 1.0, 1.0) #Black -> Yellow -> White Blue Comet (0.8, 1.0, 1.0), (0.0, 0.8, 1.0), (0.0, 0.0, 1.0) # Shallow -> Blue -> Deep Blue

```
layers:
   breathing  .5 0 screen (0,.1,1), (0,1,.5), (0, 1,1), (0,.1,.5) # 呼吸效果 速度0.5 截止值0 屏幕混合模式 4种蓝色调
   static     1 0 bottom (1,.1,0), (1,.1,0), (1,.1,0), (1,1,0)    # 静态效果 速率1 截止值0 底部混合模式 橙色(重复4次)
```

There are several effects to choose from.

#### Static (Static)​

**Effect Rate:** Not used, but must be provided **Cutoff:** not used, but must be provided **Palette:** Color is evenly mixed on the lamp Displays a single color and does not change. If a palette containing multiple colors is provided, the colors will be evenly mixed on the LED based on hue differences.

#### Linear Feroling (LinearFade)​

**Effect Rate:** 3 Duration of a full cycle (seconds) **Cutoff:** 0 not used, but must be provided **Palette:** Color loops in order LEDs are linearly gradientd between colors. If a palette containing multiple colors is provided, it loops through these colors in the order specified in the palette. The effect rate parameter controls the time it takes to traverse all colors.

#### Breathing (breathing)​

**Effect Rate:** 3 Duration of a full breathing period (bright->dark->bright) (seconds) **Cutoff:** 0 not used, but must be provided **Palette:** Color loops in order The color gradually brightens and then darkens (breathing). If a palette containing multiple colors is provided, it loops through these colors in the order specified in the palette. The effect rate parameter controls the time it takes to “breathe” once.

#### Flashing (Blink)​

**Effect Rate:** 1 Duration of a full period (bright->灭->blow) (seconds) **Cutoff:** 0.5 LEDs are on the time ratio (between 0 and 1) **Palette:** Color loops in order The LED is completely lit and completely extinguished according to the effect speed. If a palette containing multiple colors is provided, it loops through these colors in the order specified in the palette.

#### Strobe (Strobe)​

**Effect Rate:** 1 number of strobe per second **Cutoff:** 1.5 Determine the attenuation rate. The larger the value, the faster it decays. **Palette:** Color loops in order The LED is completely lit up and then extinguished with attenuation over time. If a palette containing multiple colors is provided, it loops through these colors in the order specified in the palette. The effect rate controls the number of strobes per second of the light. Deadline value parameters control the attenuation rate. 1.5 is a good attenuation rate value.

#### Flashing (Twinkle)​

**Effect rate:** 1 Increase the probability of LED lighting. **Cutoff:** .25 determines the attenuation rate. The larger the value, the faster it decays. **Palette:** Random selection of colors Light and attenuate along the lamp strip. If a palette is specified, a color is randomly selected from the palette.

#### Gradient (gradient)​

**Effect Rate:** The **speed** of the 1 gradient cycle\*\*,\*\* negative change direction. **Cutoff:** 1 Number of gradients on the lamp chain (the larger the value, the shorter the single gradient) **Palette:** A linear gradient with uniform spacing\*\*.\*\* The colors in the palette are mixed into a linear gradient along the length of the lamp strip. The effect rate parameter controls the speed of the color cycle. The negative value of the effect rate changes the direction of the gradient loop (from right to left vs from left to right). The cut-off value determines the relationship between the gradient length relative to the length of the lamp chain. The larger the value, the shorter the gradient (for example, the value 2 indicates 2 gradients on the length of the lamp chain).

#### The pattern (pattern)​

**Effect Rate:** 1 pattern moving time interval (seconds) **Cutoff:** Number of LED positions per movement of 1 pattern **Palette:** Pattern to move The palette is applied as a repeating pattern to the lamp chain and moves along the lamp chain. The effect rate determines the time between movements (seconds), and the cut-off value determines the number of LED positions per movement of the pattern.

#### The Comet (Comet)​

**Effect Rate:** 1 The speed of the comet's movement, the negative change direction **Cutoff:** 1 length of the tail (relative value) **Palette:** The color of the "meat head" and the gradient of the "meat tail" A point of light passes through the LED and has a tailing attenuation. Directions can be controlled by using negative effect rate values. The color of the palette determines the color of the comet and the tail. The first color of the palette defines the color of the comet's "head", and the rest is mixed into the "tail".

#### Chasing (Chase)​

**Effect Rate:** 1 point of light moving **speed**, negative change direction **Cutoff:** 1 length of trail (relative value) **Palette:** The color of the “head” and the gradient of the “tail” The settings are the same as comets, but there are multiple points of light chasing each other.

#### Heater (heater)​

**Effect Rate:** 1 minimum temperature (°C) for activation **Cutoff:** 0 Disabling effect after reaching target temperature (1= disabled, 0= remain static) **Palette:** Color values mixed from cold to hot This effect is activated when the specified heater is active or when the temperature is higher than the specified minimum temperature. For example, if the heater is turned on and the target temperature is set, the LED will cycle the color until the target temperature is reached. Once the target temperature is reached, the last color of the gradient is used, and the effect essentially becomes a static color until the heater state changes. If the cutoff value parameter is provided (set to 1), the effect is disabled after the target temperature is reached. If the heater is turned off, the color will follow this pattern in reverse until the temperature drops below the minimum temperature specified in the configuration. This can be used to indicate that the hot end or hot bed is in a safe state of touchability.

#### Temperature (Temperature)​

**Effect Rate:** 20 cold junction temperature (°C) **Cutoff:** 80 hot-end temperature (°C) **Palette:** Color values mixed from cold to hot The temperature of the configured heater determines the color of the gradient display on the palette. When only one color is defined in the palette, the brightness of that color is determined by the temperature.

#### The Fire (Fire)​

**Effect Rate:** The probability of a “spark” occurring **Cutoff:** 40 “Cooling” rate **Palette:** Color value from "cold" to "hot" mixed color The Arduino’s FastLED library comes with an example program called Fire2012WithPalette. This effect is a portable version of the program. It simulates the flame by “lighting” an LED. The "heat" from the LED propagates along the LED length and gradually cools. Higher spark rates cause more heat to accumulate at the bottom of the lampband, creating a stronger flame. Changing the cooling rate causes the overall flame to be longer or shorter.

#### Heater Fire (HeaterFire)​

**Effect Rate:** 1 minimum temperature (°C) for activation **Cutoff:** 0 Disabling effect after reaching the target temperature (1= disabled, 0=maintained) **Palette:** Color value from "cold" to "hot" mixed color The flame effect is, but there is a response to the temperature of the specified heater. The flame starts with a small ember and increases the intensity as the heater reaches the target temperature. If the cutoff value parameter is set to 1, the effect will be disabled after reaching the target temperature, otherwise it will remain active until the heater is turned off.

#### Analog pins (AnalogPin)​

**Effect Rate:** 10 multipliers of input signals **Cutoff:** Minimum threshold for 40 triggers (original value or percentage of voltage) **Palette:** Mixed color value This effect uses values read from the analog pin to determine the color. If multiple colors are specified in the palette, a color is selected based on the value of the pin. If only one color is specified, the brightness is proportional to the pin value. An example usage is to connect an analog potentiometer to control the brightness of the LED strip. Internally, the input voltage is measured as a percentage relative to the reference voltage (aref). Another use could be an RPM line that connects the fan (if the fan has a tachometer). It must be used with caution, because the current is too large or the voltage is too high, which can damage the pin or destroy the control board.

#### Stepper motor (Stepper)​

**Effect Rate:** 4 number of tail LEDs (negative value indicates all LEDs before the fill step position) **Cutoff:** 4 Number of head LEDs (negative value represents all LEDs after the filling step position) **Palette:** Mixed color value The position of the specified stepper motor is represented by the first color in the palette. The remaining colors in the gradient are mixed and mirrored on both sides. As the position of the stepper motor changes relative to the length of the shaft, the light moves up and down the lamp belt. It should be noted that the effect itself updates the stepper motor position every half second (based on the reported step position, similar to the GET\_POSITION gcode command). It is not real-time. The negative value in the effect rate fills the entire strip before the step position, and the negative value in the cutoff value fills the entire strip after the step position.

#### The color (stepper color)​

**Effect Rate:** 1 position scaling factor **Cutoff:** 0 position offset **Palette:** Mixed color value The color of the LED is determined by the position of the stepper motor. The position is mapped to 0 to 100, and then multiplied by the effect rate and the cutoff value is added as the offset. This result determines the value taken in the palette as a specified color value gradient.

#### Progress (progress)​

**Effect Rate:** 4 Number of tail LEDs (negative value indicates all LEDs before the fill progress position) **Cutoff:** 4 Number of head LEDs (negative value represents all LEDs after filling progress position) **Palette:** Mixed color value The configuration is exactly the same as the Stepper motor, but this layer reports the printing progress rather than the stepper motor position.

#### Return to (Homing)​

**Effect rate:** 1 Determines the attenuation rate\*\*.\*\* The larger the value, the slower the attenuation (fading out time) **Cutoff:** 0 not used, but must be provided **Palette:** Color loops in order During the return process, when the limit switch is triggered, the LED lights up and fades out again. The effect rate determines the time required to fade out. If a palette containing multiple colors is provided, it loops through these colors in the order specified in the palette.

## Effect Layer Blending (Effect Layer Blending)​

If you've used image editing software, you're probably familiar with color mixing between image layers. Several common color mixing techniques have been added for mixing LED layers. The layers defined in the configuration are sorted from top to bottom (the bottom layer in the configuration file is the "bottom" layer, and the "top" layer is on top).

If you define three layers, first mix the bottom layer with the middle layer. Then mix the result layer with the top layer. Even if the mixing mode is specified, the bottom layer is never mixed with any layer (it is the base).

Layer mixing is always evaluated from the bottom up.

Since values cannot exceed 100% brightness or 0% darkness, they are limited to floating point numbers (0.0 - 1.0) in this range.

#### The Bottom (bottom)​

No mixing, use the color channel value of the bottom layer. `(b)`(Color at the bottom)

#### The Top (top)​

No mixing, use the color channel value of the top layer. `(t)`(Top color)

#### Added (add)​

```
    ( t + b ) # 顶部层颜色 + 底部层颜色
```

Color channels (red, green, blue) add to each other. This causes the passage to become brighter.

#### Subtracted (subtracted)​

```
    ( b - t ) # 底部层颜色 - 顶部层颜色
```

Subtract the top layer from the bottom layer. This will cause similar colors to darken.

#### Reverse subtraction (subtract\_b)​

```
    ( t - b ) # 顶部层颜色 - 底部层颜色
```

Subtract the bottom layer from the top layer. This will cause similar colors to darken.

#### Difference (difference)​

```
    abs( t - b ) # |顶部层颜色 - 底部层颜色|
```

The brighter in the two channels subtract the darker.`( |t - b| )`

#### Average (average)​

```
    ( t + b ) / 2 # (顶部层颜色 + 底部层颜色) / 2
```

Take the average of the channel.

#### Multiply (multiply)​

```
    ( t * b ) # 顶部层颜色 * 底部层颜色
```

The channel multiplies, which helps to dim the color.

#### Separated (divide)​

```
    ( t / b ) # 顶部层颜色 / 底部层颜色
```

The channel divides, which brightens the color, usually white.

#### Reverse phase division (divide\_inv)​

```
    ( b / t ) # 底部层颜色 / 顶部层颜色
```

Similar to division, but divided by the bottom.

#### Filtered (screen)​

```
    1 - ( 1 - t ) * ( 1 - b ) # 1 - (1 - 顶部层颜色) * (1 - 底部层颜色)
```

Take the value in reverse, multiply it, and then reverse. Similar to diageting, it produces brighter colors.

#### Lighten (lighten)​

```
    max( t, b ) # 取每个通道 t 和 b 的最大值
```

Use the brighter one in the color channel.`( max(t, b) )`

#### It's Dark (Darkness)​

```
    min( t, b ) # 取每个通道 t 和 b 的最小值
```

In contrast to brightening, use the darker one in the color channel.`( min(t, b) )`

#### Overlay (overlay)​

```
    ( 2*t*b if t > .5 else 1 - 2*(1-t)*(1-b) ) # 条件混合
```

Overlay is a combination of multiply and screen. This has a similar effect of increasing contrast.

# Examples of (Sample Configurations)

## Fault Warning Lamps (das Blinkenlights)​

In the event of a serious error, all LED lights are synchronously breathed red to provide visible indication that the printer is in an error condition. This effect is disabled during normal operation and starts only when the MCU goes into a closed state (not currently supported).

```
[led_effect critical_error]
leds:
    neopixel:tool_lights
    neopixel:bed_lights
layers:
    strobe         1  1.5   add        (1.0,  1.0, 1.0) # 频闪 白色
    breathing      2  0     difference (0.95, 0.0, 0.0) # 呼吸 红色 (与下方差值混合产生脉动)
    static         1  0     top        (1.0,  0.0, 0.0) # 静态 纯红 (基础)
autostart:                             false             # 不自动启动
frame_rate:                            24                # 24 FPS
run_on_error:                          true              # (计划特性: 出错时运行)
```

## Bed Idle with Temperature (Bed Idle with Temperature)​

```
[led_effect bed_effects]
leds:
    neopixel:bed_lights                 # 应用在热床灯带
autostart:                          true
frame_rate:                         24
heater:                             heater_bed         # 关联热床加热器
layers:
    heater  50 0 add    (1,1,0),(1,0,0) # 加热器效果: 激活温度>50℃, 渐变从黄(1,1,0)到红(1,0,0)
    static  0  0 top    (1,0,0)         # 静态红色层 (顶部, 当达到目标温度时显示纯红)
```

## Brightness Controlled by Potentiometer​

This is an example of how analog pin (Analog Pin) effects are mixed with layers to control the brightness of the light. The potentiometer can be connected to the thermistor port and the voltage values read from that pin can be used to determine the amount of color subtracted from the base layer. The wiring of the potentiometer needs to ensure that when it is "down", the voltage on the analog pin is full output (i.e., the potentiometer grounding analog pin), and when it is "highened", the voltage is the minimum output (i.e., the potentiometer VCC termination analog pin). In this way, when the potentiometer is "turned down", the color (1.0, 1.0, 1.0) (pure white) is subtracted from the color of the layer, resulting in (0.0, 0.0, 0.0) (all black). The effect rate and cutoff value need to be adjusted according to the specific potentiometer and board combination.

```
[led_effect bed_effects]
leds:
    neopixel:bed_lights
autostart:                          true
frame_rate:                         24
analog_pin:                         ar52                # 模拟输入引脚
layers:
    analogpin 10 0 subtract    (1,1,1) # 模拟引脚效果: 乘数10 阈值0 相减混合模式 白色(1,1,1) (值越大, 减的白色越多, 越暗)
    static    0  0 top         (1,1,1) # 静态 纯白层 (基础亮度)
```

## Posts Tagged ‘Progress Bar’​

Using a single LED strip, the print progress is displayed as a light blue line on a dark blue background.

```
[led_effect progress_bar]
leds:
    neopixel:progress_lights          # 进度条灯带
autostart:                          true
frame_rate:                         24
layers:
    progress  -1  0 add         ( 0, 0,   1),( 0, 0.1, 0.6) # 进度效果: 负值填充左侧, 添加混合, 从深蓝(0,0,1)到中蓝(0,0.1,0.6)渐变
    static     0  0 top         ( 0, 0, 0.1)                  # 静态 暗蓝色背景层 (顶部)
```

# Frequently Asked Questions (Frequently Asked Questions)

## My LEDs are flashing randomly. ​

This is usually due to some sort of signal problem. Most programmable LEDs use specific communication protocols. It usually involves sending data bits at specific intervals, followed by reset latch signals to notify them to light up. They will keep the color of the last setting until they are told to do something else.

In most programmable LED schemes implemented in the printer firmware, the color data is sent once when the gcode command is executed, and then no longer. As long as the initial signals are read, they will retain that color.

In this specific implementation, the color data is updated at a fixed interval determined by the effect frame rate. Thus, 10 frames per second means sending 10 color updates to the LED per second.

Data lines are susceptible to electromagnetic interference from other electronic devices on the printer. When this interference exists, it may cause an error in the format of the data sent to the LED.

To alleviate this, attempts can be made to insulate the data lines or isolate them from other wires. Keep the data line as short as possible. This is particularly problematic for 32-bit motherboards that typically output data signals at 3.3V.

Ringing and reflection on the data line also reduces signal integrity, especially when the cable connected to the first LED is quite long. This can be reduced by connecting a 470-1000 ohm (usually about 700 ohms) resistance on the data line in front of the first LED.

Another source of flashing is pressure drop. Depending on the brightness of the settings, each programmable LED consumes 20 to 60mA of power. If they run on a power supply that cannot provide sufficient power (such as the internal regulator of the printer’s motherboard), it may appear as a flashing or a overall dimmer of the lamp band.

## The LED at the end of my lamp is not as bright as the other parts. ​

This is usually related to the lack of power obtained by the LED at the beginning of the lamp belt compared to the LED at the end of the lamp. The solution is to weld the VCC and GND lines at the end of the lamp belt. These additional power cords will enable the end LEDs to get the power they need. This usually only happens when the light belt is very long, or the power supply has reached its limit. It is always recommended to use a motherboard-independent 5V power supply to power the LED.

## My color is incorrect. ​

Different chipmakers and chip types use slightly different color data protocols. Some specify color order as red, green, blue, and others as green, red, and blue. The configuration of the LED strip has an optional parameter in the 'neopixel' section to set the color order.

`color_order: GRB`(For example, if the strip is actually in the GRB order, but the RGB data is sent as green)

If you are unsure of the color order of the LED and want to test it, you can annotate or disable all the configured effects and use the gcode command to set the color of the lamp belt directly.

`SET_LED LED=<config_name> RED=1 GREEN=0 BLUE=0 TRANSMIT=1`(For example, `SET_LED LED=panel_ring RED=1 GREEN=0 BLUE=0 TRANSMIT=1`)

This command should change the entire strip to red. If the strip changes green, it uses the GRB color order.

Some LEDs, such as SK6812, also have a white channel. For these LEDs, the color order can be set to: `color_order: GRBW`

*   Note: For FLY's TP (possibly brand-specific/model) drives, they need to be configured as `colorreorder: GRB`