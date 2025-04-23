# Lab 10: Localization (sim)

In this lab, I implemented a localization filter using a Bayes filter in simulation. I implemented several functions before using a provided driver script to run the Bayes filter algorithm. As an overview, the Bayes filter requires an odometry model and a sensor for the robot. Using a control input, the filter predicts the state of the car and updates its belief about the car's location with a sensor measurement. As more sensor measurements are recorded, the filter's estimate of the car's state becomes more accurate. In this lab, the world was discretized into 12 x 9 x 18 grid cells. Each grid cell representated one possible state of the robot. The formal algorithm is shown below. 

<img width="667" alt="image" src="https://github.com/user-attachments/assets/745b25d9-4176-4336-a10e-22e90eb3b1a6" />

# Implementation

To implement the Bayes filter, I implemented five functions (compute_control(), odom_motion_model(), prediction_step(), sensor_model(), update_step()).

# compute_control

To implement compute_control(), I had to use a motion model for the car. The model motion enabled me to determine the control inputs necessary to move the robot from one state to another state. In this lab, we used an odometry-based motion model. The control inputs to move from one state to another are derived in the following image. 

<img width="764" alt="image" src="https://github.com/user-attachments/assets/0a8d4d19-cde4-4c25-990e-39cb1eb06f37" />

Using this motion model, I implemented the compute_control() function shown below.

<img width="1047" alt="image" src="https://github.com/user-attachments/assets/238ae776-7263-4eac-a3bb-3a04a4c75696" />


# odom_motion_model

After compute_control(), I implemented the odom_motion_mode() function. In this function, I used compute_control() to determine the probability that a given control input can move you between two states. This assumes guassian noise and the noise in each direction (translation, rotation) was given to us. 

<img width="887" alt="image" src="https://github.com/user-attachments/assets/71ac80c0-ea8f-4e60-aee9-6561ba25fada" />


# prediction_step

With compute_control() and odom_motion_model() completed, I implemented the prediction_step() of the algorithm. In this step, the robot determines the control input needed to move from last pose to the current pose. Using this information, the filter computes the belief that the robot moves from every possible state to every other possible state with several nested for loops. To speed up the computation, I disregarded updating the belief that the robot was in a state was too low. 

<img width="890" alt="image" src="https://github.com/user-attachments/assets/ddc4d49d-20f5-47ba-9877-13927cac94f1" />


# sensor_model

To compute the update_step(), I modeled the sensor measurements as a gaussian. The robot computes the probability that a given set of sensor observations is close to the actual measured sensor observations. 

<img width="801" alt="image" src="https://github.com/user-attachments/assets/177ed0f9-adf4-438e-9f36-1324eadaa3f4" />


# update_step

Using the sensor model, I implemented the update step of the loop. This updates the belief in every grid cell by comparing the observed sensor measurements to the sensor measurements that would have been received if the robot was actually in that grid cell. It uses the sensor_model() function developed above. Afterwards, the probabilties are normalized to one. Initially, I implemented this function as a loop. After running a trial, it took 5 minutes to complete. I talked to my classmate, Anunth Ramaswami, who helped me implement a faster version using matrix multiplication. It significantly speed up my run time. I have listed both implementations below. 

Loop Based Implementation:

<img width="575" alt="image" src="https://github.com/user-attachments/assets/49876854-aa14-4c50-ab86-d3313f6e926c" />

Matrix Implementation:

<img width="676" alt="image" src="https://github.com/user-attachments/assets/af9a2885-f7f9-42ba-b55b-76c256b9a5fd" />



# Testing 

# Conclusion
