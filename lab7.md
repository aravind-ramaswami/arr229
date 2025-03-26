# Kalman Filter

In this lab, I developed a Kalman Filter and implemented in it simulation and on my physical robot. 

# Estimate Drag and Momentum

The Kalman Filter requires a state space model of our dynamic robot. A simple dynamic model for this robot is derived below. In this model, x is the position of the robot, m is the mass of the robot, and d is the drag coefficient 

--insert dynamic model analysis here--

We need to estimate the robot's mass and drag parameters. From the model above, we can see that they can be derived from the steady-state velocity and time.

--insert parameter derivation--

I programmed an open loop response for my robot to get these parameters. I chose a PWM value of 200, since this was approximately what I had been using to control my robot in labs 5 and 6. I drove the car toward a wall from above 3m away and braked when it got very close to the wall. I used this code to generate the open-loop response.

![open_loop_code](https://github.com/user-attachments/assets/e61316d4-ba7c-4f06-9d00-7fbfa3783ebb)
![open_loop_distance - Copy - Copy](https://github.com/user-attachments/assets/80df5888-0a88-41a9-bf1c-09c97ec646b4)
![control_input - Copy](https://github.com/user-attachments/assets/87b75150-c189-442e-9acb-2505537ad352)
![control_input](https://github.com/user-attachments/assets/b83efde1-7372-4d65-a123-c735e342f060)

Using the position data, I numerically estimated the velocity of the car. The following graphs show v(t), u(t), and x(t) for the car during the open-loop response. 



From these graphs, we can measure the steady state speed, 90% rise time, and speed at 90% rise time. 

--show values here--

Using these values, we can compute the mass and drag parameters.

--show mass and drag parameter calculation--

# Initialize KF (python)

The kalman filter 

# Implement and test your Kalman Filter in Jupyter (Python)

# Implement the Kalman Filter on the Robot

# Speed Up (Optional)
