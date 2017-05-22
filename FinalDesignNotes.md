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
**No feedforward control is implemented.**

Each car can have two operation modes:

* _Free driving_: the car is in first position, and follows trajectory at constant speed.
* _Platooning_: the car is in second position, and follows first car at a constant distance while also controlling it's lateral distance to the wall.


#### Formal Description

This project introduces a new task called *Closed Trajectory Driving*.  
This task uses two capabilities: _Platooning_ and _Lateral Control_.  
The first capability is already implemented on the PiCar and is reused for this task.
The second capability adjusts the car's lateral position to keep it at a constant distance of the reference (within tolerance).

The capability _Lateral Control_ reuses the already present functions _Proximity Sensor Driver_ and _PiCar Driver_. It also uses a new function _Lateral Control Logic_, which computes the required motor speeds to stay at the desired distance from the wall.

The functions _Platooning Control Logic_ and _Lateral Control Logic_ both have as outputs speeds for the motors. These outputs can be conflicting (one function demands to increase speed while the other demands to reduce speed/go backward). The motors are thus **shared resources**.  
To remedy this, the outputs of both functions are sent through a 'buffer' function block _Wheel Speed Calculator_ that combines both outputs into one single input for the motors, trying to take the 'best of both worlds'. Only then is the function block _PiCar Driver_ triggered.

A four-levels diagram is shown in `<pi_car_concept_mod3.svg>`


#### Milestones

The defined milestones for this design are listed below. Most of the milestones concern **one single PiCar**, on which increasingly involved control strategies are applied, and whose behaviour is benchmarked until it can satisfactorally complete the closed loop trajectory.  
Only in a last milestone is a second PiCar added in platooning mode:

1. One car drives along a straight wall at constant distance using 'basic' control strategy (i.e. a modification of the platooning forward control for lateral control).
    * Benchmark: The car must drive along the wall at a distance of _(to be defined)_ within a tolerance of _(to be defined)_.

2. One car drives along a straight wall at constant distance using P(ID) control strategy.
    * Benchmark: The car must drive along the wall at a distance of _(to be defined)_ within a tolerance of _(to be defined)_. Initial response rise time and overshoot must be limited to _(to be defined)_ and _(to be defined)_ respectively.

3. One car drives along a straight wall at constant distance using P(ID) control strategy **and** state estimation scheme.
    * Benchmark: The car must drive along the wall at a distance of _(to be defined)_ within a tolerance of _(to be defined)_. Initial response rise time and overshoot must be limited to _(to be defined)_ and _(to be defined)_ respectively.

4. One car follows the full closed loop trajectory using same control as in milestone 3. 
    * Benchmark: The car must be able to follow a corner with a minimal radius of _(to be defined)_.

5. Both cars drive in a closed loop trajectory, one in front (_Free Driving_) and one in _Platooning_. 
    * Benchmark: The maximum distance from the second car to the first car must stay constant within a tolerance of _(to be defined)_.


#### SWOT

1. Strengths: More flexible conceptual design.

2. Weaknesses: More complex implementation due to desired flexibility.

3. Opportunities: Possibility for implementation in vehicles as lateral tracking control.

4. Threats: Low-lying obstacles cannot be detected by frontal sensor, motors cannot be operated with battery power, ...
