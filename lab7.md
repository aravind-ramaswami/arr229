# Kalman Filter

In this lab, I developed a Kalman Filter and implemented in it simulation and on my physical robot. 

# Estimate Drag and Momentum

The Kalman Filter requires a state space model of our dynamic robot. A simple dynamic model for this robot is derived below. In this model, x is the position of the robot, m is the mass of the robot, and d is the drag coefficient. 

![image](https://github.com/user-attachments/assets/b90ca1ed-d902-43ed-8a06-40e2468b346a)

We can convert this equation into a state space model by tracking the position and velocity of the car as our states. This gives us the "A" and "B" matricies that we will need later. 

![image](https://github.com/user-attachments/assets/7e863b23-d103-4d91-a159-d3f7042c0d88)

We need to estimate the robot's mass and drag parameters. From the model above, we can see that they can be derived from the steady-state velocity and 90% rise time.

![image](https://github.com/user-attachments/assets/db53cfaa-54df-4685-b9e8-a171ec8f7686)

I programmed an open loop response for my robot to get these parameters. I chose a PWM value of 200, since this was approximately what I had been using to control my robot in labs 5 and 6. I drove the car toward a wall from above 3m away and braked when it got very close to the wall. I used this code to generate the open-loop response.

![open_loop_code](https://github.com/user-attachments/assets/e61316d4-ba7c-4f06-9d00-7fbfa3783ebb)

Using the position data, I numerically estimated the velocity of the car. The following graphs show v(t), u(t), and x(t) for the car during the open-loop response. 

![open_loop_distance](https://github.com/user-attachments/assets/1b41559a-539f-4f09-8a2f-96104d0c37fa)
![open_loop_velocity](https://github.com/user-attachments/assets/83f1ca85-9742-4c14-9934-1639f9e787c7)
![control_input](https://github.com/user-attachments/assets/b83efde1-7372-4d65-a123-c735e342f060)

From these graphs, we can measure the steady state speed, 90% rise time, and speed at 90% rise time. 

t_r = 3.15 
v_ss = 1.4

Using these values, we can compute the mass and drag parameters.

![image](https://github.com/user-attachments/assets/63dcf839-96bf-4f01-a8ef-46db9f8ac909)

With these parameters, we can start implementing the kalman filter in python. 

# Initialize KF (python)

The kalman filter is a state estimation algorithm that estimates a state vector over time by combining sensor data with a linear dynamic model. Importantly, the dynamics of the system must be linear, the sensor noise must be gaussian, and the procces noise also needs to be gaussian. Additionally, the sensor and state vector must have a linear relationship. Our dynamic and sensor models are linear, and we assume the noise for both is gaussian, so we can use a kalman filter for this system. 

The Kalman Filter requires a state space model for the system. Let the system be governed by the following state space model where A,B,C are the system matrices, x is the state, u is the control input, and Z is the sensor measurement

![image](https://github.com/user-attachments/assets/2a67b871-d499-4096-8bfb-8e0e9a4fcdd0)

Additionally, the kalman filter assumes the state dynamics have a covariance matrix defined as Q and the sensor has a covariance matrix defined as R. Assuming the robot is at state x_{t-1} and u_{t-1}, the kalman filter gives an estimated update for the new state when it receives a sensor measurement (Z) at time t. 

![image](https://github.com/user-attachments/assets/b112c1d3-694b-4622-9826-2ebfe542d253)

The kalman filter returns the new state x_{t} which would be used to give the next state. Importantly, the first two steps are called the "predict" step and they run regardless of whether a sensor measurement is received. If a sensor measurement is not received, the kalman filter returns the new state using only the dynamics model. If a sensor measurement is received, the output from the "predict" step is modified through the "update" step (lines 3 and 4) and the new state is returned. 

To initialize the kalman filter in python, I translated the equations above into a python function using the code given on the lab website. 

![kalman_python](https://github.com/user-attachments/assets/e22ac554-73fb-47e1-a13d-7088852581e1)

After this, I initialized my A,B,C matricies and converted them from continous time to discrete time. The equations for the conversion are given below. I also initialized the covariance matricies for the initial state, dynamic model, and sensor noise. I assumed the intiial state covariance matrix was uncorrelated. Additionally, I assumed the dynamic model covariance matrix was also uncorrelated. 

![image](https://github.com/user-attachments/assets/93e51fe0-fc5a-491a-8f8c-397c0f43a3f1)

# Implement and test your Kalman Filter in Jupyter (Python)

# Implement the Kalman Filter on the Robot

# Speed Up (Optional)
