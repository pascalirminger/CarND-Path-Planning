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

Based on the provided seed project, the path planning algorithm sits in [```src/main.cpp```](./src/main.cpp). The code can be separated in five different segments:

|    | Code segment                                 | Code lines                          |
| -- | -------------------------------------------- | ----------------------------------- |
| 1. | Determine main car parameters                | [234-258](./src/main.cpp#L234-L258) |
| 2. | Generate predictions from sensor fusion data | [264-313](./src/main.cpp#L264-L313) |
| 3. | Determine best trajectory                    | [319-355](./src/main.cpp#L319-L355) |
| 4. | Generate new path                            | [361-472](./src/main.cpp#L361-L472) |
| 5. | Send new path to the simulator               | [478-485](./src/main.cpp#L478-L485) |

The segments 1 and 5 basically were provided by the seed code. Therefore, only the three segments in between are described in detail below.

### Generate predictions from sensor fusion data

This code segment deals with sensor fusion data. It intents to scan the environment for other vehicles. In particular, it's up to answer the following questions:

- Is there a car in front of us blocking the traffic?
- Is there a car on the left of us making a lane change unsafe?
- Is there a car on the right of us making a lane change unsafe?

The code segment answers these question by iterating through all other vehicles that have been recognized with sensor fusion.

### Determine best trajectory



### Generate new path


