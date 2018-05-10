# CarND-Path-Planning-Project
Self-Driving Car Engineer Nanodegree Program
   
### Simulator.
You can download the Term3 Simulator which contains the Path Planning Project from the [releases tab (https://github.com/udacity/self-driving-car-sim/releases/tag/T3_v1.2).

### Goals
In this project our goal is to safely navigate around a virtual highway with other traffic that is driving +-10 MPH of the 50 MPH speed limit. 

We are provided with the car's localization and sensor fusion data, there is also a sparse map list of waypoints around the highway. The car should try to go as close as possible to the 50 MPH speed limit, which means passing slower traffic when possible, note that other cars will try to change lanes too. The car should avoid hitting other cars at all cost as well as driving inside of the marked road lanes at all times, unless going from one lane to another. The car should be able to make one complete loop around the 6946m highway. Since the car is trying to go 50 MPH, it should take a little over 5 minutes to complete 1 loop. Also the car should not experience total acceleration over 10 m/s^2 and jerk that is greater than 10 m/s^3.

## REFLECTION

I have completed this project by implementing each required part in a single file i.e main.cpp . It could have been easily implemented by making different class files for each part too but I wanted to avoid the overhead of jumping from one file to another.

Since in the begining I didnt have much idea of how to start with this project so I went through the walkthrough of the project provided in the classroom. It helped me a lot in building up my approach for implementing this project.

The code model for generating paths is divided into three parts:
 1. PREDICTION
 2. BEHAVIOUR
 3. TRAJECTORY GENERATION
 
#### PREDICTION: 

For prediction I have used the sensor_fusion data provided by the simulator. As it contains vector information of the vehicles on road. 
Here I have predicted the following :
1.If there is any car in the lane of ego_car
2.If the lane towards the right of the ego_car is free
3.If the lane towards the left of the ego_car is free
by taking the following flags : (intially all these flags are set to false)

```bool too_close = false;
bool left_free = false;
bool right_free = false;
```
The implementation of the prediction part is present in main.cpp at line 261 to 304. Where I have calculated lane and position of each car , and considered it to be dangerous for the ego_car if its distance from it is less than 30m.

#### BEHAVIOUR:

Once we have the flags updated from the prediction ,here we will decided what to do based on the boolean value of the flags.
It has the following behaviors : 
1. Checks if the vehicle in front of the ego_car is within 20m or not
If it is then change lane based upon the flag and current lane of the ego_car . 
Initially i was using 30 as the distance as suggested in the classroom but then i decreased it to 20 as the car was taking much time to change the lane as 30 miles was quite large range to predict the vehicles. By using 20 miles i got better results and the car was able to complete 4.32 miles in less time compared to 30 miles as distance.
2. Maintain the speed and lane of the ego_car. By decideing whether to accelerate or deaccelerate and return to the middle lane if it is free so as to avoid the possibility of getting stuck among the vehicles.

The implementation of the behavior is present in main.cpp at line 306 to 334.

#### Trajectory :

In this part we compute the trajectory for the decision taken by the behaviour. For this I have followed the code as suggested in the classroom.

The implementation of the trajectory is present in main.cpp at line 336 to 442.

##OUTPUT
All the following points of the rubrics are fullfilled i.e. 

1.The car is able to drive 4.48 miles without incident. 
	![image1](output_image/image1.png)\
2. The car drives according to the speed limit.
3. Max Acceleration and Jerk are not Exceeded.
4. Car does not have collisions.
5. The car stays in its lane, except for the time between changing lanes.
6. The car is able to change lanes

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

