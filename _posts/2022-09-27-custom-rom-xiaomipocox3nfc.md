---
layout: post
title:  "Development of an Autonomous Robot for Detecting and Collecting Objects"
date:   2022-09-27
categories: yolo yolov4 yolov5 robot autonomous robot ros turtlebot turtlebot3 burger object detection
---

Autonomous robots already have a broad spectrum of practical applications like autonomous cars, delivery vehicles, warehouses, or service robots. As the technology behind this becomes more applicable, it was time to learn more about it.

For this, we build a small robot prototype capable of navigating toward objects with just one front camera, using the Robot Operating System (ROS) to control the robot and You Only Look Once (YOLO) for object detection.

While in this blog post I want to concentrate on a more technical viewpoint, we also published a paper about this project. The paper describes different approaches to navigation and object detection and their challenges. It also goes into performance differences between YOLOv4 and YOLOv5. You can find it [here](../assets/documents/AMR_Ball-Schubser_Term_Paper_HSMA_2022_v1.pdf). If you're solely interested in the technical setup continue reading here.

We used a preconfigured Turtlebot3 Burger robot and equipped it with a front camera and a fork to pick up the objects (in this case a tennis ball).

<img src="/assets/images/robot/ball-schubser_front_side_new_desc.jpg" height="300"/>

The robot 