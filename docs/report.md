# Final Report

# Table of Contents
* Abstract
* [Introduction](#1-introduction)
* [Related Work](#2-related-work)
* [Technical Approach](#3-technical-approach)
* [Evaluation and Results](#4-evaluation-and-results)
* [Discussion and Conclusions](#5-discussion-and-conclusions)
* [References](#6-references)

# Abstract

 Theses measurments allow for users to control multiple smart devices in a room by simply pointing their phone at the device they wish to control.

![alt text](./media/phone.png?raw=True "User pointing phone for device recognition and control") 
# 1. Introduction

## Motivation & Objective

Current estimates expect the number of IoT devices to hit 30 Billion by 2025 with continous exponential growth[1]. With the growth of avaiable devices, the number of smart devices that each user has to manage also continues to grow. This increase in devices per user, makes it continuously harder for users to manage and control their devices in an intuitive way. The goal of the project is to create a localization and orientation technique for detecting and controlling smart device objects in a room via pointer control using only the sensors available in smartphones today. The project aims at accomplishing this with the least amount of additional sensors as possible.
 
## Todays Limitations

Most localization techniques used today include GPS, bluetooth, or Wi-fi ranging. The localizations techniques have accuracies in the range of 1-5m and so are generally not considered to be precise enough to distinguish interactions in a typical home setting. Additionally these techniques each have ranging capabilities which also can have limitations such as not being in a building with cement walls for GPS and being within close proximity for BLE and Wifi. UWB ranging techniques on the other hand offers localization accuracies within +/-20cm per anchor tag pair. This localization precision is required for accurate indoor user interation tracking. 

# Novelty, Rationale:, and Impact: 

This project is novel because it present a feasable method for intuitively managing smart devices, and provides a novel approach localization techniques using UWB and IMU data that could stretch beyond the intended use case. 

# Challenges: 
- using as few UWB anchors as possible while still providing accurate location data
- succesfully proceesing IMU data to understand movements of humans while being able to filter out drift
- accuratly fusing orientation and position to make predictions about intended line of sight object selections

# Requirements for Success: 
- Create a "Smart Controller" with IMU and UWB sensor to ESP32 microcontroller to be used to test accuracies.
- Tune UWB tag and initiator antennas to offer accurate measurements
- Filter UWB position etimates to allow for continous/not noisy readings
- Create GUI which illustrates position of objects in the room and the guesses of which object the device is interacting
- Demonstrate the use of a controller on a smart object
# Metrics of Success: 
- Positional Accuracy (x, y, z)
- Orientation Accuracy (Alpha, Beta, Gamma)
- Cost requirements/sensors needed to perform localization
- Ability of controller to distinguish between multiple objects in a room

# 2. Related Work

# 3. Technical Approach

## Hardware
- BN055 9-axis IMU
- ESP32 Wrover
- Qorvo DWM300 (1 Initiator, 1 Tag)
![alt text](./docs/media/sensors.png?raw=True "User pointing phone for device recognition and control")

## Sensor Fusion Approach

![alt text](./docs/media/Pose_estimation.png?raw=True "Orientation and Pose Estimation")

To achieve accurate detection of where a user is pointing a controller in free space, the two measurements that are needed are orientation estimation (Alpha, Beta, Gamma), and pose estimation (x, y, z). 

To obtain an oreintation estimation the 9 axis IMU was used. There are two ways that a 9 axis IMU can detect orientation.The first method is by sensing where the magnetic fields point with the magnetometer and where gravity acceleration is pointing with the accelerometer, and then finding the cross product of theses two vectors we can get orientation. The second method is if we know the original orientation and the gyroscope is working perfectly, the angular rotation multiplied by time will tell us the orientation of an object. The IMU and built in Arm Cortex M0 microprocessor in the BN055 provide cutom software to fuse these two estimation together relying partially on measurments from each to make a fused orientation estimate which can be update at 100Hz.  

To obtain a pose estimation, the goal of the project was to use the orientation estimation and fuse it with IMU and UWB measurements to get X, Y an Z cordinates. In theory there are two methods to get position with this approach. The first is if you relative know orientation of the controller to the room you are in, you can perform a tranformational rotation on the Acceleration sensors to get relative x, y, and z positional accelerations.

![alt text](./media/rotation.png?raw=True "Rotation of Raw Accelerometer Values to get true Ax, Ay, Az values") 

By using the above translated accelerations, you can integrate acceleration to get velocity, and integrate velocity to get position. The biggest limiting factor with this approach is that the acceleromter is prone to drift and since position is a result of a double integrtaion, accumulation of positional error can be accumulated. The second method that was initially intended to be used to estimate positionwas using one UWB anchor and tag, where the initial anchor position in the room was known. By having ine tag in the room and getting a distance measurment from the UWB. there is essentially a sphere of possible positions that the tag could be in realtion to the anchor. The idea was that over time if we combined both positional observation from the acceleration and distance observations from the UWB anchor, the locations where are user is could be to see these observations overtime could mean the user was only in one spot.

# 4. Evaluation and Results


# Updated Postion approach
### Trick One

### Trick Two

# Line of Sight Object Detection

# 5. Discussion and Conclusions

# 6. References

[1] https://explodingtopics.com/blog/iot-stats
[2] 


## Project Links
* [Proposal](proposal)
* [Midterm Checkpoint Presentation Slides](http://)
* [Final Presentation Slides](https://docs.google.com/presentation/d/1ARPfKs8R3b8PLh7hjExXQs66cmnulIzx0FZq7Vi8yAQ/edit?usp=sharing)
* [Final Report](report)