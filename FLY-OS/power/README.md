## Power-off Shutdown and Power-loss Resume

> Important Notice
>
> * **Power-off shutdown** and**power-loss resume** functions conflict with each other and cannot be enabled simultaneously
> * If only the power-off shutdown function is enabled, the power-loss resume function will be unavailable
> * Ensure correct configuration and maintain Klipper connection for the power-loss resume function to work properly. After this function is triggered, the system will automatically enter the shutdown process

## Power-loss Resume

### Disabling Power-off Shutdown Function

> Notice
>
> * You need to disable the power-off shutdown function, otherwise the power-loss resume function will not work
> * The power-loss resume function includes an auto-shutdown feature after saving progress
> * Ensure no other power supply is connected to the host computer, otherwise normal shutdown will not work

1. **Access the device configuration page**

   * Enter the device IP address in the browser address bar, for example:`http://192.168.1.2/`
2. **Show hidden files**

   * **Fluidd** : Uncheck "Filter hidden files and folders"
   * **Mainsail** : Check "Show hidden files"


   | ![](config1.webp) | ![](config2.webp) |
   | ------------------- | ------------------- |
3. **Edit configuration file**

   * Locate and enter the`.flyos-config` folder
   * Open the`sys-config.conf` file

   ![](config3.webp)
4. **Comment out power-off shutdown configuration**

   * Locate the`shutdown_pin_state` and`shutdown_pin` configuration items
   * Add`#` in front of these two configurations to comment them out
5. **Save and restart**

   * Save the modified configuration file
   * Close the file and restart the system

   ![](kppm.webp)

### Configuring Power-loss Resume Function

1. **Edit the plr.cfg configuration file**

   * On the printer configuration page, find or create the`plr.cfg` file
   * Clear the file content and paste the following configuration:
   * Please modify the`power_pin` parameter according to the actual GPIO number used

   ```marlin
   [mcu host]   
   serial: /tmp/klipper_host_mcu

   [power_loss_resume]
   power_pin: xxxx
   is_shutdown: True # Whether to perform shutdown operation, default is enabled
   paused_recover_z: -2.0 # If printing is paused when power loss occurs, this sets the Z-axis movement distance before resuming, default is no movement
   start_gcode:
      # Gcode executed before resuming print
      # All parameters saved before power loss can be retrieved using {PLR}
      # M118 {PLR} can be used to output all available parameters
      # M118 {PLR}
      M118 Resuming print: {PLR.print_stats.filename}
      M118 Interruption position: X:[{PLR.POS_X}] Y:[{PLR.POS_Y}] Z:[{PLR.POS_Z}] E:[{PLR.POS_E}]
      {% if PLR.bed.target > 0 %}
         M140 S{PLR.bed.target}      ; Set heated bed temperature
      {% endif %}
      {% if PLR.extruder.target > 50 %}
         M104 S{PLR.extruder.target - 10}  ; Wait for extruder to heat to set temperature
      {% endif %}
      G91                             ; Relative coordinates
      G1 Z2 F100                      ; Raise Z, prepare for X and Y homing
      G90                             ; Absolute coordinates
      G28 X Y                         ; Home XY
      {% if PLR.bed.target > 0 %}
         M190 S{PLR.bed.target}      ; Wait for heated bed to reach set temperature
      {% endif %}
      {% if PLR.extruder.target > 0 %}
         M109 S{PLR.extruder.target}  ; Wait for extruder to reach set temperature
      {% endif %}
      M83                              ; Relative extrusion
      # G1 E0.5 F400                   ; Extrude a little
   layer_count: 2 # Execute layer_change_gcode after resuming print for {layer_count} layers
   layer_change_gcode:
      # Gcode executed after resuming print for {layer_count} layers
      M118 Resume printing speed
      M106 S{PLR.fan_speed}             ; Turn on part cooling fan
      M220 S{PLR.move_speed_percent}    ; Set requested movement speed percentage
      M221 S{PLR.extrude_speed_percent} ; Set requested extrusion speed percentage
   shutdown_gcode:
      # Gcode executed before shutdown
      M118 Low power voltage, shutting down
      # M112 ; Emergency stop
   ```
