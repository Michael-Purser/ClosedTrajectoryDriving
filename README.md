<header>
<h3>ECS software assignment: Closed Trajectory Driving with Pi Cars</h3>
</header>

<h3> INFORMAL DESCRIPTION </h3>:

Two pi_cars drive in a closed trajectory while platooning.
The trajectory cannot cross over itself and cannot contain any curves that the cars can't manage.
*Examples of trajectory: circle, ellipse, "hippodrome", ...*
The reference is a wall in the shape of this trajectory.
The cars must stay at a given constant lateral distance of this wall within tolerance.

Each pi_car is equipped with two proximity sensors:
	* A frontal sensor: for platooning control
	* A lateral sensor: for trajectory control
These two sensors allow to control the car motions using position feedback.

Each car can have two operation modes:
	* Free driving: car is in first position, and follows trajectory at constant speed.
	* Platooning: car is in second position, and follows first car at a constant distance while also controlling lateral distance to the wall.


<h3> FORMAL DESCRIPTION </h3>

This project introduces a new task called *Closed Trajectory Driving*. 
This task uses two capabilities:
	* Platooning
	* Lateral Control
The first capability is already implemented on the pi_car and is reused for this task.
The second capability adjusts the car's lateral position to keep it at a constant distance of the reference (within tolerance).


FUNCTIONS:
----------

The second (new) capability reuses the function 
