---
layout: page
title: Robotic Mouse Project Outline

permalink: robotic-mouse-outline
---

# Timeline

| Status  |  End Date | Description  | 
|---------|-----------|--------------|
| Complete | Oct 2024  | Design and order the first prototype PCB |
|    | Nov 2024 | Debugging of PCB |
| In Progress   | Nov 2024 | CAD design of mouse |
|    | Dec 2024 | Programming robot |
|    | Jan 2025 | Uni club competition |

# What is Micromouse

Micromouse is a robotics contest with the aim of constructing a mouse (robot) that can solve a maze in the fastest time possible.
It was called into existence be the IEEE in the late 1970s.
Over the years the competition has become more advanced with unconnected sections to make wall following an unsuccessful strategy.
The classic [rules](https://ukmars.org/contests/contest-rules/micromouse-classic/) for some basic guidance on measurements and dimensions
of the maze and the robot.
The UK Micromouse and Robotics Society [UKMARS](https://ukmars.org/) offers a getting started page and a comprehensive list of parts distributors.

# Project concept

The aim is to build a cheap(ish) robotic platform inspired by the robots used in the Micromouse competition.
This mouse should be affordable enough for others to reproduce, given the files.
Therefore, the project source will be made available for others to clone, modify and experiment with.
However, contributions from others to the repository are not intended.

# Project constraints

- Rust based

The main constraint for this project is the use of Rust as the primary programming language, mainly so I can evaluate if I like Rust for embedded programming and to explore the available infrastructure.
I may know C better, but tripping and falling over my feet as I figure out embedded Rust is the main goal.

- ESP32 based

Because of the available [documentation](https://github.com/esp-rs) for the Espressif platform and the ability to use C or Rust the ESP32 family of 
microcontrollers was chosen.
The availability for SoCs with wireless capabilities on board was a big plus.

# Primary goals
- Construct a robotic mouse
- Adapt the robot to play tag with other robots
    - Chasing mode
    - Fleeing mode


# Secondary goals
- Make the robot solve a maze
- Localisation
    - Implement odometry
    - Add feedback through measurement
- Mapping
    - Build an internal representation of the maze

