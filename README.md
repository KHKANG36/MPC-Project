## MPC project for vehicle dynamics 
This is a project for Udacity Self-Driving Car Nanodegree program. In this project, I implemented a MPC(Model Predictive Control) S/W code for autonomous vehicle control. All codes are written with C++ and tested on the Udacity simulator. 

## Requirement 
- C++- Udacity simulator : [here](https://github.com/udacity/self-driving-car-sim/releases)
- uWebSocketIO : [uWebSocketIO](https://github.com/uWebSockets/uWebSockets) for either Linux or Mac
- For window users : [Windows 10 Bash on Ubuntu](https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/) to install uWebSocketIO. 

## Other Important Dependencies
* cmake >= 3.5  
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1  
  * Linux: make is installed by default on most Linux distros  
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)  
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4  
  * Linux: gcc / g++ is installed by default on most Linux distros  
  * Mac: same deal as make - [install Xcode command line tools](https://developer.apple.com/xcode/features/)  
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* Ipopt install is required : [Click here for installation instructions](https://projects.coin-or.org/Ipopt/)
* CppAD library is required : [Click here for installation instructions](https://www.coin-or.org/CppAD/)
  
## Run the Project 
Once the install for uWebSocketIO is complete, the main program can be built and run by doing the following from the project top directory.

1. mkdir build
2. cd build
3. cmake ..
4. make
5. ./mpc

## About the Project 
1) The vehicle model 
- I used kinetic model 
optimizer to find the control inputs and minimize the cost function 
define the duration of trajectory, (T, N, dt)
define the vehicle model and constraints . vehicle model predict next positioin of 
x, y, psi, v, cte, error of psi, limitation - actuator limitation 
define the cost function 

pass current state -> model predictive controller -> optimization solver called -> return a vector of control inputs that minimize
the cost function 
this control input aplllied to the vehicle and repreat the loop 
2) Latency 
A contributing factor to latency is actuator dynamics. For example the time elapsed between when you command a steering angle to when that angle is actually achieved. This could easily be modeled by a simple dynamic system and incorporated into the vehicle model. One approach would be running a simulation using the vehicle model starting from the current state for the duration of the latency. The resulting state from the simulation is the new initial state for MPC.




I set 20, 0.05 total 50km/h (=13.8m/sec) So, 1-sec front expectation 
too much is increect. T is sould large, dt should small should be T is a couple of sec too low dt compext 
The tricky part in this project was 1)the weight of the steering difference cost. Without the weight, the change rate of steering is too high. So, I multiplied it by 1,000 weight, and the movement was smoother. 2) Latency handling. Since there was 100milisec delay between the input and response of actuator. To handle this, I initially apply the 100 milisec delay to next prediction state.   

## Discussion/Issues 
The vehicle in simulator drives pretty well up to about 50Km/h. Over the 50Km/h speed, however, the prediction is getting unstable, and the vehicle sometimes way off the road. I've tuned several parameters (such as weight, N, dt), but didn't get perfect solution. This would be future works to resolve in this project.  
