# Path Planning Project

Udacity Self-Driving Car Nanodegree - Term 3, Project 1

[//]: # (Image References)
[5mi]: ./5mi.png "5 mi"

# [Rubric](https://review.udacity.com/#!/rubrics/1020/view) points

## Compilation

### Your code should compile

The code compiles without errors or warnings using the following command line:

    cmake .. && make

A new file was added to the project: [```src/spline.h```](./src/spline.h). It is the [Cubic Spline Interpolation](http://kluge.in-chemnitz.de/opensource/spline/) implementation, which allows to use splines instead of polynomials. It was a great suggestion in the classrooms Q&A lecture.

## Valid Trajectories

### The car is able to drive at least 4.32 miles without incident

[todo]

![5 mi][5mi]

| Additional requirements                                               | Status |
| --------------------------------------------------------------------- |:------:|
| The car drives according to the speed limit                           | YES    |
| Max acceleration and jerk are not exceeded                            | YES    |
| Car does not have collisions                                          | YES    |
| The car stays in its lane, except for the time between changing lanes | YES    |
| The car is able to change lanes                                       | YES    |

## Reflection

### There is a reflection on how to generate paths

Based on the provided seed project, the path planning algorithm sits in [```src/main.cpp```](./src/main.cpp).
