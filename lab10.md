# Lab 10: Localization (sim)

In this lab, I implemented a localization filter using a Bayes filter in simulation. I implemented several functions, before using a provided driver script to run the bayes filter algorithm. As an overview, the Bayes filter requires an odometry model and sensor for the robot. Using a control input, the filter predicts the state of the car and updates its belief about the car's location with a sensor measurement. As more sensor measurements are recorded, the filter's estimate of the car's state becomes more accurate. The formal algorithm is shown below. 

# Implementation

To implement the Bayes filter, I implemented five functions (compute_control(), odom_motion_model(), prediction_step(), sensor_model(), update_step()).

# compute_control

<img width="1047" alt="image" src="https://github.com/user-attachments/assets/238ae776-7263-4eac-a3bb-3a04a4c75696" />


# odom_motion_model

<img width="887" alt="image" src="https://github.com/user-attachments/assets/71ac80c0-ea8f-4e60-aee9-6561ba25fada" />


# prediction_step

<img width="890" alt="image" src="https://github.com/user-attachments/assets/ddc4d49d-20f5-47ba-9877-13927cac94f1" />


# sensor_model

<img width="801" alt="image" src="https://github.com/user-attachments/assets/177ed0f9-adf4-438e-9f36-1324eadaa3f4" />


# update_step

<img width="575" alt="image" src="https://github.com/user-attachments/assets/49876854-aa14-4c50-ab86-d3313f6e926c" />


<img width="676" alt="image" src="https://github.com/user-attachments/assets/af9a2885-f7f9-42ba-b55b-76c256b9a5fd" />



# Testing 

# Conclusion
