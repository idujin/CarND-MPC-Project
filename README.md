# **Model Predictive Control (MPC)** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

---

## The Model
  In this project, I used a kinetic model that ignores tire forces, gravity and mass. This model is less accurate than a dynamic model but I thought the used model would have enough accuracy in this simulation environment. The used state vector has position (x, y), orientation (psi), velocity (v), cross track error (cte) and psi error (epsi). The actuator vector has steering angle (delta) and accelation (a). The used quation is as follows: 
  
  * x[t+1] = x[t] + v[t] * cos(psi[t]) * dt 
  * y[t+1] = y[t] + v[t] * sin(psi[t]) * dt
  * psi[t+1] = psi[t] - v[t] * delta[t] / Lf * dt
  * v[t+1] = v[t] + a[t] * dt
  * cte[t+1] = f(x[t]) - y[t] + v[t] * sin(epsi[t]) * dt
  * epsi[t+1] = psi[t] - psides[t] + v[t] * delta[t] / Lf * dt
  
  where Lf is the distance between the center of mass of the vehicle and dt is time step.
  Since the sign of the angle of the model in the lecture and the simulator is the opposite, I changed the psi update equation. 

## Timestep Length and Elapsed Duration (N & dt)
  At first, I tried 10 N and 0.1 dt which are provided in the example code of the Q & A video but these parameters did not fit without latency compensation. When there is no code for handling 100 millisecond latency, the latency should be added to the dt which is proper at no latency environment. In my case the dt was 0.2 but I restrored the value to 0.1 after handling 0.1s of the latency.
##Polynomial Fitting and MPC Processing
  Before processing polynomial fitting, I transformed the map coordinate position to the vehicle coordinate position. For fitting waypoints, the third order polynomial is used.

##Model Predictive Control with Latency
  To deal with the 100 millisecond latency, I set the model to start with current state which is after 100 milliseond state from input state. That code can be found in line 104 to 107 in main.cpp. I referenced [How to incorporate latency into the model] (https://discussions.udacity.com/t/how-to-incorporate-latency-into-the-model/257391/18) to solve this latency issue.