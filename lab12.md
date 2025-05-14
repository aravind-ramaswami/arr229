# LAB 12: 

Instead of doing the normal lab task of navigation, I decided to follow in the lead of Stephan Wagner from last year, who balanced the car like an inverted pendulum using a PID controller. In his report, he mentioned that it would be cool to get the car to flip up and balance as an extension to his work. I decided that I wanted to tackle this challenge. To tackle this challenge, I decided to partner up with Anunth Ramaswami and Nita Kattimani. 

# Overview

The goal of the project was to develop an algorithm that allowed the car to start horizontal on the ground, accelerate forward, perform a flip, and balance on its rear wheels. We decided to approach this task by breaking up the algorithm into several discrete steps to make the problem tractable. We needed to design a controller that can stabilize the car when it is on its rear wheels. To improve the controller, we decided to add on a kalman filter to generate better sensor measurements for the controller. At this stage, we would have a controller that can reliably stabilize the car once it had flipped up past a certain angle. Finally, we also needed code that would drive the car forward and initiate a flip tp get the car to this algorithm. With this general workflow in mind, we had to develop a system model, create a stabilizing controller, create a kalman fitler, create flip code, and integrate it all togethor. The following sections in this report explain each part of the algorithm below. 

# System Modeling

We are using a kalman filter and a state space based controller, so we need a state space model of the system. The car balancing on its backwheels can be approximated as a wheel with a rod attached it. The diagram below shows the simplified model of the system. 

We can derive the dynamics of this system using lagrangian mechanics. Lagrangian mechanics is an alternative method for deriving dynamics models that uses energy of the system instead using forces and torques along with Newton's 2nd law. We can assume the following parameters for the system: x, theta, l, m, M

x: x position of the wheel

l: length of the "rod"

m: mass of the rod

M: mass of the "wheel"

I: moment of inertia of the rod about its center of mass

The 1st step when using lagrangian mechanics is to derive the total kinetic and potential energy of the system. The potential energy of the system is determined by the postion of the rod's center of mass. The wheel does not contribute to the potential energy since it does not leave the ground, so we can defined potential energy as the center of the wheel. Similarly, we can define zero potential energy for the rod when the rod is pointing straight up. The kinetic energy of the system is the kinetic energy of the rod + the kinetic energy of the wheel. These are derived below. 

<img width="100" alt="image" src="https://github.com/user-attachments/assets/ea417bf8-797e-49c4-8afd-add27b5833d6" />

<img width="236" alt="image" src="https://github.com/user-attachments/assets/40019f93-bc27-4026-a183-dff8eab41684" />

<img width="88" alt="image" src="https://github.com/user-attachments/assets/7fb5d4b8-cf2c-4343-9390-bcf1fbe3e9df" />

After computing the kinetic and potential energy, we can derive the Lagrangian of the system, which is simply the kinetic energy - potential energy. 

<img width="313" alt="image" src="https://github.com/user-attachments/assets/1214c4cc-e442-4187-9c6f-bd0a8e227e40" />

After the lagrangian of the system has been derived, we can use the Euler - Lagrange equations to derive the equations of motion for the system. 

<img width="169" alt="image" src="https://github.com/user-attachments/assets/12c8beff-9f73-41a7-9033-0de724e90c4b" />

q_i are the "generalized coordinates" of the system, which are x and theta in our case. Q_i are the "generalized forces" which are simply the external forces acting on the system which is zero for theta and the force applied to the wheel for x. With this, we can write down the equations of motion for x and theta using the euler lagrange equation. 

<img width="329" alt="image" src="https://github.com/user-attachments/assets/8d719eb4-f96f-4174-870d-9b32a09a3a15" />

Thus, the nonlinear equations of motion are:

<img width="206" alt="image" src="https://github.com/user-attachments/assets/008f36f4-fe78-47d3-beb1-8510667f2e24" />

To create a state space model, we need to linearize the system. Formally, we would have to compute the jacobians around an equilibrium point, but we can use linearize this system by noting some properties of theta for small angles. The linearization is shown below. 

<img width="131" alt="image" src="https://github.com/user-attachments/assets/ae34620a-e110-448d-a2ce-5e004aab5d96" />

After the equations are linearized, we can solve for theta so it is not dependent on x. 

<img width="308" alt="image" src="https://github.com/user-attachments/assets/c98b2e88-adef-4eb2-8115-e893ed83426d" />

At this point, we can ignore the x equation, since we only care about stabilizing the angle. We did not care about the x position, so we can ignore that equation. This leaves us with the final linearized equation for theta.

<img width="158" alt="image" src="https://github.com/user-attachments/assets/922af6b0-47b3-42ef-a823-3a5e7e0cf553" />

We can develop a state space model from this linearized model. We were able to measure both the angle and angular velocity (this is described in the controller design section).

<img width="109" alt="image" src="https://github.com/user-attachments/assets/44bcadf4-9e62-4827-a03b-0738e4db1280" />

<img width="233" alt="image" src="https://github.com/user-attachments/assets/b0d8fdb3-cc07-4d6a-b408-387ac4a5347a" />

Finally, we can check the controllability and observability of our system, which are neccessary to develop a kalman filter and state space controller. 

<img width="227" alt="image" src="https://github.com/user-attachments/assets/fd9adc62-16db-4bed-bd95-c7d353e0891a" />

Since the system is controllable and observable, we can develop a state space controller and a Kalman Filter. 

# Controller Design 

# Kalman Filter Implementation 

# Flip 

# Full System demonstration

# Work breakdown

# Conclusion
