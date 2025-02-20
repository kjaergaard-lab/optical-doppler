# GRBL configuration

The configuration loaded into grbl can be listed by sending the `$$` command. Note that the stage will try to home before it will respond to serial commands, so if you have the lid off the stage controller, you probably don't want to be powering the device with mains. The arduino can be powered by the USB C cable. Connect to the arduino serial (115200 baud rate), and the GRBL welcome should pop up. After waiting a second, tap the limit switches on the stage twice to trick it into thinking that it has homed successfully, and it should respond to commands. 

The configuration loaded initially is as follows.

```
$0=10
$1=25
$2=0
$3=0
$4=0
$5=1
$6=0
$10=1
$11=0.010
$12=0.002
$13=0
$20=0
$21=1
$22=1
$23=1
$24=25.000
$25=500.000
$26=250
$27=1.000
$30=1000
$31=0
$32=0
$100=1000.000
$101=250.000
$102=250.000
$110=2000.000
$111=500.000
$112=500.000
$120=200.000
$121=10.000
$122=10.000
$130=235.000
$131=1.000
$132=1.000
```
