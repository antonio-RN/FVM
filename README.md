# FVM

This personal project aims to develop a full-vehicle simulation model that takes into account major factors in chassis dynamics: tires and their interaction with road surface, suspension kinematics and its influence in tire force / moment generation and whole-vehicle movement. 

It is written in Modelica via OpenModelica, using the Modelica Standard Library v4.0.0 (older versions of the library will not work due to structure changes).

Currently on v1, it will be updated (non-regularly) to improve accuracy and correct mistakes. The objective is to have different final versions to be able to compare how they reflect changes in vehicle setup one against the others.

## Model description

The system uses a coordinate system fixed to the vehicle CG ground projection. X-axis is positive in the vehicle heading direction (centerline), Y-axis is positive in the right direction and Z-axis is positive downwards (all the Z-axis coordinates should be negative).

The full model comprises 2 sub-models:

- Independent wheel suspension (*Quarter_Susp*): fully vertical suspension (Z-axis) sub-system with the main mechanical elements represented by their mathematical 1D counterpart. Four instances of this sub-model make the full suspension system (*Full_Susp*), wich takes into account the sprung body rotation (X- and Y-axis) and vertical suspension work (Z-axis). From ground to upper joint:

  - Tire: spring-damper assembly always connected to ground (z=0).

  - Unsprung mass: puntual mass that accounts for wheel + half suspension links mass.

  - Suspension coil / damper: main spring and damper actuators, in parallel and the same lenght end-to-end. Upper joint is the chassis fixing point.

- Intependent rotating wheel and tire (*Quarter_Wheel*): sub-system that takes into account wheel rotational inertia, drivetrain / brakes moments applied to wheel and tire force / moment generation. This last part is based on a simplified Pacejka Magic Formula v6.1, calculated with constant coefficients that takes the normal load from the suspensions sub-system. Four instances of this sub-model make the full chassis system (*Full_Chassis*), which takes into account the whole vehicle translational movements (X- and Y-axis) and yaw attitude (Z-axis rotation)

## Model limits

As of today, the vehicle model does take into account:

- Vertical suspension travel of wheel
- Wheel rotational inertia
- Load transfer between wheels / axles
- Pitch and roll movement of sprung chassis
- Tire force / moment generation vs slip (both longitudinal and lateral)
- Tire force / moment generation vs normal load (direct proportionality)

The vehicle model deoes NOT take into account:

- Non-vertical components of suspension (e.g. kingpin angle)
- Wheel lift from ground
- Camber effects
- Wheel attitude change with suspension travel (e.g. roll steer)
- Tire force / moment generation variations with slip quantities (e.g. combined slip friction reduction)
- Tire force / moment generation variations with suspension travel

This list will be updated with coming model improvements.

## Model usage

Most of vehicle properties are contained within *variables* file. Basic input for simulation (e.g. torque applied to wheels, wheel rotation speed) is defined there also (*input* section at the end), as well as initial longitudinal speed. 
*FVM_sim* file must be the model simulated within OpenModelica.
