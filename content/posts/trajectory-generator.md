---
author: Declan Freeman-Gleason
title: A Constrained Trajectory Generator for Wheeled Mobile Robots
date: 2019-12-01
description: Background on the trajectory generator, odometry, and controller I wrote for mobile robots
math: true
tags: ["Java", "Trajectory generation", "Splines", "Kinematics", "Controls", "Lie thoery"]
---

## Summary
I gave a presentation to about a hundred students on trajectory generation for wheeled mobile robots. The constrained trajectory generation algorithims I presented on are very similar to those in *Sprunk*[^1]. I contributed some of an implementation of this trajectory generator to [WPILib](https://wpilib.org), which is an open source robotics utility library with thousands of users. I also wrote my own version of this trajectory generator[^2] following *Sprunk*[^1] and added exponential map-based odometry and a nonlinear time-varying feedback controller for a unicycle model[^3] from *De Luca et al.*[^4]. This results in a trajectory generator that can (given a kinematic model of the robot) generate some target wheel velocities from a set of waypoints and constraints. Given the odometry and the unicycle controller we can also modify the target wheel velocities to control our global pose, in case the robot deviates from the desired path. The target wheel velocities are fed into a set of (usually) PD controllers on each of the robot's wheels. This trajectory generation methedology can easily be extended to holonomic robots by changing out the unicycle controller and kinematics model.

## Technical details
I wrote a small JavaScript-based implementation of this trajectory generator and used it to make an accurately-animated presentation about the trajectory generation process. The resulting presentation is embeded below, and contains most of the technical details the trajectory generator, including:
 - Hermite splines.
 - Recursive spline subdivision with the logarithim map (the inverse of the exponential map) to get closely-spaced spline sample points that allows for approximating arc length with Euclidian distances. This method is much faster than the naive method which requires picking a (usually) very small parameter increment, $\Delta s$, that will always give points that are closely spaced enough that a Euclidian distance is appropriate.
 - Trapezoidal motion profiles
 - A detailed animation and explanation on the algorithim used to generate constrained motion profiles
 - Inverse kinematics
 - Odometry
 - The global pose controller
 - Demo of a real robot using my trajectory generator implementation
 A video of me giving a voiceover on the presentation is also embedded for convenience.

<iframe width="100%" height="450px" name="presentation-frame" src="https://pietroglyph.github.io/trajectory-presentation/"></iframe>

<iframe width="100%" height="450px" src="https://www.youtube-nocookie.com/embed/fEVU7dVc8B4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

[^1]:[*Sprunk*](http://www2.informatik.uni-freiburg.de/~lau/students/Sprunk2008.pdf)
[^2]:[Trajectory generation code](https://github.com/Spartronics4915/SpartronicsLib/tree/323c6f245b12f2280b2d16eb36916efa1ebf8731/src/main/java/com/spartronics4915/lib/math/twodim)
[^3]:The unicycle controller is similar to a traditional pose-stabilizing controller except that it degenerates and doesn't control heading when the desired velocity is zero (like a unicycle, which "falls over" in that case.) This controller is appropriate for differential drive robots. I also wrote, but never tested, a controller for swerve drive and mecanum drive robots, which is PID-based.
[^4]:[*De Luca et al.*](https://doi.org/10.1007/3-540-45000-9_8)
