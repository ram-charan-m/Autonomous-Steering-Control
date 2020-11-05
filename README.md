# CarND-Controls-PID
Self-Driving Car Engineer Nanodegree Program

---

## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1(mac, linux), 3.81(Windows)
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

Fellow students have put together a guide to Windows set-up for the project [here](https://s3-us-west-1.amazonaws.com/udacity-selfdrivingcar/files/Kidnapped_Vehicle_Windows_Setup.pdf) if the environment you have set up for the Sensor Fusion projects does not work for this project. There's also an experimental patch for windows in this [PR](https://github.com/udacity/CarND-PID-Control-Project/pull/3).

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 

Tips for setting up your environment can be found [here](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/23d376c7-0195-4276-bdf0-e02f1f3c665d)

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

### Effect of each component of PID controller

1. Proportional parameter: 
	* This parameter effects the contol output in proportion to the error. Increasing this parameter gain increases the control signal in proportiona to the error. A lone P controller exhibits marginally stable behavior. 
    * A lone P controller for steering control initially started off smoothly exhibiting stable behavior. However, with every passing moment its amplitude of oscillations increased, making the vehicle unstable.

2. Differential paramter: 
	* This parameter effects the control input by keeping rate of change of error in check. It is often deployed in tandem with Proportional paramter for eliminating the latter's oscilatary behavior. It is often used to eliminate overshoots as a dampening solution.
    * A well tuned PD steering controller proved sufficient for stably completing the course. The D parameter eliminated the increase in amplitude of oscilations by keeping the rate of change of error in check.

3. Integral paramter:
	* This parameter effects the control input by keeping accumulating errors in check. It is most often deployed to eliminate steady-state error by countering system biases being built over a period of time. 
    * The effect of adding an I paramter to a PD controller is very subtle and in this case almost unnecessary. With the absence of accumulating biases, the I gain was kept very small inorder not to interfere with a stable PD controller. A large I gain resulted introducing an unecessary bias which resulted in deviation of vehicle from lane center.

### Hyperparameter tuning method
I took the approach of manual tuning as it seemed sufficient and also provides the opportunity to understand each parameter in isolation. The following approach worked best for me:
* Initially, set all gains to zero.
* Increase only P until the vehicle starts to exhibit marginally stable behabior.
* Increase only D until the vehicle oscillations reduce to a stable behavior.
* Repeat steps 2 and 3 until satisfactory behavior is achieved.
* Increase only I gain only if necessary, else keep it as low as possible.
    
My steering controller gains are:
* Kp = 0.1
* Kd = 1.0
* Ki = 0.00001
These gains resulted in an average error value of less than 0.5.