### ECS software assignment: Closed Trajectory Driving with Pi Cars
Authors: Thijs Devos, Michael Purser

#### Informal Description

Two PiCars drive in a closed trajectory while platooning.  
The trajectory cannot cross over itself and cannot contain any curves that the cars can't manage.  
*Examples of trajectory: circle, ellipse, "hippodrome", ...*  
The reference is a wall in the shape of this trajectory. The cars must stay at a given constant lateral distance of this wall within tolerance. Basically, the cars follow a path which is a scaled version of the wall shape they are following.

Each PiCar is equipped with two proximity sensors:

* A frontal sensor: for platooning control  
* A lateral sensor: for trajectory control

These two sensors allow to control the car motions using position feedback.

Each car can have two operation modes:

* _Free driving_: the car is in first position, and follows trajectory at constant speed.
* _Platooning_: the car is in second position, and follows first car at a constant distance while also controlling it's lateral distance to the wall.


#### Formal Description

This project introduces a new task called *Closed Trajectory Driving*.  
This task uses two capabilities: _Platooning_ and _Lateral Control_.  
The first capability is already implemented on the PiCar and is reused for this task.
The second capability adjusts the car's lateral position to keep it at a constant distance of the reference (within tolerance).

The capability _Lateral Control_ reuses the already present functions _Proximity Sensor Driver_ and _PiCar Driver_. It also uses a new function _Lateral Control Logic_, which computes the required motor speeds to stay at the desired distance from the wall.

A four-levels diagram is shown in `<pi_car_concept_mod1.svg>`


#### Milestones

Because of the limited available hardware (currently only 1 sensor can be mounted on the car at a time), all the milestones agreed upon for this project concern the motion of **one single PiCar**, with a **laterally** mounted sensor:

1. The car drives along a straight wall at constant distance (within tolerance) using 'basic' control strategy (i.e. a modification of the platooning forward control for lateral control).

2. The car drives along a straight wall at constant distance (within tolerance) using P(ID) control strategy.

3. The car drives along a straight wall at constant distance (within tolerance) using P(ID) control strategy **and** angle compensation scheme.

4. The car follows the full closed loop trajectory using same control as in milestone 3.

Because the frontal distance sensor is not available, forward motion is achieved by implementing forward velocity directly in the function block _Lateral Control Logic_. The milestone implementation thus effectively only uses the capability _Lateral Control_.
