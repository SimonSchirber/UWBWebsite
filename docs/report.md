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
- 
# Metrics of Success: 

# 2. Related Work

# 3. Technical Approach

# 4. Evaluation and Results

# 5. Discussion and Conclusions

# 6. References

[1] https://explodingtopics.com/blog/iot-stats
[2] 


## Project Links
* [Proposal](proposal)
* [Midterm Checkpoint Presentation Slides](http://)
* [Final Presentation Slides](https://docs.google.com/presentation/d/1ARPfKs8R3b8PLh7hjExXQs66cmnulIzx0FZq7Vi8yAQ/edit?usp=sharing)
* [Final Report](report)