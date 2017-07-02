# SelfDrivingCar-P9-PID-Control
Self-Driving Car Engineer Nanodegree Program Term 2 Project 4

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
  * Run either `./install-mac.sh` or `./install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.
* Simulator. You can download these from the [project intro page](https://github.com/udacity/self-driving-car-sim/releases) in the classroom.

There's an experimental patch for windows in this [PR](https://github.com/udacity/CarND-PID-Control-Project/pull/3)

## Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid initial_p initial_i initial_d`. For example: `./pid 1 0 22`.

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

More information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/e8235395-22dd-4b87-88e0-d108c5e5bbf4/concepts/6a4d8d42-6a04-4aa6-b284-1697c0fd6562)
for instructions and the project rubric.

## Reflection
1. Describe the effect each of the P, I, D components had in your implementation.  
* P - The "Proportional" part makes the car steer in proportion to the cross track error (CTE). The proportional factor `Kp` is mulitplied by the CTE to calculate the steering angle. This results in the car overshooting the reference trajectory, then changing course and oscillating. Increasing `Kp` will make the the vehicle to oscillate faster. I settled on a proportional factor of 1.0 for the PID controller.

* I - The "Integral" part corrects systemic bias such as steering drift. The integral factor `Ki` is multiplied by sum of all the previous CTE. The steering drift is not an issue in the simulation, so `Ki` was set to 0. I tried some non-zero values of `Ki`, and they would make the vehicle oscillate and even drive off the track. 

* D - The "Differential" part makes the steering angle to decrease as it reaches the reference trajectory. This allows the car to approach the trajectory "gracefully" rather than osclillating wildly. It is calculated by multiplying the differential factor `Kd` by the derivative of CTE. This is equal to CTE at timestep `t` - CTE at timestep `t-1` divided by `delta t`. The larger `Kd` will make the steering angle decrease faster as it reaches the reference trajectory. The `Kd` value I used was 20.

2. Describe how the final hyperparameters were chosen.

The parameters are chosen through manual tuning. I first tried a set of small value parameters which were introduced in the Udacity lesson. However, the car drove off the track. I realized that the steering drifting is not a issue in the simulator. Therefore, I set `Ki` to be 0. And then I started to increase `Kp` and `Kd`. At first I increase them with small rate, `0.1` increament in each step. But it does not helps much. Then I tried to increase the `Kd` with larger rate, `5` increament in each step. After `Kd` increases to larger than `10` the car tends to drive on the track. After that I started to tuning the parameter `Kp`.   

The final parameters I am using are
```
Kp = 1.0, Ki = 0, Kd = 20.0. 
```
