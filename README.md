### ur16e-incremental-forming
ROS2 path planning for Roboforming - Tested

### What is ISF?
Single Point Incremental Sheet Forming is a flexible manufacturing process where a robot-mounted tool traces a programmed contour over a clamped metal sheet, progressively deforming it into a 3D shape. Unlike traditional stamping, it requires no moulds or dies — making it ideal for low-volume, high-customisation parts such as medical implants and aerospace prototypes.

### System Setup
- **Robot:** UR16e
- **Middleware:** ROS2 Humble · MoveIt2 · pymoveit2 · ur_robot_driver
- **OS:** Ubuntu 22.04 / WSL2
- **Sheet:** 0.5 mm aluminium, vertical fixture

#### Key Problem Solved — Polygon to Smooth Circular Trajectory

The core technical challenge anyone faces is that the built of such compliant robot supports the path plan as default in a small linear waypoints which produce less smooth and more polygon like path for circular trajectories. As evident, the initial implementation used pymoveit2's `move_to_pose()` for each waypoint individually. For a circle with 72 points, this meant 72 separate trajectory executions — the robot decelerates to a full stop at every point before replanning. The result was a **72-sided polygon** instead of a circle, with visible jerks at every waypoint. This is unacceptable for ISF where tool velocity must be continuous to produce uniform surface deformation.

To resolve this issue, the solution was to switch to MoveIt2's `GetCartesianPath` service, which accepts all 72 waypoints in a single call and returns one continuous blended trajectory. The robot executes the entire circle as one smooth motion with no intermediate stops.

#### Details to follow
