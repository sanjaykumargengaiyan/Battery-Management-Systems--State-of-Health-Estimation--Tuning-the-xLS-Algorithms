In this code, you will explore a different way to determine the "x" and "y" data you use as input to the total-capacity estimation methods.

In the HEV scenarios, we accumulated data over a fixed duration of time for each (x,y) data pair used to update the total-capacity estimates 
(e.g., we updated once every 300 seconds, exactly). In two of the BEV scenarios, we instead accumulated (x,y) data pairs over random-length periods 
of time during which the battery pack was being charged via a plug-in charging mechanism. The first method is simple to apply, but does not tend to
give uncertainty bounds that are as tight as given by the second method. But, not all applications have periods of controlled operation, such as 
plug-in charging, so not all applications can use the second approach.

Here, you will look at a third approach. In this third approach, one (x,y) data pair is created every time the cell state-of-charge has changed by at 
least some threshold amount since the last update. A large threshold will maximize the value of the "x" information for an update to the estimate of 
total capacity. However, if the threshold is too large (e.g., 90%), you will never update the total-capacity estimates because the state-of-charge may 
never change by that amount during normal operation. Also, if the threshold is too large, the "y" data will accumulate current-sensor measurement errors 
over a long time, and will have high uncertainty regarding the correct value.

So: large threshold tends to provide higher-quality "x" data to the updates but noisier "y" data; small threshold tends to cause more frequent updates,
which is needed to track a moving total capacity and to adapt if the initial estimate is badly wrong.

In this code, you will experiment with this threshold "dz" as well as with the forgetting-factor "gamma" for the WTLS, TLS, and AWTLS methods. 
You will be graded based on the root-mean-squared error between the true capacity and estimated capacity using your set of tuning parameters (dz,gamma) ... 
the lower the error the better.

Change the dz and gamma tuning pairs for the WTLS, TLS, and AWTLS methods, in the tunexLS function, below to experiment.


The raw dataset was created as follows. First, the battery pack was "fully charged" to a top state of charge of 95%. Then, a random number of 
urban dynamometer driving schedule (UDDS) profiles were simulated, having the effect of discharging the cell to a ramdom point. Then, the battery pack was
charged in a constant-power mode. This discharging/charging protocol was repeated 200 times (simulating roughly 1 year of vehicle operation). The cell's capacity 
slowly decayed over this year of operation.

This simulation was performed with a full ESC cell model: one you learned about in the second course in this specialization. The output of the simulation was true
cell state-of-charge versus time as well as cell current versus time. Measurement noise was then added to the SOC measurements (1% standard deviation), and to 
the current-sensor measurements (assuming a 12-bit A2D).
