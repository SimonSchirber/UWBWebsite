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

As the number of smart devices in a household continues to grow there is need for a system to distinguish and control said devices. The goal of this project is develop as system on top of the sensors available in current smartphones to allow a user to interact with multiple smart devices indoors. Current smartphones are equipped with both IMU sensing and UWB ranging, by leveraging these sensors we have created a system to introduce two UWB anchors in a known space to facilitate localization in said space. Then, by combining the location estimate with the IMU data for where the user is pointing their smartphone a smart device in a room can be selected. After this selection is made the user can control the smart device and the stimulus is broadcasted over BLE.

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
Indoor localiztion is a very wider area of research that this project builds upon. Two specific techniques for indoor localization that are used in this project are UWB indoor localiztion and IMU localization.

UWB Indoor Localization
In a paper by Zwirello et al. [CITE] indoor localization using UWB is done by rigging a space with many anchors and ranging to a tag somewhere in the space. This group analyzed both the optimal locations for placing UWB in a space as well as the optimal algorithms for converting the ranging data into an absolute position. The following figure shows an example scheme of anchors in a cube space.

![alt text](./media/UWB_range.png?raw=True "UWB Anchor Positioning Example") 

The goal of this group was to find and optimal positioning algorithm that could use many anchors to find very precise location. For systems using few UWB anchors they found that the optimal approach is to simple estimate the location of the tag as somewhere on the surface of a sphere centered at each anchor with a radius equal to the range measurment. For a 3-D space, 4 anchors are need to pinpoint the location of a tag to one point. As this project uses a maximum of 2 anchors, the best estimate is somewhere on a circle that is the intersection of 2 spheres. A representation of this intersection is shown in the figure below.

![alt text](./media/sphere_intersect.png?raw=True "Intersection of 2 Spheres") 

IMU Indoor Localization
In a paper by Ibrahim et al. [CITE] indoor localization was achieved using a 9-DOF IMU sensor and a barometric pressure sensor. The basis of the system was to find the derivative of the acceleration to obtain the jerk and then take the triple integration to determine displacement. This is done in an attempt to reduce the effects of sensor drift on the overall measurements. The pressure measurement was used to estimate the height by making assumptions about how atmospheric pressure decreases as height increases. They then passed these measurements through a Kalman filter to find the displacement estimates and were able to track a walk through a multi-story building withing 3% error. The graph of this experiment is shown in the figure below.

![alt text](./media/IMU_experiment.png?raw=True "IMU Localization Experimetnt [CITE]") 

This data is very impressive and lends some support to the feasibility of doing human localiztion with IMU data but it relied on several crucial assumptions. The assumptions made by this group was that the subject have the IMU sensor attached at the belt and that the subject always be moving forward. For this project, the user must be allowed to wave their smartphone around the room to point at smart devices and so the assumption of having the IMU be fixed on the body was simply not feasible. Allowing the user to wave the smartphone around introduces far too much noise in the reading the come up with any useful data to predict position from the IMU data.
# 3. Technical Approach

## Hardware
- BN055 9-axis IMU
- ESP32 Wrover
- Qorvo DWM300 (1 Initiator, 1 Tag)
![alt text](./media/sensors.png?raw=True "User pointing phone for device recognition and control")

## Sensor Fusion Approach

![alt text](./media/Pose_estimation.png?raw=True "Orientation and Pose Estimation")

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

[2] Ibrahim, Magdy, and Osama Moselhi. “IMU-Based Indoor Localization for Construction Applications.” Proceedings of the International Symposium on Automation and Robotics in Construction (IAARC), 2015, https://doi.org/10.22260/isarc2015/0059.

[3] Zwirello, Lukasz, et al. “UWB Localization System for Indoor Applications: Concept, Realization and Analysis.” Journal of Electrical and Computer Engineering, vol. 2012, 2012, pp. 1–11., https://doi.org/10.1155/2012/849638.


## Project Links
* [Proposal](proposal)
* [Midterm Checkpoint Presentation Slides](http://)
* [Final Presentation Slides](https://docs.google.com/presentation/d/1ARPfKs8R3b8PLh7hjExXQs66cmnulIzx0FZq7Vi8yAQ/edit?usp=sharing)
* [Final Report](report)
