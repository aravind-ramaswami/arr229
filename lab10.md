# Lab 10: Localization (sim)

In this lab, I implemented a localization filter using a Bayes filter in simulation. I implemented several functions before using a provided driver script to run the Bayes filter algorithm. As an overview, the Bayes filter requires an odometry model and a sensor for the robot. Using a control input, the filter predicts the state of the car and updates its belief about the car's location with a sensor measurement. As more sensor measurements are recorded, the filter's estimate of the car's state becomes more accurate. In this lab, the world was discretized into a 12 x 9 x 18 grid. Each grid cell represented one possible state of the robot. The formal algorithm is shown below. 

<img width="667" alt="image" src="https://github.com/user-attachments/assets/745b25d9-4176-4336-a10e-22e90eb3b1a6" />

# Implementation

To implement the Bayes filter, I implemented five functions (compute_control(), odom_motion_model(), prediction_step(), sensor_model(), update_step()).

# compute_control

To implement compute_control(), I had to use a motion model for the car. The model motion enabled me to determine the control inputs necessary to move the robot from one state to another. In this lab, we used an odometry-based motion model. The control inputs to move from one state to another are derived in the following image. 

<img width="764" alt="image" src="https://github.com/user-attachments/assets/0a8d4d19-cde4-4c25-990e-39cb1eb06f37" />

I implemented the compute_control() function shown below using this motion model.

<img width="1047" alt="image" src="https://github.com/user-attachments/assets/238ae776-7263-4eac-a3bb-3a04a4c75696" />


# odom_motion_model

After compute_control(), I implemented the odom_motion_mode() function. I used compute_control() in this function to determine the probability that a given control input can move you between two states. This assumes Gaussian noise and that the noise in each direction (translation, rotation) was given to us. 

<img width="887" alt="image" src="https://github.com/user-attachments/assets/71ac80c0-ea8f-4e60-aee9-6561ba25fada" />


# prediction_step

With compute_control() and odom_motion_model() completed, I implemented the prediction_step() of the algorithm. In this step, the robot determines the control input needed to move from the last pose to the current pose. Using this information, the filter computes the belief that the robot moves from every possible state to every other possible state with several nested for loops. To speed up the computation, I disregarded updating the belief that the robot was in a state was too low. 

<img width="890" alt="image" src="https://github.com/user-attachments/assets/ddc4d49d-20f5-47ba-9877-13927cac94f1" />


# sensor_model

To compute the update_step(), I modeled the sensor measurements as a Gaussian. The robot computes the probability that a given set of sensor observations is close to the actual measured sensor observations. 

<img width="801" alt="image" src="https://github.com/user-attachments/assets/177ed0f9-adf4-438e-9f36-1324eadaa3f4" />


# update_step

Using the sensor model, I implemented the update step of the loop. This updates the belief in every grid cell by comparing the observed sensor measurements to the sensor measurements that would have been received if the robot were actually in that grid cell. It uses the sensor_model() function developed above. Afterwards, the probabilities are normalized to one. Initially, I implemented this function as a loop. After running a trial, it took 5 minutes to complete. I talked to my classmate, Anunth Ramaswami, who helped me implement a faster version using matrix multiplication. It significantly speeds up my run time. I have listed both implementations below. 

Loop-Based Implementation:

<img width="575" alt="image" src="https://github.com/user-attachments/assets/49876854-aa14-4c50-ab86-d3313f6e926c" />

Matrix Implementation:

<img width="676" alt="image" src="https://github.com/user-attachments/assets/af9a2885-f7f9-42ba-b55b-76c256b9a5fd" />

# Testing

These videos show my Bayes filter working. The first video is with the loop-based implementation, and the second video is using the matrix implementation. 

Trial 1:

sAafPrQ_Bjs

<img width="473" alt="fast_robots_l10_v1" src="https://github.com/user-attachments/assets/3777a0ea-c730-4bdf-8909-2f1ea344cd8f" />

<img width="528" alt="image" src="https://github.com/user-attachments/assets/b57ad5fc-52d2-4a98-aceb-379f14e89fc4" />

Trial 2:

O9dfm5hdWCc

<img width="364" alt="fast_robots_l10_v2" src="https://github.com/user-attachments/assets/bbc2080a-f992-4dc5-8155-891f9c14479a" />

<img width="543" alt="image" src="https://github.com/user-attachments/assets/e3f4f872-7b9a-40a7-a19f-b8351bef685f" />

From these trials, I saw that the filter does a very good job of tracking the robot's position through the map. It is best when the robot is traveling along narrow walls, when the sensor measurements are relatively consistent. It works the worst in the center of the map, where some measurements are obstructed by obstacles and others are not. Overall, the errors were relatively small, and the robot tracks the robot very well. Additionally, the simulation also shows that the odometry measurements are completely off, as the red curve is not close at all to the green curve. 

