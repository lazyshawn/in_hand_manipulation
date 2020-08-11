## Extrinsic Dexterity: In-hand Manipulation with External Force
* “In-hand manipulation” is the ability to reposition an object in the hand.

* **Extrinsic dexterity** and **Intrinsic dexterity**.

By “extrinsic dexterity” we mean the manipulation of an object in the hand,
using resources extrinsic to the hand.
Given that it relies on resources intrinsic to the hand,
we refer to it as “intrinsic dexterity”.

| Dexterity           | Resources                                                   |
|---------------------|-------------------------------------------------------------|
| extrinsic dexterity | gravity, surfaces, inertal                                  |
| intrinsic dexterity | wiggle the fingers, or walk them along the object’s surface |



## In-Hand Manipulation via Motion Cones
* We can think of motion cones as a geometric representation of 
the underactuation inherent to frictional contacts.


## Design and Control of Roller Grasper V2 for In-Hand Manipulation
### Devices
Teensy 3.6 microcontroller, Dynamixel motors, Gearmotors,
SmoothOn MoldStar 16 (the semi-spherical silicone covers).

Mujoco 2.0 (Learning software).

### Keywords
TTL half-duplex asynchronous serial conmunication protocol,
PD control.

### Notes

![RollerGrasper_prototype](./pics/RollerGrasper_prototype.png =x200)
![Contact_motion_components](./pics/contact_motion_components.png =x200)

* Non-anthropomorphic robot grasper with active surfaces at fingertips.

* Active surfaces achieved by spherical rolling fingertips with 2-DOF, 
a pivoting motion for surface reorientation, and a continuous rolling motion 
for moving object.

* Instantaneous kinematics. Manipulate with a custom hard-coded control scheme,
and a learned through imitation learning.

* Common **approaches to design** grippers that can perform in-hand manipulation:
anthropomorphic hands, underactuated hands,
grippers with active surfaces (such as conveyors).

* The ability to re-orient an object to any direction
also lessens the need to use externally actuated degrees of freedom
(e.g. actuation of the robotic arm and wrist)
which simplifies the control scheme.

* Use an imitation learning based approach to learn a control policy
in order to arbitrarily transform an object while holding it.

* The first attempt at developing a grasper 
with active surfaces at the fingertips that transforms grasped objects 
through an imitation learning policy.

* There are two types of approaches to tackle an imitation learning problem.
Our approach is based on one *Behaviour Cloning* method
called DAgger(Dataset Aggregation).

| Approaches                     | Aims                                                                                                                   |
| ---                            | ---                                                                                                                    |
| Behaviour cloning              | train the agent to learn a mapping from observation to actions given demonstrations, in a supervised learning fashion. |
| Inverse Reinforcement Learning | learn a reward function that describes the given demonstrations                                                        |

* The object’s initial and target position and orientation $\Rightarrow$
the contact location and contact motion on the roller $\Rightarrow$
the pivot joint orientation and motions of the base and roller joints.

* We divide the contact motion into two components:
the component resulting from the motion of the base joint, 
$δx_{cb}$ , and the component resulting from the rolling, $δx_{cr}$ .

* Object motion $x_{obj}$ $\Rightarrow$
the motion at the contact frame $J_{obj}\delta x_{obj}$ $\Rightarrow$
component motion $δx_{cb}$, $δx_{cb}$ $\Rightarrow$
Joint angle (base joint $\theta_1$, pivot joint $\theta_2$, roller joint $\theta_3$).

* No path planning.
Simply compute the difference between the initial and target pose,
and set the desired velocity equal to a scaling factor multiplied
by this computed difference.
Works very well for convex objects whose radii of curvature
do not change drastically.

* The imitation model learn the optimal policy through given expert demonstrations.
The method is particularly useful when it is difficult to explicitly specify
the reward function or the desired policy.

* The state space is defined as $\vert S \vert \in \mathbb{R}^{35}$.

