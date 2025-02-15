---
laout: page
title: Robotic Mouse Hardware Platform

permalink: robotic-mouse-hardware

tag: robotic mouse
---

# Microcontroller

For prototyping I have chosen a [XIAO Seeed ESP32C3](https://wiki.seeedstudio.com/XIAO_ESP32C3_Getting_Started/), because I already own it and it has just enough GPIOs to get started.
Additionally, it supports I2C which is the main protocol I need to communicate with on- and off-board sensors.

The choice of an ESP based development board enables the option of replacing it with the on-board ESP module.
There are lots of options available for this, including the two core ESP32S3 module.
Other options like the ESP32S2 expand the GPIO capabilities in case more sensors are required.


# IMU

The inertial measurement unit is placed on the PCB.
It helps with discerning pose of the robot, it serves the same purpose as the inner ear does for humans.
Using this is more precise and versatile than counting the steps of the motor encoders and calculating the angle from the distance each wheel travelled.


The [MPU-6050](https://invensense.tdk.com/wp-content/uploads/2015/02/MPU-6000-Datasheet1.pdf) IC was chosen as the IMU, combining a gyroscope and an accelerometer.
This IC is can be controller using the common I2C protocol which is widely supported and including the ESP32C3.

![IMU](media/MicroMouse-sensing_module.svg)

# Infrared Time of Flight distance sensor

For distance sensing an I2C off-board [module](https://thepihut.com/products/tf-luna-lidar-ranging-sensor) is used.
This is an IR based sensor which may or may not cause issues when other robots also use the same IR based sensor.
This issue might be solved by using IR LEDs and IR phototransistors, that only work with a specific wavelength of light.
However, this is no guarantee for avoiding interference intentional or not.


# Motors & encoders

The motors I'm using are [Micro Metal Motor](https://thepihut.com/products/micro-metal-gearmotor-with-motor-connector-shim-mcs) which are driven at 6V.
Furthermore, the shims are swapped for [shims](https://thepihut.com/products/micro-metal-motor-encoder-mmme-pack-of-2) with hall effect sensors for encoders.
This combined with the 50:1 gear ratio of the drive train attached to the motor allows for encoding one revolution of the wheel to as 50 pulses registered by the encoders.

For odometry the fact that 50 hall effect sensor pulses equal 1 full rotation of the wheel is used.
With this the distance travelled can be calculated, and the position can be estimated.
For this the angle given by the IMU needs to be taken into account.
In a pinch the angle can be calculated from the distance travelled by each wheel, but there is higher uncertainty in this than using a dedicated IMU.

![motor dirver](media/MicroMouse-motor_module.svg)

The motor driver used is a standard [L293D](https://www.ti.com/product/L293D) with [NAND gates](https://www.ti.com/lit/ds/symlink/sn7400.pdf?ts=1731168774987&ref_url=https%253A%252F%252Fwww.mouser.fr%252F) between the inputs and the EN pins of the driver.
This enables combinations of the inputs to be used to enable/disable the motor driver, which can be used for free running stop and slow breaking by the driver.


| A | B | EN (A NAND B)| Functionality |
|---|---|-----|---------------|
| 1 | 1 | 0 | Free-running motor stop |
| 0 | 1 | 1 | Turn right |
| 1 | 0 | 1 | Turn left |
| 0 | 0 | 1 | Fast motor stop |

This table shows how the motor is turning bepending on A and B.
Using the NAND gate is a cheese for using the available inputs for more functionality and reduces the number of GPIO pins needed.

# Power delivery

A LiPo battery is used to power the whole system.
It can be charged by plugging in a power brick into the on-board USB-C port.
This necessitates a LiPo charging IC and an under/overcharge protection IC.

The one cell LiPo battery delivers 3.7V, while the DC motors used need 6V a boost converter is needed.
The motors max current being 1A when they stall requires the splitting into high and low power ground where everything that is connected to the 6V line should be connected to high power ground.
All the 3.3V circuitry is on its own ground connected to the battery and MCU ground.
Another important thing to keep in mind is that the battery needs to be able supply the max current the system (in this case mostly the motor) uses.

![power delivery circuit](media/MicroMouse-power_module.svg)

For the boost converter I have coloured the different nodes to make the component placement easier.
Since this is a switching keeping the feedback and switching loop as small as possible is really important.
Shout out to [Phil's Lab](https://www.youtube.com/watch?v=1g-D8T65SJU) for making a video on boost converter layout and component sizing, I would have struggled a lot more without these videos.

To avoid dealing with charging and powering the robot at the same time I've added a p-channel MOSFET and a On/Off switch in series.
The MOSFET automatically disconnects the boost converter from the battery when it is charged.
Additionally, the switch enables turing the robot on and off.

# PCB

KiCAD is the software I used to design the circuit and the PCB, the footprints and part numbers were gathered from available parts on mouser and RS.
The PCB is manufactured and assembled by PCBWay.
To allow for debugging test pads and large (0805) components were chosen, this makes it easier to debug the system.
Access via test pads is given for the power outputs and the I2C paths.

![micromouse PCB](media/MicroMouse_front.png)

It is important to note that this is the first iteration of the board and the expectation is to figure out kinks, what works and what doesn't in order to fabricate a better successive version.
I'm not expecting it to work, but to have to solder some wires for hardware debugging.

# Future improvements

To minimize the size of the PCB and consequently the robot removing the requirement of a boost converter and a battery charging module would help substantially.
This is due to the fact that roughly a third of the ICs on the PCB are dedicated to power delivery.
Since a LiPo Battery is used, a dis- & overcharge protection IC might still be necessary.
However, separating the battery charging circuit from the board would be a major footprint improvement.

Replacing the IR ToF distance sensing module with sets of IR LEDs and Phototransistors should be considered.
This would simplify the programming since it eliminates an I2C device and replaces it with analog inputs from the phototransistors.
The downside of this approach is the need for additional GPIO pins for the IR LED triggering and transistor inputs.
An example design of this is given on the [UKMARS](https://ukmars.org/projects/ukmarsbot/reference-guide/ukmarsbot-wall-sensor-basic/) website.

## Version 2

There are already some improvements I want to make for the second version.

# MCU

Instead of using a dev board that contains the MCU I want to mount the MCU directly on the board.
Additionally, an upgrade for the MCU is needed to account for more needed GPIO pins.
I will still stay with the Espressif family of chips, but an ESP32S3 will be better suited then the currenct ESP32C3.
I'm not sure yet if I want the PCB antenna or not.


# Miniturisation

The [rules](https://ukmars.org/contests/contest-rules/micromouse-classic/) for the full sized Micromouse contest state that the size of each cell should be 16,8 x 16,8 cm^2 inside the walls.
This gives a spacing of the edges of the diagonals at 11,8 cm for the full sized competition.
If the mouse is wider than that it won't be able to cut straight through the zig-zag sections, this is an optimisation and not the first priority.
However, it is nice to have the option without having to redesign the hardware.

# Updating Sensing Modules

I will change from using a range finder to 3 sets of IR-LEDs and IR-Phototransistors.
Cross-talk is still an issue with this setup.


# JTAG port

With the MCU directly mounted on the PCB, a different way of programming it needs to be found.
This way is the JTAG (Joint Tag Action Group) interface, it's a very common connector in industry, therefore it'd be good to know how to use it.
Additionally, if I can avoid putting a USB port on the board I will.
The benefit of begin able to debug it as it is running is also a nice plus.
