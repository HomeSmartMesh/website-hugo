---
title: "Rover Balance"
description: "Open source home robot with a double pendulum balance. Targetting the ability to stand up switching from flat mode (car) to the standing mode (balancing robot), on each of its sides. Further dynamic behaviors expected."
date: 2019-02-10T08:00:00+00:00
lastmod: 2021-03-07T08:00:00+00:00
draft: false
images: ["/images/robotics/rover_concept.png" ]
weight: 1
toc: true
---
{{<load-svg-pan-zoom>}}

# Overview
{{<icon_button text="RobLibs Github Organization" href="https://github.com/Roblibs" icon="github" >}}

{{<image src="/images/robotics/rover_concept.png" >}}

* Open source hobby project, creation of a robot with an original structure that moves in a home environment.
* The robot structure is a double sided inverted pendulum, with the ability to stand up switching from flat mode (car) to the standing mode (balancing robot), on each of its sides. Further dynamic behaviors to be expected. 

# Concept

* A gazebo simulation approximates the robot's physics and allows the user to control it with a joystick to have a feeling of the robot's power depending on the weight and motors power.
* nRF52840 Allows high throughput log with native usb uspport.
* A custom RF protocol allows 1 ms order of magnitude for mesh network communication between the different robots part.
* The Programmable peripheral Interconnect feature of the nRF family is leveraged to increase real time responsiveness of events. This is like having a small FPGA interconnect inside of a powerful micro controller.
* C++ drivers wrappers and Standard library support.

# Requirements
* The robot shall have a body and 4 limbs.
* Each limb shall have a motorized joint connecting it to the body.
* Each limb shall have a motorized wheel.
* In flat mode, the robot shall be able to move like a tank.
* The robot shall be able to go from flat mode to standing mode.
* The robot shall be able to keep balance and move in standing mode.
* All articulations shall be reversible, to allow force control loop (smooth interactive reaction).

# Simulation
* intro video to the concept, Joystick controlled to check the possibilities of a multi legged wheeled robot.
{{<youtube 1Tv4VYB-lmo>}}

# BLDC control
* This BLDC control, although running STM32F103C8T6 is a showcase for combined speed and position control of a BLDC motor, the Raspberry pi is only used as a PS4 joystick interface, the STM32 is controlling an L6234D triple H bridge module.

{{<youtube Z7JaMNv-Two>}}


# Repository structure

Gray boxes are git repositories / submodules with html links



{{<svg-pan-zoom "/design/rover_repo_structure.svg" >}}

# FAQ - Discussion
If you need support, want to ask a question or suggest an idea, you can join the discussion on the forum
{{<icon_button text="Home Smart Mesh - Discussions" href="https://github.com/HomeSmartMesh/raspi/discussions" icon="github" >}}

{{<faq>}}
Is this robot ready ?
<--->
No, this project started in 2019 is currently in slow progress mode and only have concept and simulation. The electronic design is still in progress.
<===>
Why not use existing BLDC drivers such as Flipsky ?
<--->
Flipsky is overdimensioned in size and in power for driving a small robot.
{{</faq>}}
