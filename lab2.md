# LAB 2: IMU

The purpose of this lab was to get acquainted with IMU. 

# Setting up the IMU

I connected the IMU to the Artemis using the QWIIC connectors.

--insert image showing this--

After connecting the IMU to the Artemis, I ran the example code to test its functionality. 

--insert video showing this--

AD0_VAL = 1 in the example code, indicating that ADR jumper on the back of the IMU has not been soldered together. This shows that the address of the IMU is 0x69 and allows for SPI communication. If the jumper is soldered together, SPI communication is not possible. It should be 1 because our ADR jumper is not soldered together and we want to use SPI communication.  

--insert image showing unsoldered ADR Jumper--

I also added a visual signal that the IMU had successfully turned on (LED binks three times)

--insert video showing this--

# Accelerometer

# Gyroscope

# Sample Data

# Stunt


