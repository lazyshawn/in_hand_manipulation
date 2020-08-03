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
Author's approach is based on one *Behaviour Cloning* method called DAgger.

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
