2. **Include configuration file**

   * Open the`printer.cfg` file and add the following at the beginning:

   ```marlin
   [include plr.cfg]
   ```

   * Click save and restart in the upper right corner

### Configuring Homing Override

> Important Notes
>
> * If`[homing_override]` is used, do not arbitrarily set the homing position in the configuration
> * Incorrect configuration may cause power-loss resume to fail

**Configuration Description**

* `[force_move]`: Enables the force move function, allowing forced movement to a specified position
* `[force_move]` replaces the`set_position_z` function in`[homing_override]`
* The following configuration ensures correct Z-axis homing during power-loss resume

```marlin
[force_move]
enable_force_move: true

[homing_override]
axes: z
gcode:
   {% set max_x = printer.configfile.config["stepper_x"]["position_max"]|float %}
   {% set max_y = printer.configfile.config["stepper_y"]["position_max"]|float %}
   {% if 'z' not in printer.toolhead.homed_axes %}
      SET_KINEMATIC_POSITION Z=0
      G90
      G0 Z5 F600
   {% endif %}
   {% set home_all = 'X' not in params and 'Y' not in params and 'Z' not in params %}

   {% if home_all or 'X' in params %}
   G28 X
   {% endif %}

   {% if home_all or 'Y' in params %}
   G28 Y
   {% endif %}

   {% if home_all or 'Z' in params %}
   G0 X{max_x / 2} Y{max_y / 2} F3600 
   G28 Z
   G1 Z10 F2000
   {% endif %}
```

**Z-axis Lift Description** This configuration only executes when the Z-axis is not homed and does not affect normal use:

```marlin
{% if 'z' not in printer.toolhead.homed_axes %}
   SET_KINEMATIC_POSITION Z=0
   G90
   G0 Z5 F600
{% endif %}
```

### Function Testing

#### Step 1: Simulate Power Loss Test

1. Start printing any file
2. During printing, click the**Emergency Stop** button to simulate a power loss situation
3. Click**Firmware Restart** and wait for Klipper to reconnect
4. Check if a pop-up appears on the web interface (if it does, the function is working properly)
5. Subsequently, perform a real power loss test to verify the reliability of the function

#### Step 2: Real Power Loss Test

1. **Test Preparation** : Confirm the LED indicator status next to the host computer (LED should be blinking during normal operation)
2. **Power Loss Test** : Disconnect the power supply directly while the device is running normally
3. **LED Check** : Observe whether the LED completely turns off within**5 seconds**
   * **Turns Off** : Power-off shutdown function works normally
   * **Does Not Turn Off** : Auto-shutdown function did not activate; check configuration
4. **Recovery Test** : Wait at least 5 seconds before restoring power
5. **Function Verification** :
   * **Popup Appears** : Power-loss resume function works normally
   * **No Prompt** : Power-loss resume function did not activate; check configuration

---

## Configuring Power-off Shutdown

Notice

Please follow the steps below to configure the power-off shutdown function

1. **Access the device configuration page**

   * Enter the device IP address in the browser address bar, for example:`http://192.168.1.2/`
2. **Show hidden files**

   * **Fluidd** : Uncheck "Filter hidden files and folders"
   * **Mainsail** : Check "Show hidden files"


   | ![](config1.webp) | ![](config2.webp) |
   | ----------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
3. **Edit configuration file**

   * Locate and enter the`.flyos-config` folder
   * Open the`sys-config.conf` file (this file is a soft link to`config.txt` in the`FlyOS-Conf` disk)

   ![](config3.webp)
4. **Modify configuration parameters**

   * Locate the`shutdown_pin_state` and`shutdown_pin` configuration items
   * Modify them to the following settings:

   ```plaintext
   shutdown_pin_state=1
   shutdown_pin=xxxxx
   ```
   * Ensure`shutdown_pin=none` is deleted or commented out (add`#` in front)
   * Here,`xxxxx` is the actual GPIO number used; choose the correct GPIO according to your device model:
5. **Save and restart**

   * Save the modified configuration file
   * Close the file and restart the system

   ![](kppm.webp)
