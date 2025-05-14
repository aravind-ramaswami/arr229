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

The kinetic energy of the wheel is relatively easy.

<img width="100" alt="image" src="https://github.com/user-attachments/assets/ea417bf8-797e-49c4-8afd-add27b5833d6" />

The kinetic energy of the rod is more complicated and reqiures some manipulation. 

<img width="236" alt="image" src="https://github.com/user-attachments/assets/40019f93-bc27-4026-a183-dff8eab41684" />

The potential energy of the rod is also simple to compute.

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

We decided to implement a state space based controller, instead of the PID loop that we had used in Lab 6. The state space controller is typically defined as u = -Kx, where x is the state of the system. However, because of how we oriented the car and how the sensor gave as measurement, we actually had to apply a control signal u = Kx to stabilize the car. We used the DMP and gyroscope to measure both the angle and angular velcoity at the same time. This gave us better control of the car. Essentially then, the u = Kx was basically implementing a PD controller, but the gains were determined using state space control design techniques. We can use the place() command in matlab, which requires system matricies(A,B) and the eigenvalues you want the closed loop ststem (A + BK) to have.

Similar to lab 5, we used lumped system modeling to create two constants(alpha1, alpha2) that govern the dynamics of the car. 

<img width="159" alt="image" src="https://github.com/user-attachments/assets/6494b31b-e52d-49fc-817e-727fc358314b" />

With the RC car's measurements, we found that alpha1 = 6.21, alpha2 = 50 best describe the car. These parameters seemed to work well for the controller. After selecting these parameters, we had to discretize the system based on the loop speed. With testing, we determined that the loop speed was 0.017 seconds. With this, we discretized the system using euler discretization. 

<img width="217" alt="image" src="https://github.com/user-attachments/assets/629e3e9f-988f-4631-a1c9-d3e95c4a3e22" />

We played around in MATLAB with the discretized system and the place() command to generate different controllers given a set of eigenvalues. We eventually settled of eigenvalues of 0.87 and 0.75. Since we are operating in discrete time, these eigenvalues will stabilize the system. These eigenvalues returned controller gains of 2.373 and 0.4471. However, these system matricies assumed that the controller operates on radians, while our controller operates on degree measurements. Thus, we scaled these controller values by multiplying by pi/180 to give us 0.041 and 0.0078. In practice, we found that the 2nd gain value, which operates on the angular velocity, was too high, and made the system jerk too much. We lowered it down to 0.002, and the system worked better. 

Thus, our final gains were K = [0.04 0.002], which was used on the controller. The implemetation is shown in the code below. The actual code implementation is very similar to Anunth's lab 6 orientation controller (since we are using his robot), except that the gains are replaced by the ones we computed using the dynamic model. 



# Kalman Filter Implementation 

The kalman filter was essentially taken from the Aravind's lab 7 code. The system matricies A,B,C were derived above. Since the lab 7 code was written to be general, all we had to change the initialization near the top where the matricies and noise parameters are defined. We assumed very low noise for the DMP and IMU, since we trusted those measurements far more than the dynamics model, which is a very crude approximation. The code for the kalman filter is shown below, along with the initialization parameters. 


# Flip 

After the controller and kalman filter was implemented, we moved on to developing the flip code. The flip code was modified from Nita's lab 8 code, since she attempted the stunt. The flip code essentially drives the robot forward, brakes, and reverses it. The brake and reversal is what initiates a flip. There are delay times to control how long the car should accelerate for and reverse. These timers were tuned to achieve a flip. As the car is executing the flip, the code constantly checks if the pitch angle is above 60 degrees (measured from the vertical). When this happens, the code immediates switches from executing the flip to running the controller and kalman filter. The following state machine shows how this works. 

<img width="843" alt="image" src="https://github.com/user-attachments/assets/717c6743-884b-41c7-ae66-3e901303094b" />


The flip code is shown below. 

# Full System demonstration

# Work breakdown

# Conclusion
