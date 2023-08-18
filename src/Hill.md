# 8 Hill Climb

The purpose of this exercise is to program the Pololu robot to execute a specified task, namely to climb a ramp, detect when it reaches the top, turn around, and drive back down the ramp, all without falling off the edge of the ramp.

For this lab, you will need a suitable ramp like the one here:

<img src="img/Ramp.jpg" alt="Ramp"/>

This is pretty fancy one, but the key elements that you need are easy to construct from reasonably stiff cardboard. The key properties it needs are:

1. A sloped surface for the robot to climb with a roughly 15 degree slope.
2. A flat surface on top for the robot to turn around.
3. A light-colored surface.
4. Dark colored edges that the robot can detect to not drive off the edge.

The ramp shown above is about 4 feet long and 1.5 feet wide.

## 8.1 Prelab

1. Read through the subsequent sections of this lab and state all of the requirements that your robot program must satisfy. 

2. Review [Section 6.5, Line and bump sensors](https://www.pololu.com/docs/0J86/6.5) of the [Pololu 3pi+ 2040 robot User's Guide](https://www.pololu.com/docs/0J86). How do the line sensors work?  Is it possible to use the line sensors in combination with the bump sensors?

3. Recall from the [Sensors](./Sensors.md) lab that you used an accelerometer to measure pitch and roll of the robot. Assume you have a measurement _p_ of pitch and _r_ of roll. If the robot is sitting on a ramp and its _x_ axis is pointing straight down the hill, then what values should you see for _r_?  What should the sign of _p_ be (assuming the angle is given in the range -&pi; / 2 to &pi; / 2)?
You will use feedback control and adjust the wheel speeds to make _r_ approach the desired value. 

## 8.2 Cliff Sensing

For the hill-climbing exercise, your task is ultimately to get the robot to climb a ramp. So as to not damage the robot, you need to be sure that it will not drive off the edge of the ramp. Your first task, therefore, is to get the robot to take evasive action when it encounters the edge of the ramp.

Robots that navigate in the real world, such a Roomba vacuum cleaning robot, typically
include "cliff sensors," which detect when the edge of the robot hangs over an empty space,
for example at the top of a stairway.
These are often implemented with ultrasonic distance sensors pointing down.
The Pololu robot has infrared reflectivity sensors that can detect such cliffs (open space below the robot does not reflect infrared light), but to be even safer, your ramp can be equipped with dark bands on the edges as shown in the figure above.
Your task now is to program the robot to identify when its front end is above one of these bands and have it stop.
If you have a higher risk tolerance, you could do without the dark bands and use the IR sensors to detect when the front of the robot is hanging over the edge of the ramp.

**NOTE:** According to [Section 6.5, Line and bump sensors](https://www.pololu.com/docs/0J86/6.5) of the [Pololu 3pi+ 2040 robot User's Guide](https://www.pololu.com/docs/0J86), it is not practical to use the bump sensors in combination with the line sensors. Hence, in this lab, you will only use the line sensors.

First, you will get familiar with the reflectivity sensors.
Then you will use them.  Your tasks:

1. Examine and run the provided program `src/LineDisplay.lf`. How does this work? Use it to calibrate your robot on the ramp so that it reliably detects when the front of the robot is over the dark bands on the edges (or, if you choose the riskier option, over the edge of the ramp). Note that once your robot is calibrated, you should not have calibrate it again, so you can use the simpler `Line` reactor `src/lib/Line.lf` for subsequent exercises. However, if lighting conditions change or you run the robot on a different surface, you will likely need to recalibrate.

2. Create a Lingua Franca program that displays on the LCD display one of:

    - **No Edge**
    - **Forward Edge**
    - **Left Edge**
    - **Right Edge**

    depending on what the line sensors detect.
    
    **Hint:** A [center of mass](https://en.wikipedia.org/wiki/Center_of_mass) calculation may be useful.

    **Checkoff:** Show that your program detects edges of the ramp. You can manually push the robot towards the edge to check.

3. Create a Lingua Franca program that drives the robot forward, but when the line sensors detect edges, backs up, turns, and then moves forward again.  The direction in which the robot turns should be influenced by whether the edge is detected in front of the robot, to the left, or to the right.

    **Checkoff:** Show your robot navigating on the ramp and avoiding the edges.

## 8.3 Hill Climbing

Your final task is to add accelerometer and gyroscope measurements to your navigation code so that while the robot is on the slope of the ramp, it turns and drives towards the top. When it reaches the plateau at the top, it should turn 180 degrees and then drive down to the bottom of the ramp.  All the while, it should avoid the edges of the ramp.

**Hint:** To drive up or down the ramp, periodically adjust the wheel speeds to attempt to keep the roll measurement near zero.  That is, if the roll measurement is positive, adjust up the speed of one wheel and down the speed of the other.
If the roll is negative, perform the opposite adjustment.
If you make the adjustment proportional to the roll, then adjustments will get smaller as the robot more closely approximates heading straight up or down the ramp.
You may want to review Section 2.4, Feedback Control, of [Lee and Seshia](https://leeseshia.org).
You will want to keep the wheel speeds within reasonable bounds.


**Checkoff:** Show your robot driving to the top, turning around, and going down the ramp.

## Postlab

1. Just as you used roll measurement and feedback to control the direction in which the robot travels, you could use the encoders to control the speed at which it travels.  Describe how you would do this.

2. What were your takeaways from the lab? What did you learn during the lab? Did any results in the lab surprise you?