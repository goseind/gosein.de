---
layout: post
title:  "Understandable Diagrams of Software System Architecture"
date:   2023-08-13
categories: software architecture model diagrams views software engineering c4 systems design
---

In my search for an understandable and intuitive model for designing a software system architecture, I recently came across the C4 model. The C4 model makes use of Shneiderman's mantra, which is a simple concept for data visualisation and can be summarised as: "Overview first, zoom and filter, then detail on demand". I would like to demonstrate the use of the C4 model by illustrating the architecture of the [Autonomous Robot](https://gosein.de/autonomous-robot.html) described in a previous blog post.

We start with what is called a system context diagram, which shows how the software fits into its environment, including user and system dependencies. For larger environments, the model provides a landscape view. The context clearly shows the two software systems, the containers running on the computer and the bot.

![](/assets/2023-08-13-nice-diagrams/structurizr-85399-SystemContext-001.png)

Zooming in on the container diagram shows the individual components of each software system in scope and their interactions across systems.

![](/assets/2023-08-13-nice-diagrams/structurizr-85399-Container-001.png)
![](/assets/2023-08-13-nice-diagrams/structurizr-85399-Container-002.png)

The cool thing is that C4 is a DSL and you can save all of the above as code, e.g. along with your source code of the systems.

```
workspace "Ball Schubser" "A robot that pushes a ball" {

    model {
        ballschubser = softwareSystem "Ball Schubser" "Software System for Ball-Schubser" {
            cockpit = container "Cockpit" "Webpage with robots camera and status in real-time" {
            }
            detectnode = container "Object Detection Node" "Real-time object detection with YOLO" {
            }
            navnode = container "Navigation Node" "Publisher for navigation instructions" {
            }
            ros = container "ROS Core" "Roboter Operating System Master" {
            }
        }
        bot = softwareSystem "Turtlebot" "Bot with camera and Raspberry Pi with WiFi module" {
            rosbot = container "ROS Robot Node"
            camera = container "Camera"
            controller = container "Robot Controller"
    }
    camera -> rosbot "raw image"
    rosbot -> ros "raw image"
    ros -> detectnode "raw image"
    detectnode -> ros "image with detections"
    detectnode -> ros "ball position"
    ros -> navnode "ball position"
    ros -> cockpit "mission status"
    ros -> cockpit "image with detections"
    navnode -> ros "instructions"
    ros -> rosbot "instructions"
    rosbot -> controller "instructions"
}
```

I find this a very practical approach to architecture design, and it has helped me a lot as it provides an easy and fast way to create the first draft but then allows you to build on that and create more sophisticated diagrams and look at them with different views to understand how things interact.

For more information about C4 and the many ways you can use it, click here:

* [C4 Model](https://c4model.com/)
* [Structurizr DSL cookbook](https://github.com/structurizr/dsl/tree/master/docs/cookbook)
* [Language reference](https://github.com/structurizr/dsl/blob/master/docs/language-reference.md)

I can also recommend [Structurizr](https://structurizr.com/), a dedicated platform with an online DSL editor and diagram download and embed.
