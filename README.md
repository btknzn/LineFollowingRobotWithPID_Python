THE CODE PART İS WRİTTEN FOR adafruit crickit.


# LineFollowingRobotWithPID_Python
The possible sensor array output when following a line are:

0 0 0 0 1---
0 0 0 1 1---
0 0 0 1 0---
0 0 1 1 0---
0 0 1 0 0---
0 1 1 0 0---
0 1 0 0 0---
1 1 0 0 0---
1 0 0 0 0---
Having 5 sensors, permits a generation of an "error variable" that will help to control the robot's position over the line, as shown bellow.

Let's consider that the optimum condition is when the robot is centered, having the line just bellow the "middle sensor" (Sensor 2). The output of the array will be: 0 0 1 0 0 and in this situation, the "error" will be "zero". If the robot starts to driven to the left (the line "seems move" right") the error must increase with a positive signal. If the robot start to move to the right (the line "seems move" left"), in the same way, the error must increase, but now with a negative signal.

The error variable, related with the sensor status will be:

0 0 0 0 1 ==> Error = 4
0 0 0 1 1 ==> Error = 3
0 0 0 1 0 ==> Error = 2
0 0 1 1 0 ==> Error = 1
0 0 1 0 0 ==> Error = 0
0 1 1 0 0 ==> Error = -1
0 1 0 0 0 ==> Error = -2
1 1 0 0 0 ==> Error = -3
1 0 0 0 0 ==> Error = -4


How does PID work?

The system calculates the ‘error’, or ‘deviation’ of the physical quantity from the set point, by measuring the current value of that physical quantity using a sensor(s). To get back to the set point, this ‘error’ should be minimized, and should ideally be made equal to zero. Also, this process should happen as quickly as possible. Ideally, there should be zero lag in the response of the system to the change in its set point.

More information can be found in many books and websites, including here:

http://en.wikipedia.org/wiki/PID_controller

Implementing PID

i) Error Term (e):

This is equal to the difference between the set point and the current value of the quantity being controlled.

error = set_point – current_value (in our case is the error variable get from the position of Robot over the line

ii) Proportional Term (P):

This term is proportional to the error.

P = error

This value is responsible for the magnitude of change required in the physical quantity to achieve the set point. The proportion term is what determines the control loop rise time or how quickly it will reach the set point.

iii) Integral Term (I):

This term is the sum of all the previous error values.

I = I + error

This value is responsible for the quickness of response of the system to the change from the set point. The integral term is used the eliminate the steady state error required by the proportional term. Usually, small Robots doesn't use the integral term because we are not concerned about steady state error and it can complicate the "loop tuning".

iv) Differential or Derivative Term (D):

This term is the difference between the instantaneous error from the set point, and the error from the previous instant.

D = error - previousError

This value is responsible to slow down the rate of change of the physical quantity when it comes close to the set point. The derivative term is used to reduce the overshoot or how much the system should "over correct".

Equation:

PIDvalue = (Kp*P) + (Ki*I) + (Kd*D)

Where:

Kp is the constant used to vary the magnitude of the change required to achieve the set point.
Ki is the the constant used to vary the rate at which the change should be brought in the physical quantity to achieve the set point.
Kd is the constant used to vary the stability of the system.
One approach to tune the loop can be Try-error tentative metod:

Set the Kd variable to 0 and tune the Kp term alone first. Kp of 25 is a good place to start in our case here. At last step we used a Kp of 50 that works very well with my Robot.
If the robot reacts too slowly, increase the value.
If the robot seems to react fast becoming unstable, decrease the value.
Once the robot responses reasonably, tune the derivative portion of the control loop (Kd). First set the Kp and Kd value each to the 1/2 of the Kp value. For example, if the robot responses reasonable with a Kp = 50, then set Kp = 25 and Kd = 25 to start. Increase the Kd (derivative) gain to decrease the overshoot, decrease it if the robot become unstable.
One other component of the loop to consider is the actual Sample/Loop Rate. Speeding this parameter up or slowing this down can make a significant difference in the robot's performance. This is set by the delay statements that you have in your code. It is a Try-error tentative metod to get the optimum result
