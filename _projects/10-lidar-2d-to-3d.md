---
layout: project
title: 3D scanning head with 2D lidar
category: project
thumbnails:
  - /assets/projects/lidar-2d-to-3d/ezgif-3-d5575b524c23.gif
  - /assets/projects/lidar-2d-to-3d/thumbnail.bmp
description: I built a DIY 3D scanning system that converts a 2D lidar into a 3D scanner using a rotating servo mechanism. The project includes custom firmware for AVR and STM32 microcontrollers, real-time visualization tools, and a complete software stack for processing and synchronizing lidar data. Features include IMU integration for improved accuracy and movement tracking.
stack:
  - C++
  - Go
  - Embedded C
  - RPlidar
  - Atmega328p
  - MPU-6050 / MPU-9250
  - STM32
source_code: https://github.com/dsonyy/lidar-2d-to-3d
clickable: true
tags:
  - 10k lines of code
  - Robotics
  - 3D scanning
---
The goal of this project was to create a device that can scan its surroundings in 3D using a 2D lidar.

![ezgif-3-d5575b524c23](/assets/projects/lidar-2d-to-3d/ezgif-3-d5575b524c23.gif)


- [Gallery](#gallery)
  - [3D](#3d)
  - [2D](#2d)
- [Submodules](#submodules)
  - [lidar-tools\*](#lidar-tools)
    - [sync\*](#sync)
    - [servoctl](#servoctl)
    - [transmitter/receiver](#transmitterreceiver)
    - [scan-dummy](#scan-dummy)
  - [lidar-avr\*](#lidar-avr)
  - [lidar-scan\*](#lidar-scan)
  - [lidar-vis](#lidar-vis)
  - [lidar-visualizations](#lidar-visualizations)
  - [lidar-stm32](#lidar-stm32)
- [Components](#components)
  - [Lidar](#lidar)
    - [Specification](#specification)
    - [Sources](#sources)
    - [Scanning Modes](#scanning-modes)
    - [Data](#data)
    - [Lidar Motor Speed Measurements](#lidar-motor-speed-measurements)
    - [General Lidar Results Measurements](#general-lidar-results-measurements)
  - [Servo](#servo)
  - [IMU](#imu)
    - [MPU-6050](#mpu-6050)
    - [MPU-9250](#mpu-9250)
    - [Calibration](#calibration)
    - [DMP](#dmp)
  - [Microcontroller](#microcontroller)
  - [Scanning Head](#scanning-head)
    - [Prototype](#prototype)
    - [Final Version](#final-version)
- [3D Scanning](#3d-scanning)
    - [Processing Raw IMU Measurements](#processing-raw-imu-measurements)
- [Configuration](#configuration)
  - [Hardware](#hardware)
  - [Software](#software)
    - [Parameters for _lidar-tools/sync_](#parameters-for-lidar-toolssync)
- [Materials](#materials)
  - [Helper Software](#helper-software)
  - [Point Cloud Examples](#point-cloud-examples)

# Gallery

## 3D

<img src="/assets/projects/lidar-2d-to-3d/komputer-16324113767931.bmp" width=700>

<img src="/assets/projects/lidar-2d-to-3d/ogrod.bmp" width=700>

<img src="/assets/projects/lidar-2d-to-3d/pokoj.bmp" width=700>

<img src="/assets/projects/lidar-2d-to-3d/pracownia.bmp" width=700>

<img src="/assets/projects/lidar-2d-to-3d/capture.bmp" width=700>

## 2D

![](/assets/projects/lidar-2d-to-3d/2d-cars.gif)

![2d-room](/assets/projects/lidar-2d-to-3d/2d-room.gif)

# Submodules

This repository combines several repositories with the prefix **lidar-** developed under [KNEI](https://github.com/knei-knurow) that are related to 3D scanning. You can find more information about each project in their README files.

**The asterisk \* marks repositories that are required for 3D scanning.** The other projects can be useful when working with 2D scanning.

## [lidar-tools\*](https://github.com/knei-knurow/lidar-tools)

A collection of _tools_ useful when working with the lidar.

### sync\*

The most important of the _tools_. Its tasks include:

- Establishing connection with the microcontroller (which controls the servo and accelerometer) and the lidar
- Synchronizing data/measurements from available sources
- Calculating 3D point cloud points
- Printing point by point (x, y, z) to standard output (stdout)

### servoctl

Sends a data frame to the microcontroller to set the servo to a given position.

### transmitter/receiver

Sends/receives 2D lidar data (obtained e.g. using _lidar-scan_) via UDP.

### scan-dummy

Pretends to be _lidar-scan_. Generates sample data.

**Authors:** [Szymon Bednorz](https://github.com/github.com/dsonyy),
[Bartek Pacia](https://github.com/github.com/bartekpacia)

## [lidar-avr\*](https://github.com/knei-knurow/lidar-avr)

Software designed to be flashed onto the AVR Atmega328p microcontroller, which handles the servo and accelerometer and communicates via USART with the operator's device.

More details about the exact configuration and operation can be found in later sections of this document.

**Authors:** [Szymon Bednorz](https://github.com/github.com/dsonyy),
[Bartek Pacia](https://github.com/github.com/bartekpacia)

## [lidar-scan\*](https://github.com/knei-knurow/lidar-scan)

A project derived from _lidar-visualizations._ The idea was to create a program that [does one thing and does it well](https://en.wikipedia.org/wiki/Unix_philosophy).

The program's task is to establish a connection with the lidar and print received data in text form to standard output (stdout). The data can then be passed to any other program that understands the input data format.

Compatible with Linux, macOS, and Windows systems.

**Example output:**

```
# A comment
# ! ID Number   Elapsed time [ms]
# Angle [°]   Distance [mm]
! 0 0
120  100
240  100
360  100
! 1 500
120  200
240  200
360  200
! 2 500
```

**Authors:** [Szymon Bednorz](https://github.com/github.com/dsonyy),
[Bartek Pacia](https://github.com/github.com/bartekpacia)

## [lidar-vis](https://github.com/knei-knurow/lidar-vis)

Graphically displays data received from the lidar using _lidar-scan_.

A project derived from _lidar-visualizations_. It's a "sister" program to _lidar-scan - [does one thing and does it well](https://en.wikipedia.org/wiki/Unix_philosophy).

Compatible with Linux, macOS, and Windows systems.

**Authors:** [Szymon Bednorz](https://github.com/github.com/dsonyy),
[Bartek Pacia](https://github.com/bartekpacia)

## [lidar-visualizations](https://github.com/knei-knurow/lidar-visualizations)

Displays lidar data in text or graphic form in real-time.

The software has changed its form almost completely several times. It was created for an article in "Elektronika Praktyczna". Currently, it's not actively developed. The code consists of several interfaces that theoretically allow for creating additional functionality. The program uses the official RPLIDAR SDK and the high-level SFML graphics library.

Compatible with Linux, macOS, and Windows systems.

More information can be found in the article ["Visualization of LIDAR Scanner Measurements"](https://ep.com.pl/kursy/notatnik-konstruktora/14763-wizualizacja-pomiarow-skanera-lidar) in ["Elektronika Praktyczna"](https://ep.com.pl) from March 2021.

**Author:** [Szymon Bednorz](https://github.com/github.com/dsonyy)

## [lidar-stm32](https://github.com/knei-knurow/lidar-stm32)

An interesting project that, like _lidar-visualizations_, was created for "Elektronika Praktyczna". It runs on an STM32 board, which sends/receives data to/from the lidar via UART and then visualizes it.

The RPLIDAR SDK is not adapted for embedded devices, so an original implementation for STM32 was created based on it.

More information can be found in the article ["Visualization of LIDAR Scanner Measurements"](https://ep.com.pl/kursy/notatnik-konstruktora/14763-wizualizacja-pomiarow-skanera-lidar) in ["Elektronika Praktyczna"](https://ep.com.pl) from March 2021.

**Author:** [Bartek Dudek](https://github.com/github.com/doodek)

# Components

## Lidar

<img src="/assets/projects/lidar-2d-to-3d/a3m1.jpg" width=300>

The lidar used in the project is [Slamtec RPLIDAR A3](https://www.slamtec.com/en/Lidar/A3Spec).
The manufacturer also provides [RPLIDAR SDK](https://github.com/Slamtec/rplidar_sdk)
supporting Linux, macOS, and Windows systems, which allows working with the hardware.

The lidar has its own power supply. It also has a USB cable to send data to the operator's device.

### Specification

| Parameter               | A3           |
| ----------------------- | ------------ |
| Measurement range       | up to 25m    |
| Measurement frequency   | up to 16kHz  |
| Point cloud frequency   | 5Hz - 20Hz   |
| Angular resolution      | up to 0.225° |
| Communication interface | TTL UART     |
| Communication speed     | 256000bps    |

### Sources

- [RPLIDAR A3](https://www.slamtec.com/en/Lidar/A3)
- [SDK GitHub](https://github.com/Slamtec/rplidar_sdk)
- [RPLIDAR A3 User Manual](https://download.kamami.pl/p573426-LM310_SLAMTEC_rplidarkit_usermanual_A3M1_v1.0_en.pdf)
- [RPLIDAR A3 Introduction and Datasheet](https://download.kamami.pl/p573426-LD310_SLAMTEC_rplidar_datasheet_A3M1_v1.3_en.pdf)

### Scanning Modes

RPLIDAR offers several scanning modes. Only two are worth noting: default sensitivity (indoor) and stability (outdoor). When working outside, especially on very sunny days, you can clearly see the difference in the number of correctly performed measurements. The other modes exist for backward compatibility with previous generations of the device and/or require lower baudrate during data transmission.

| ID  | Scan mode                        | Sample time [us] | Frequency |
| --- | -------------------------------- | ---------------- | --------- |
| 0   | Standard                         | 252              | 0.484406  |
| 1   | Express                          | 126              | 0.968812  |
| 2   | Boost                            | 63               | 1.93762   |
| 3   | **Sensitivity/Indoor (default)** | 63               | 1.93762   |
| 4   | **Stability/Outdoor**            | 100              | 1.2207    |

### Data

- The default format of lidar data is pairs (angle, distance) in units [degrees, millimeters].
- Measurement pairs are grouped by 360 degrees - one point cloud.
- If a measurement for a given angle fails, the distance value is 0.
- The maximum number of points in one cloud is 8192, which is a limitation set by the manufacturer.
- The number of points in a cloud - 360 degrees - depends on the scanning mode and the number of revolutions per minute (RPM). The slower the rotation speed, the more points in the cloud. The recommended operating range is from 200 to 1023 rpm. At values lower than 200, there is a risk of exceeding 8192 points per cloud, which can cause problems. The default value is 660 rpm.

### Lidar Motor Speed Measurements

| Software RPM | Measured RPM | Software RPS | Measured RPS | Error  |
| ------------ | ------------ | ------------ | ------------ | ------ |
| 200          | 196.62       | 3.33         | 3.28         | ~1.7%  |
| 400          | 432.02       | 6.67         | 7.20         | ~8.0%  |
| 600          | 684.52       | 10.00        | 11.41        | ~14.0% |
| 800          | 965.14       | 13.33        | 16.09        | ~20.6% |
| 1000         | 1268.98      | 16.67        | 21.15        | ~26.8% |

### General Lidar Results Measurements

Scanning mode: **Sensitivity**

| Target RPM | Total time [s] | Points | Points per cloud | Clouds | Avg. time per cloud [s] | Measured RPM |
| ---------- | -------------- | ------ | ---------------- | ------ | ----------------------- | ------------ |
| 200        | 28.369         | 447133 | 4860.14          | 92     | 0.31                    | 194.58       |
| 400        | 28.187         | 444097 | 2198.50          | 202    | 0.14                    | 429.99       |
| 600        | 28.308         | 446794 | 1387.56          | 322    | 0.09                    | 682.49       |
| **660\***  | 28.378         | 448529 | 1274.23          | 352    | 0.08                    | 744.24       |
| 800        | 26.792         | 423121 | 984.00           | 430    | 0.06                    | 962.97       |
| 1000       | 27.327         | 432130 | 748.93           | 577    | 0.05                    | 1266.88      |

\* default

## Servo

The servo is responsible for rotating the plane that is scanned by the lidar.
It is controlled by a PWM signal using the microcontroller.

It requires its own power supply.

## IMU

IMU (Inertial Measurement Unit) is a device used for inertial navigation equipped with an accelerometer and gyroscope. The system we use is **MPU-6050** with 6 degrees of freedom (accelerometer X, Y, Z and gyroscope X, Y, Z).
In the initial phase of project development, it was noticed that a construction consisting of just the lidar and servo could be prone to inaccuracies caused by:

- the construction could be tilted more or less in any direction during scanning
- the servo we use has limited accuracy

Both problems were meant to be solved by the IMU, which would provide real-time data about the current tilt/rotation of the scanning plane.

Unfortunately, results using the described IMU and plane rotation estimation methods turn out to be even more inaccurate than using just the lidar and servo. The reasons should be sought in position estimation, which involves continuously updating the state based on previous measurements. In this way, the error grows very quickly, which means that two scans of the same scene can differ significantly from each other.

The final version of the project is prepared to work **with** and **without** IMU.
For better results, it is recommended not to use IMU. If you plan to move the construction during scanning, you can decide to use it, although you will then have to deal with growing error.

### MPU-6050

<img src="/assets/projects/lidar-2d-to-3d/imu.jpg" height=200>

The basic functionality of the device is using a 3-axis accelerometer and gyroscope. In this way, we receive a stream of _raw_ measurements consisting of six 2-byte floating-point numbers in the scale described in the documentation. The module communicates via I2C interface.

[MPU-6050 in i2cdevlib database](https://www.i2cdevlib.com/devices/mpu6050#links)

### MPU-9250

A module completely compatible with MPU-6050. The difference is that it has an additional 3-axis magnetometer, which requires special calibration. The magnetometer is currently not used in the project.

### Calibration

MPU-6050 calibration involves placing it on a surface as flat as possible, performing a series of measurements (6 values per measurement), calculating their average, and saving the results. The gravitational acceleration should also be taken into account, which should be clearly visible on one of the axes. The calibration results should be added to each subsequent measurement with the appropriate sign.

The _lidar-tools/sync_ program performs calibration automatically before starting scanning (if we use IMU).

### DMP

An interesting part of MPU is the DMP (Digital Motion Processor) module, which the manufacturer boasts about, but at the same time official documentation completely omits its existence. On the internet, you can find numerous unofficial discussions and libraries trying to use its functionality. One of many (including still undiscovered) functionalities is rotation estimation based on _raw_ data (calculating quaternion).

## Microcontroller

<img src="/assets/projects/lidar-2d-to-3d/atmega.png" width=300> 

The microcontroller used in the project is ATmega328p. The software that must be on it is in the _lidar-avr_ repository. It is responsible for:

- controlling the PWM signal to control the servo
- communication via I2C with MPU-6050 (or MPU-9250)
- communication with the operator's computer (receiving/sending data frames)

Details about wire connections and project components can be found in the [hardware](#hardware) section (an Arduino UNO board was used, but the microcontroller is programmed directly).

## Scanning Head

The mechanical construction on which the individual project components are mounted.

### Prototype

![ezgif-3-d5575b524c23](/assets/projects/lidar-2d-to-3d/ezgif-3-d5575b524c23.gif)

### Final Version

The construction was designed in Autodesk Fusion 360 and printed using 3D technology.

<img src="/assets/projects/lidar-2d-to-3d/Mocowanie na łazik v21 V2.png" width=300>

# 3D Scanning

3D scanning involves collecting data from several sources and combining them in a way to get information about the surroundings.

The most important device is the lidar, which when started can generate several point clouds per second. Points are represented as pairs (angle, distance) in units [degrees, millimeters].

The next important element is the servo. We know the position we want it to set to. For this purpose, an internal unit is used, which can be converted to degrees.

The last (but optional) element is the IMU, which can provide measurements in the form of 3 values (x, y, z) of the accelerometer, 3 values (x, y, z) of the gyroscope.

The **lidar-tools/sync** program when started on the operator's device:

1. creates a subprocess and starts the **lidar-scan** program, which connects to the lidar connected to the operator's device; starts a loop receiving **lidar** measurements from lidar-scan
2. connects to the microcontroller, which should have the **lidar-avr** program on it
3. starts a loop sending commands to the microcontroller controlling the **servo**
4. starts (optionally) a loop receiving **IMU** measurements from the microcontroller

IMU measurements and requested servo positions are collected along with the time they occurred. For each received lidar point cloud, the best time-matching IMU measurement or requested servo position is selected. The method chosen depends on the user (in other words, the user decides whether to use IMU or requested servo positions).

Then calculations are made, resulting in a group of ready points. They are printed to standard output (stdout).

### Processing Raw IMU Measurements

MPU-6050 provides six 2-byte floating-point values in each _raw_ measurement. To understand the data, it can be easily visualized on a graph. They will correspond to movements and rotations of the device.

[Video with visualization](https://youtu.be/J4pH3LHojVM)

<iframe width="560" height="315" src="https://www.youtube.com/embed/J4pH3LHojVM?si=2tLsH-W5limSQQEy" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

**Example graph:**

<img height=300 src="https://raw.githubusercontent.com/dsonyy/python-stuff/master/accelerometer-live-plot/example.png">

_Raw_ data after being received by _lidar-tools/sync_ is passed to the attitude estimator, which is updated with subsequent values. As a result, the estimator returns a [quaternion](https://pl.wikipedia.org/wiki/Kwaterniony) - a mathematical object consisting of 4 floating-point numbers (w, x, y, z), which can be interpreted as object rotation.

# Configuration

## Hardware

1. The microcontroller must be connected to the operator's device to enable USART communication
2. The servo must be connected to power and the microcontroller as shown in the diagram
3. IMU (MPU-6050 or compatible MPU-9250) can be connected to the microcontroller as shown in the diagram
4. The lidar must be connected to a stable power source and to the operator's device

![](/assets/projects/lidar-2d-to-3d/sketch.png)

## Software

**On the microcontroller**, the _lidar-avr_ program must be flashed.

**On the operator's device**, you need:

- _lidar-tools/sync_
- _lidar-scan_

Then you can proceed to configure _lidar-tools/sync_. All available program parameters should be adjusted using command line arguments. Their number can be large, so it's convenient to prepare a script that will run _lidar-tools/sync_ passing it appropriate parameters (like _sync.ps1_ located in the main _lidar-tools_ project repository).

### Parameters for _lidar-tools/sync_

| Parameter     | Description                                                                                                                 | Notes                                 |
| ------------- | --------------------------------------------------------------------------------------------------------------------------- | ------------------------------------- |
| avrport       | port through which communication with the microcontroller takes place                                                       |                                       |
| avrbaud       | baudrate used by the microcontroller for communication                                                                      | default 19200                         |
| lidarexe      | path to the _lidar-sync_ executable file                                                                                    |                                       |
| lidarport     | port through which communication with the lidar takes place                                                                 |                                       |
| lidarmode     | lidar operating mode                                                                                                        | 3 (indoor, default) or 4 (outdoor)    |
| lidarpm       | number of lidar revolutions per minute (RPM)                                                                                | 200-1023, default 660                 |
| servostep     | single servo step in its units                                                                                              |                                       |
| servodelay    | time interval between servo steps in milliseconds                                                                           |                                       |
| servomin      | minimum servo position in its units                                                                                         | default 1000                          |
| servomax      | maximum servo position in its units                                                                                         | default 3000                          |
| servocalib    | servo position for which the lidar plane will be in a position as parallel as possible to the ground                        | default 2500                          |
| servostart    | starting servo position for scanning                                                                                        | default 3000                          |
| servounit     | important value determined experimentally, which is used to convert servo unit to degrees: **1 servo unit = _servounit_ °** | default about -0.047                  |
| cloudrotation | all points lying on the plane scanned by the lidar will be rotated by _cloudrotation_ radians                               | for prototype head: -π/4, (-0.785398) |
| acceluse      | if true then takes IMU into account, if false then only requested servo positions                                           | _true_ or _false_                     |

# Materials

## Helper Software

Software that was created as an aid during work on the projects. These are simple programs/scripts that perform one task.

- [serial-read-accelerometer](https://github.com/dsonyy/go-stuff)
- [accel-plot](https://github.com/dsonyy/python-stuff)
- [angle-visualization](https://github.com/dsonyy/cpp-stuff)
- [pipeline](https://github.com/dsonyy/go-stuff)
- [attestimator](https://github.com/knei-knurow/attestimator)

## Point Cloud Examples

- [3D (Google Drive)](https://drive.google.com/drive/folders/1JSF53_TLBTgeE407KCZTTn-GNv1g_iZ-?usp=sharing)
- [2D (Google Drive)](https://drive.google.com/drive/folders/1GGNDYH1F7mPqHnHaxzhhpN3W45uAHUO9?usp=sharing)
