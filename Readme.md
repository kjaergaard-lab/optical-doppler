# Translation Stage for the optical Doppler effect

Originally this experiment used a stepper motor, which has substantial vibration at lower speeds. The documentation for this version is in the `OLDStepperMotorVariant`. 

The details of the servo motor setup is in the `StepperServoConversion` folder.

The current code is in `pio-grbl`. This device runs a modified version of the GRBL CNC control software, and the folder here is setup to use PlatformIO to manage compiling and uploading.

In config.h, the line
`#define CUSTOM_VARIABLE_SPEED_CYCLE`
enables code which:

- Performs a homing cycle at reset (i.e. runs the stage into the limit switch so it knows where the end of the rail is)
- The moves back and forth along the rail
- While reading the voltage on the potentiometer in order to scale the motion speed.

If this line is commented out, then the result is standard [GRBL](https://github.com/gnea/grbl/wiki) behaviour (with only an x axis). This means that nothing will happen on reset, and it will wait for G code commands on the USB serial interface (115200 baud).

As well as the settings in config.h, [GRBL has settings which may be updated without programming the device.](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Configuration) See [Config.md](Config.md) for more details.

## Schematic

Electronic schematic of the controller is in [StepperServoConversion/ServoGrblBoard](StepperServoConversion/ServoGrblBoard)

## Troubleshooting

#### Device does nothing at startup

- Confirm that Emergency Stop button is not depressed
- Check that the power switch at the rear of the unit is turned on
- Check that power is actually being supplied
- Make sure that the limit switch on the rail isn't already being pressed.

If all of these are fine, open the lid of the device (WARNING: MAINS VOLTAGES INSIDE), and confirm that the red LED display on the servo controller (located near the emergency stop switch) is lit, and that the arduino unit also has a green power LED lit.

- Connect a USB C cable to the arduino (CH340 USB-serial converter), and connect to the serial port with 115200 baud. The arduino should reset on connect, and one can note any errors that may be reported.
- Check that the GRBL configuration is correct (request config by sending `$$`), especially the speed ($110). See [Config.md](Config.md) for more details.

 
#### The device perform a homing cycle (runs the stage into the limit switch) and then does nothing

 - This implies that everything is powered, and that the arduino running GRBL is communicating effectively with the servo controller
 - If you can still hear the whine of the servo controller after homing, then GRBL still thinks it is moving the stage
    - Ensure that the speed potentiometer isn't set to 0
    - Take the top of the unit off (WARNING: MAINS VOLTAGES INSIDE), and confirm that the voltage at A0 on the arduino is non-zero
    -  Connect a USB C cable to the arduino (CH340 USB-serial converter), and connect to the serial port with 115200 baud. Check that the GRBL configuration is correct (request config by sending `$$`), especially the speed ($110). See [Config.md](Config.md) for more details.
 - If you can't hear the whine of the motor, GRBL has decided to stop controlling the motor, presumably due to an error condition
    - Connect a USB C cable to the arduino (CH340 USB-serial converter), and connect to the serial port with 115200 baud. The arduino should reset on connect, and one can note any errors that may be reported.


#### The speed of the stage isn't consistent / smooth

 - Check the potentiometer connections and health. Consider replacing, and/or adding mor filtering.

 
#### The stage runs into the end
   *YOU SHOULD IMMEDIATELY HIT THE EMERGENCY STOP BUTTON IF THE DEVICE DOESN'T IMMEDIATELY STOP ITSELF* 

 - Manually move the stage off the limit switches (most easily by twisting the lead screw near the motor where there isn't any grease), and reset the controller while watching in case it happens again.
 - If it happens during the intial homing stage (and runs into the switch), check the limit switch connection, which has possibly somehow failed short.
 - Consider reprogramming and reconfiguring the internal arduino as above.

 
