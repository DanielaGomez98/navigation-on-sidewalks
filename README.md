# Navigation of a mobile robot on sidewalks
Autonomous navigation system for a basic differential robot for navigation on sidewalks, using the ROS2 navigation stack and ROS2 foxy distribution.

<img src="https://user-images.githubusercontent.com/91514084/182298717-b150532e-84f9-4b57-8411-2d589e3b5416.png" width="400"> <img src="https://user-images.githubusercontent.com/91514084/182294487-4ab4f765-b68b-4dd3-addb-983e6cc29e97.png" width="250"> 


## Project download
Just do a git clone or download the .zip inside the workspace.
```bash
git clone https://github.com/DanielaGomez98/navigation-on-sidewalks.git
```
Then, move the navigation_sidewalk directory to the root of the workspace.

## Clone the navigation2 repository
Before running the project, you must download the ROS2 foxy navigation stack repository, from this link: https://github.com/ros-planning/navigation2/tree/foxy-devel 
```bash
git clone https://github.com/ros-planning/navigation2.git
```

## Build the project
In the root of the workspace, run colcon build.
```bash
colcon build --symlink-install navigation_sidewalk
```
## Working knowledge

* ROS2 foxy
* Rviz2
* Gazebo
