# LAB 2: IMU

The purpose of this lab was to get acquainted with IMU. 

# Setting up the IMU

I connected the IMU to the Artemis using the QWIIC connectors.

![PXL_20250211_004745628](https://github.com/user-attachments/assets/08f4c46b-6c00-4a2f-9dc0-4c3ccf092e69)

After connecting the IMU to the Artemis, I ran the example code to test its functionality. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/ELP9Sbhi3_w" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
Task 2: Serial

AD0_VAL = 1 in the example code, indicating that the ADR jumper on the back of the IMU has not been soldered together. This shows that the address of the IMU is 0x69 and allows for SPI communication. If the jumper is soldered together, SPI communication is not possible. It should be 1 because our ADR jumper is not soldered together and we want to use SPI communication.  

![PXL_20250211_004752363](https://github.com/user-attachments/assets/47667c23-2ae2-4e01-9f6c-de15d764a236)

I also added a visual signal that the IMU had successfully turned on (LED blinks three times)

<iframe width="560" height="315" src="https://www.youtube.com/embed/gODMkvEyXVw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
Task 2: Serial

# Accelerometer

# Gyroscope

# Sample Data

# Stunt


