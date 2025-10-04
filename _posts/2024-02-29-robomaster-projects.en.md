---
title: 'Robomaster Projects'
date: 2024-02-29
permalink: /posts/2024/02/robomaster-projects/
lang: en
ref: robomaster-projects
tags:
  - Projects introduction
---

To better understand the rules, you may watch [our match](https://www.bilibili.com/video/BV19V41137XZ/?spm_id_from=333.337.search-card.all.click) first

I'm part of the Vision Division within the Robomaster Club of Shanghai Jiao Tong University, where I have actively contributed to several key initiatives. Across all our projects except Robot Radar, we utilize the JETSON Orin NX developper kit to enable high performance computing.

The duty of Vision Division focuses on specifying tasks that Robots will do, rather than controlling them to accomplish these tasks. In practice, we have made up robotic commands, which are user-defined and transferred via an UART communication module to a Micro-Computing Unit (MCU). The MCU handles the deserialization of these instructions, contributing to tactile robot manipulations in real time. All algorithmic responsibilities on the MCU-front falls under the remit of our club's Electronic Control Division—a team with which we consistently collaborate.

### AuotAim Module

The AutoAim Module represents one of our division's paramount contributions, and it serves as a cornerstone for majority of the robots we design and deploy. Its relevance becomes particularly prominent during Robomaster competitions where robots engage in combative interactions by aiming shots at each other's armored plates. We've enhanced our shooting precision by incorporating a Kalman Filter into this module. This predictive system gives us an upper hand as it forecasts potential routes of robot movement, thus offering valuable insights on the prospective locations of the armor plates. Following successful filtration, our shooting commands are transmitted to the MCU which then aligns, fires bullets with high accuracy, allowing significant strategic advantages over opponents.

Notably, all our robotic models except the Radar come equipped with this integral AutoAim Module.

### Robot Sentry

The Robot Sentry project involves designing an autonomous robotic system. The diagram below illustrates the overall structure.

![Sentry Architecture](/assets/images/Robomaster/Sentry-Architecture.png)

Beginning from the bottom, we have the Hardware Layer consisting of vision-related sensors like the Livox Lidar Mid360 and Hikrobot MV-CS016-10UC industrial camera fitted with a 6mm lens. Note that only vision-related are covered in the diagram. For example, IMU is ommitted because IMU is not vision-related and is processed by the MCU.

Above the hardware stratum is the Localization Layer that houses the LIDAR-IMU Odometry (LIO), which works using the Iterative Closest Point Alignment technique to make accurate, real-time estimations of pose alignment between lidar points and pre-generated point cloud, initiating rough estimation utilizing IMU data. To optimize pose estimation, we use one algo variant, namely Fast-LIO, which filters and smooths out these computations. In detail, we have generated a `.pcd` type lidar point cloud object with FAST-LIO mapping. We then down-sampled the point cloud so as to further decrease the time-complexity of point cloud matching. Then we leverage re-localization feature for pinpointing the robot.

Next comes the Navigation Layer, built upon the ROS movebase package. A costmap, which is basically a 2D `.pgm` map (AKA static layer) overlayed with layers such as obstacle and inflation. An abstraction into a costmap graph created by a global planner lets us guide the route progression through A* search algorithm. Parallelly, unplanned events like swiftly appearing pedestrians are tackled by a local planner, necessitating the inclusion of a separate obstacle layer for imminent challenges not originally present in the static map layer. We should note that simply considering depth as obstacles may misinterpret slopes as obstacles. That's why we utilized ground segmentation algorithm to segement all the planes, including slopes.

Atop these systems is the Decision Layer, where a multi-state behavior tree manipulates navigation based on inputs fetched from a communal information space called 'BlackBoard'. This serves as data storehouse where information passed across via UART communication resides.
Our behavioural tree employs components including Sequence Node, Loop Node, Action Node and Task Node—a novel component we created to function silently in the background. The usage varies for each node; Sequence nodes prioritize different tasks and halt if a task fails whereas Loop Nodes facilitates patrolling across multiple position.

As a whole, Robot Sentry's expansive nature contributed significantly towards my learning experience—particularly when it came to debugging large-scale systems iteratively.

### Robot Aerial

In the Aerial project, our team established a fundamental setup for indoor localization using Visual Inertial Odometry (VIO) powered by PX4. By adhering to official guides, we employed the ROS package MAVROS as a key conduit, allowing ROS1 topics to interact with MAVLink and enabling effective communication with PX4. For VIO functionality, we utilized Intel's RealSense T265 technology.

Through this scheme of operations, we successfully accomplished automatic self-positioning capabilities for our aerial robot model. This marked a significant step in enhancing drone autonomy within enclosed environments.

### Robot Radar

As part of this project, we developed a comprehensive process to pinpoint the precise locations of visible robots within the world frame—an accomplishment realized through advanced lidar-camera fusion techniques. We delved into and tested several procedures for calibrating the camera frame and the lidar frame with respect to the global coordinate system. Making use of classical tools such as Perspective-n-Point (PnP) for cameras and Iterative Closest Point (ICP) for lidars, we yielded results of exceptional accuracy.

The detection and tracking of all visible robots was a crucial aspect of our project execution. We refined our systems by employing object detection mechanisms using M-detector (movable object detection via lidar point clouds) and YOLOv9 in conjunction with ByteTrack. Once robots were accurately detected and tracked, their respective positions could be determined by performing frame transformations that leveraged the meticulous calibration systems we'd put in place.

However, the responsibilities accompanying my role as Project Leader extended beyond technical advancements alone. I dedicated considerable time to manage our team efficiently, ensuring smooth communication channels between members, setting realistic deadlines, anticipating potential roadblocks, and having contingency plans ready beforehand. My focus on effective project management strategies also ensured timely completion of milestones, promoting proactive practices to prevent last-minute hitches or stressful work crunches.

It was an enriching experience to lead and collaborate with proficient minds collectively aimed at pushing boundaries of the knowledge and equipments we have. Assembling these lessons from the ground up honed my leadership skills—guiding me toward broader understandings of teamwork dynamics while instilling humility from hands-on problem-solving experiences.
