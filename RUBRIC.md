## Rubric points

- **MPC description**

For Model Predictive Control project we have used kinematic model. The state of the model is composed of vehicle's x and y coordinates,  orientation angle psi, cross track error and psi error. The vehicle is controlled with accelation and steering angle (delta) actuators. The updated state of vehicle is dependent on previous state together with actions and described by equations below: 

![equations](./eqns.png)


- **Selection of N and dt**

Since the testing track did include a lot of turns, I did not want to use model that would predict too much into future. At the same time I needed model that would anticipate the sharp turns that might happen in the future. My strategy was to keep T = N*dt in the region of 1 to 2 seconds. I have started with N = 20 and dt = 0.1. This setting however did let to some erratic turns since the very beginning. Per advice of the walk through for the project I tried using N = 10 and dt = 0.1. This met my strategy of keeping T in range of 1-2 seconds and resulted in smoother behaviour. Additionally setting N = 10 reduced the computational complexity per step. 

- **Polynomial Fitting and MPC Preprocessing**

Initially the waypoints were tranformed to vehicle's perspective (given in main.cpp lines 98-104). This way vehicle's x and y coordinates were at the origin and orientation angle was 0 which simplified the polynomail fitting. 

- **Model Predictive Control with Latency**

To deal with the 100 ms latency, I have chosen to use actuation values from 2 steps before, for update steps from 2 to N-1 (MPC.cpp lines 102-104).  Additionally I have added extra cost to cost function that dampens speed while the vehicle is steering (MPC.cpp line 62). To make driving even more careful I have increased the weights for the costs of cte and epsi to 5000. These additions lead to quite careful steering. 

