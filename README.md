# Path Planning Project

Udacity Self-Driving Car Engineer Nanodegree Program - Term 3, Project 1

[//]: # (Image References)
[5mi]: ./images/5mi.png "5 miles"
[15mi]: ./images/15mi.png "15 miles"
[20mi]: ./images/20mi.png "20 miles"

# [Rubric](https://review.udacity.com/#!/rubrics/1020/view) points

## Compilation

### Your code should compile

The code compiles without errors or warnings using the following command line:

    cmake .. && make

A new file was added to the project: [```src/spline.h```](./src/spline.h). It is the [Cubic Spline Interpolation](http://kluge.in-chemnitz.de/opensource/spline/) implementation, which allows to use splines instead of polynomials. It was a great suggestion in the classrooms Q&A lecture.

## Valid Trajectories

### The car is able to drive at least 4.32 miles without incident

The car has managed to drive incident-free over 4.32 miles multiple times.

![5 miles][5mi]

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
|---:|:---------------------------------------------|:-----------------------------------:|
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

The code segment answers these question by iterating through all other vehicles that have been recognized with sensor fusion. A car is considered "dangerous" when its distance (```s```-value) to our car is less than 30 meters.

### Determine best behaviour

This code segment decides what to do. 

- If we have a car in front of us, is it safe to change lanes?
- When do we slow down or speed up?
- If we aren't on the center lane, is it safe to change lanes back to center lane?

Based on the prediction our car is going to be in, this code segment increases or decreases the velocity or initiates a lane change when it's safe. The reason to drive as much as possible on the center lane is the given possibility to change lanes to either side when we are approaching a slower car that is driving in front of us.

### Generate new path

This code segment plans the new path based on the target velocity and target lane. The new path starts with a certain number of pionts from the previous path. From there a spline is generated beginning with the last two points of the previous path (or the current position, heading and velocity if no previous path exists) and ending with three points 30, 60, and 90 meters ahead on the target lane.

The spline produces a really smooth ```(x,y)```-trajectory. To prevent excessive jerk and acceleration, the car's velocity is only allowed to increase and decrease by a small amount for reaching the target velocity. The corresponding next ```(x,y)```-points are solely calculated along the ```x```- and ```y```-splines created earlier.

### Conclusion

The resulting path planner works well, but still not perfectly. The rudimentally structured code block of the behavior planner could be improved by implementing a real finite state maschine, that also considers real cost functions instead of the 60 meters gap search to achieve a more intelligent driving approach.

Nevertheless, I am very satisfied with the resulting path planner and its performance. It has managed to accumulate incident-free 10+ mile drives multiple times (see images below).

![15 miles][15mi]
![20 miles][20mi]

*The description below is Udacity's original README from the [seed project](https://github.com/udacity/CarND-Path-Planning-Project).*

---

# CarND-Path-Planning-Project

Self-Driving Car Engineer Nanodegree Program
   
### Simulator

You can download the Term3 Simulator which contains the Path Planning Project from the [releases tab (https://github.com/udacity/self-driving-car-sim/releases/tag/T3_v1.2).

### Goals

In this project your goal is to safely navigate around a virtual highway with other traffic that is driving +-10 MPH of the 50 MPH speed limit. You will be provided the car's localization and sensor fusion data, there is also a sparse map list of waypoints around the highway. The car should try to go as close as possible to the 50 MPH speed limit, which means passing slower traffic when possible, note that other cars will try to change lanes too. The car should avoid hitting other cars at all cost as well as driving inside of the marked road lanes at all times, unless going from one lane to another. The car should be able to make one complete loop around the 6946m highway. Since the car is trying to go 50 MPH, it should take a little over 5 minutes to complete 1 loop. Also the car should not experience total acceleration over 10 m/s^2 and jerk that is greater than 10 m/s^3.

#### The map of the highway is in data/highway_map.txt

Each waypoint in the list contains  [x,y,s,dx,dy] values. x and y are the waypoint's map coordinate position, the s value is the distance along the road to get to that waypoint in meters, the dx and dy values define the unit normal vector pointing outward of the highway loop.

The highway's waypoints loop around so the frenet s value, distance along the road, goes from 0 to 6945.554.

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./path_planning`.

Here is the data provided from the Simulator to the C++ Program

#### Main car's localization Data (No Noise)

["x"] The car's x position in map coordinates

["y"] The car's y position in map coordinates

["s"] The car's s position in frenet coordinates

["d"] The car's d position in frenet coordinates

["yaw"] The car's yaw angle in the map

["speed"] The car's speed in MPH

#### Previous path data given to the Planner

//Note: Return the previous list but with processed points removed, can be a nice tool to show how far along
the path has processed since last time. 

["previous_path_x"] The previous list of x points previously given to the simulator

["previous_path_y"] The previous list of y points previously given to the simulator

#### Previous path's end s and d values 

["end_path_s"] The previous list's last point's frenet s value

["end_path_d"] The previous list's last point's frenet d value

#### Sensor Fusion Data, a list of all other car's attributes on the same side of the road. (No Noise)

["sensor_fusion"] A 2d vector of cars and then that car's [car's unique ID, car's x position in map coordinates, car's y position in map coordinates, car's x velocity in m/s, car's y velocity in m/s, car's s position in frenet coordinates, car's d position in frenet coordinates. 

## Details

1. The car uses a perfect controller and will visit every (x,y) point it recieves in the list every .02 seconds. The units for the (x,y) points are in meters and the spacing of the points determines the speed of the car. The vector going from a point to the next point in the list dictates the angle of the car. Acceleration both in the tangential and normal directions is measured along with the jerk, the rate of change of total Acceleration. The (x,y) point paths that the planner recieves should not have a total acceleration that goes over 10 m/s^2, also the jerk should not go over 50 m/s^3. (NOTE: As this is BETA, these requirements might change. Also currently jerk is over a .02 second interval, it would probably be better to average total acceleration over 1 second and measure jerk from that.

2. There will be some latency between the simulator running and the path planner returning a path, with optimized code usually its not very long maybe just 1-3 time steps. During this delay the simulator will continue using points that it was last given, because of this its a good idea to store the last points you have used so you can have a smooth transition. previous_path_x, and previous_path_y can be helpful for this transition since they show the last points given to the simulator controller with the processed points already removed. You would either return a path that extends this previous path or make sure to create a new path that has a smooth transition with this last path.

## Tips

A really helpful resource for doing this project and creating smooth trajectories was using http://kluge.in-chemnitz.de/opensource/spline/, the spline function is in a single hearder file is really easy to use.

---

## Dependencies

* cmake >= 3.5
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `install-mac.sh` or `install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```

## Editor Settings

We've purposefully kept editor configuration files out of this repo in order to
keep it as simple and environment agnostic as possible. However, we recommend
using the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Code Style

Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).

## Project Instructions and Rubric

Note: regardless of the changes you make, your project must be buildable using
cmake and make!

## Call for IDE Profiles Pull Requests

Help your fellow students!

We decided to create Makefiles with cmake to keep this project as platform
agnostic as possible. Similarly, we omitted IDE profiles in order to ensure
that students don't feel pressured to use one IDE or another.

However! I'd love to help people get up and running with their IDEs of choice.
If you've created a profile for an IDE that you think other students would
appreciate, we'd love to have you add the requisite profile files and
instructions to ide_profiles/. For example if you wanted to add a VS Code
profile, you'd add:

* /ide_profiles/vscode/.vscode
* /ide_profiles/vscode/README.md

The README should explain what the profile does, how to take advantage of it,
and how to install it.

Frankly, I've never been involved in a project with multiple IDE profiles
before. I believe the best way to handle this would be to keep them out of the
repo root to avoid clutter. My expectation is that most profiles will include
instructions to copy files to a new location to get picked up by the IDE, but
that's just a guess.

One last note here: regardless of the IDE used, every submitted project must
still be compilable with cmake and make./

## How to write a README

A well written README file can enhance your project and portfolio.  Develop your abilities to create professional README files by completing [this free course](https://www.udacity.com/course/writing-readmes--ud777).