| Contents                                    | Definition                  |
| ---                                         | ---                         |
| current state of the grasper                | $s_1 \rightarrow s_9$       |
| current object position                     | $s_{10} \rightarrow s_{12}$ |
| current object orientation quaternion       | $s_{13} \rightarrow s_{16}$ |
| previous object position                    | $s_{17} \rightarrow s_{19}$ |
| previous object orientation                 | $s_{20} \rightarrow s_{23}$ |
| object initial position                     | $s_{24} \rightarrow s_{26}$ |
| object initial orientation (angle-axis)  | $s_{20} \rightarrow s_{23}$ |
| object termination position                 | $s_{20} \rightarrow s_{23}$ |
| object termination orientation (angle-axis) | $s_{20} \rightarrow s_{23}$ |

* The action space is defined as $\vert A \vert \in \mathbb{R}^{9}$,
and contains the nine joint positions $(a_1 \rightarrow a_9)$.

* We constructed a deep neural network to determine the actions
for each of the nine gripper joints.
The network consisted of **three fully connected** hidden layers
with **leaky ReLU** activations, expect at the output layer,
and 256 nodes in each hidden layer.

* Imitation learning step: 
generate *N* expert trajectories using previous control policy $\Rightarrow$
first trained policy $\pi^0(s^i)$ to predict the expert action
by minimizing the loss L between $\pi^0(s_j^i)$ and $a_j^i$
in a supervised approach (1) $\Rightarrow$
update policy according to DAgger (regression instead of classification).
$$L_j^i=\|\pi^0(s_j^i)-a_j^i\|_2,\forall s_j\in\vert\Gamma^i\vert,i\in N\tag{1}$$


## Robust Planar Dynamic Pivoting by Regulating Inertial and Grip Force
### Devices
The wrist joint: Maxon RE-40 DC motor (with a 4.3:1 gearbox), 
0.8Nm maximum continuous torque output.
The grip force: Maxon RE-36 DC motor.
Lever mechanism. Loadcell. Encoder.

### Keywords
**Uncertainty analysis** in coefficient matrix.
Robust control. 

### Note
* Traditional approaches based on friction compensation
do not work well for this problem, as we observe the trosional friciton
at the contact has large uncertainties during pivoting.

* Discontinuities of friction and the lower bound constraint on the grip force
all make dynamic pivoting a challenging task for robots.

* Propose a robust control strategy that directly uses friction
as a key input for dynamic pivoting, 
and show that active force control by regulating the grip force
significantly improves system stability.

* Embed a Lyapunov-based control law into a quadratic programming framework,
which ensures real-time computational speed and the existence of a solution.

* The contact brings three problems.
**Modeling Uncertainty**: the grasped area, contact friction force modeling.
A very detailed friction model is unnecessary. However, a closed-loop
strategy is preferred.
**Positive lower bound constraint** on grip force: 
a traditional nonlinear controller often commands large negative grip force
when trying to pull the system back from error.
**Friction discontinuity**.

* When the object is treated as an additional link of the robot
with frictionless joint, 
pivoting reduces to a passive last joint manipulator problem,
where partial feedback linearization is shown to be successful.
These approaches are extended in this paper in two ways.
Firstly, we add a robust control term to the feedback linearization control law,
so as to explicitly tackle the non-trivial, 
uncertain contact friction and some amount of slip during pivoting.
Secondly, we utilize the grip force as an additional source of control,
which brings notable stability improvement as well as new difficulties
in controller design.

* Robust control and adaptive control techniques were used to 
ensure convergence under friction uncertainty.

* Pivoting is a typical example of nonprehensile manipulation,
where the object is manipulated without a firm grasp.

* In this work, we will stick to a static model while relying on
hardware design to minimize unexpected dynamic frictional behavior.

* A smaller contact area will result in a more flat limit surface,
which makes rotation easier than slip.

* There is a significant hysteresis nonlinearity in motor stall torque;
hence we use a 5kg loadcell to provide grip force feedback,
then close the loop with PI plus feed-forward controller.

* There is a considerable amount of uncertainty in friction,
which is a compound result of a simple friction model,
non-perfect friction parameter estimation,
and the noise in grip force control.














