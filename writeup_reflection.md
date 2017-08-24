# CarND-Controls-PID Reflection
Self-Driving Car Engineer Nanodegree Program

---

## Describe the effect each of the P, I, D components had in your implementation.

**Proportional (P)**

The P component dictates how much the car steers based on the CTE.

The higher the P value, the greater the oscillations will be.  Once the car begins to oscillate immensely left and right, it's hard for it to recorrect itself back to the smooth driving behavior at the start of the track.  

However, having low P values will make sharp turns very difficult to clear.  

**Integral (I)**

The I component aims to cancel the steering drift by using the sum of all previous CTE observed to correct the vehicle's motion.  A high value of this component can make the steering overshoot immensely, causing the vehicle to turn in circles.  At miniscule values this component appears to help steering at corners but creates small inconsistencies when the car returns to a straight path.  

**Differential (D)**

The D component dictates the amount of change in the model and helps minimize overshooting drastically.  Having lower values helps the smoothness of the vehicle and having higher values compliments how well a vehicle can turn sharp corners.  

## Describe how the final hyperparameters were chosen.

The final hyperparameters were chosen through manual tuning.  

The main issue I believe is that we have to work with constant velocity. How much a vehicle should turn is greatly based on it's velocity.  Since the controller doesn't have brake implementations and it does not recognize when sharp turns are approaching, we have to take these factors into consideration.  By lowering the velocity we can avoid many of these issues but it's not optimal for certain areas of the track where we can travel at much higher speeds.   

First, leaving the I component at 0 helps minimize the confusion because of varying CTE at straight paths and corners.  Having CTE data beforehand can help the controller immensely, since CTE starts at 0, using this component will cause the car to drive wildly initially and it might go off-track before it corrects itself.

I started with a high P value to ensure the car make clear all the turns and kept lowering it to keep the oscillations under control.   At values under -0.01, it seemed to be able to make smooth transitions between the straightaways and corners but it wasn't able to consistently clear certain sharp corners.  Having high differential values helped clear those sharp corners and I was able to find a balance between the two components which enabled to car to consistently clear the track.  I essentially implemented a PD controller with values of -0.095 for P and -2.5 for D.   
