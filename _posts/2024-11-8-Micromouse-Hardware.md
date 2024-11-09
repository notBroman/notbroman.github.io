---
laout: page
title: Robotic Mouse Hardware Platform

permalink: robotic-mouse-hardware
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

# Power delivery

A LiPo battery is used to power the whole system.
It can be charged by plugging in a power brick into the on-board USB-C port.
This necessitates a LiPo charging IC and an under/overcharge protection IC.

The one cell LiPo battery delivers 3.7V, while the DC motors used need 6V a boost converter is needed.
The motors max current being 1A when they stall requires the splitting into high and low power ground where everything that is connected to the 6V line should be connected to high power ground.
All the 3.3V circuitry is on its own ground connected to the battery and MCU ground.

![power delivery circuit](media/MicroMouse-power_module.svg)

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

