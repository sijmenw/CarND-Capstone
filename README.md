# Team Hyperloop Capstone project

Team members

| Name| Udacity Account Email Address |
| -- | -- |
| Praveen Bandaru | praveen6.007@gmail.com  |
| Muhammad Nassef | muhamad.nassef@gmail.com |
| Rangarajan Ramanujam | r.rangarajan1991@gmail.com  |
| Anudeep Sekhar|  anudeep.sekhar@gmail.com|
| Sijmen van der Willik| sijmenwillik@hotmail.com  |

 
This is the project repo for the final project of the Udacity Self-Driving Car Nanodegree: Programming a Real Self-Driving Car. For more information about the project, see the project introduction [here](https://classroom.udacity.com/nanodegrees/nd013/parts/6047fe34-d93c-4f50-8336-b70ef10cb4b2/modules/e1a23b06-329a-4684-a717-ad476f0d8dff/lessons/462c933d-9f24-42d3-8bdc-a08a5fc866e4/concepts/5ab4b122-83e6-436d-850f-9f4d26627fd9).

# System Architecture Diagram

The following is a system architecture diagram showing the ROS nodes and topics used in the project. You can refer to the diagram throughout the project as needed. The ROS nodes and topics shown in the diagram are described briefly in the  **Code Structure**  section below.

[](https://classroom.udacity.com/nanodegrees/nd013/parts/6047fe34-d93c-4f50-8336-b70ef10cb4b2/modules/e1a23b06-329a-4684-a717-ad476f0d8dff/lessons/462c933d-9f24-42d3-8bdc-a08a5fc866e4/concepts/455f33f0-2c2d-489d-9ab2-201698fbf21a#)

![](https://d17h27t6h515a5.cloudfront.net/topher/2017/September/59b6d115_final-project-ros-graph-v2/final-project-ros-graph-v2.png)

# Code Structure

Below is a brief overview of the repo structure, along with descriptions of the ROS nodes. The code for the project will be contained entirely within the  `(path_to_project_repo)/ros/src/`  directory. Within this directory, you will find the following ROS packages:

## (path_to_project_repo)/ros/src/tl_detector/

This package contains the traffic light detection node:  `tl_detector.py`. This node takes in data from the  `/image_color`,  `/current_pose`, and  `/base_waypoints`  topics and publishes the locations to stop for red traffic lights to the  `/traffic_waypoint`  topic.

The  `/current_pose`  topic provides the vehicle's current position, and  `/base_waypoints`  provides a complete list of waypoints the car will be following.

We retrained a TensorFlow model (**[ssd_mobilenet_v1_coco_11_06_2017](http://download.tensorflow.org/models/object_detection/ssd_mobilenet_v1_coco_11_06_2017.tar.gz)**) on images of traffic lights at the Udacity's test site obtained from different rosbag files . The trained model was then used as a frozen inference graph to detect the state of the traffic light. The detailed explanation of the training approach can be found at [Traffic-Light-Classifier](https://github.com/praveenbandaru/Traffic-Light-Classifier). In this project, we implemented both a traffic light detection node and a traffic light classification node. Traffic light detection code is under  `tl_detector.py`, whereas traffic light classification code is at  `../tl_detector/light_classification_model/tl_classfier.py`.

[](https://classroom.udacity.com/nanodegrees/nd013/parts/6047fe34-d93c-4f50-8336-b70ef10cb4b2/modules/e1a23b06-329a-4684-a717-ad476f0d8dff/lessons/462c933d-9f24-42d3-8bdc-a08a5fc866e4/concepts/455f33f0-2c2d-489d-9ab2-201698fbf21a#)

![](https://d17h27t6h515a5.cloudfront.net/topher/2017/September/59b6d189_tl-detector-ros-graph/tl-detector-ros-graph.png)

## (path_to_project_repo)/ros/src/waypoint_updater/

This package contains the waypoint updater node:  `waypoint_updater.py`. The purpose of this node is to update the target velocity property of each waypoint based on traffic light and obstacle detection data. This node will subscribe to the  `/base_waypoints`,  `/current_pose`,  `/obstacle_waypoint`, and  `/traffic_waypoint`  topics, and publish a list of waypoints ahead of the car with target velocities to the  `/final_waypoints`  topic.

[](https://classroom.udacity.com/nanodegrees/nd013/parts/6047fe34-d93c-4f50-8336-b70ef10cb4b2/modules/e1a23b06-329a-4684-a717-ad476f0d8dff/lessons/462c933d-9f24-42d3-8bdc-a08a5fc866e4/concepts/455f33f0-2c2d-489d-9ab2-201698fbf21a#)

![](https://d17h27t6h515a5.cloudfront.net/topher/2017/August/598d31bf_waypoint-updater-ros-graph/waypoint-updater-ros-graph.png)

## (path_to_project_repo)/ros/src/twist_controller/

Carla is equipped with a drive-by-wire (dbw) system, meaning the throttle, brake, and steering have electronic control. This package contains the files that are responsible for control of the vehicle: the node  `dbw_node.py`  and the file  `twist_controller.py`, along with a pid and lowpass filter that you can use in your implementation. The  `dbw_node`  subscribes to the  `/current_velocity`  topic along with the  `/twist_cmd`  topic to receive target linear and angular velocities. Additionally, this node will subscribe to  `/vehicle/dbw_enabled`, which indicates if the car is under dbw or driver control. This node will publish throttle, brake, and steering commands to the  `/vehicle/throttle_cmd`,  `/vehicle/brake_cmd`, and  `/vehicle/steering_cmd`  topics.

[](https://classroom.udacity.com/nanodegrees/nd013/parts/6047fe34-d93c-4f50-8336-b70ef10cb4b2/modules/e1a23b06-329a-4684-a717-ad476f0d8dff/lessons/462c933d-9f24-42d3-8bdc-a08a5fc866e4/concepts/455f33f0-2c2d-489d-9ab2-201698fbf21a#)

![](https://d17h27t6h515a5.cloudfront.net/topher/2017/August/598d32e7_dbw-node-ros-graph/dbw-node-ros-graph.png)

In addition to these packages you will find the following, which are not necessary to change for the project. The  `styx`  and  `styx_msgs`  packages are used to provide a link between the simulator and ROS, and to provide custom ROS message types:

-   ### (path_to_project_repo)/ros/src/styx/
    
    A package that contains a server for communicating with the simulator, and a bridge to translate and publish simulator messages to ROS topics.
-   ### (path_to_project_repo)/ros/src/styx_msgs/
    
    A package which includes definitions of the custom ROS message types used in the project.
-   ### (path_to_project_repo)/ros/src/waypoint_loader/
    
    A package which loads the static waypoint data and publishes to  `/base_waypoints`.
-   ### (path_to_project_repo)/ros/src/waypoint_follower/
    
    A package containing code from  [Autoware](https://github.com/CPFL/Autoware)  which subscribes to  `/final_waypoints`  and publishes target vehicle linear and angular velocities in the form of twist commands to the  `/twist_cmd`  topic.

## Result

A short video showing the successful navigation of the car obeying the traffic lights in a simulator is [here](https://www.youtube.com/watch?v=ZRaw4B1urQs&list=PL0-S7MOKfBRQ-JmaZ36G4laB8KGmIXlYL&index=14).

[![System Integration](https://img.youtube.com/vi/ZRaw4B1urQs/0.jpg)](https://www.youtube.com/watch?v=ZRaw4B1urQs&list=PL0-S7MOKfBRQ-JmaZ36G4laB8KGmIXlYL&index=14)

---
# Setup
Please use **one** of the two installation options, either native **or** docker installation.

### Native Installation

* Be sure that your workstation is running Ubuntu 16.04 Xenial Xerus or Ubuntu 14.04 Trusty Tahir. [Ubuntu downloads can be found here](https://www.ubuntu.com/download/desktop).
* If using a Virtual Machine to install Ubuntu, use the following configuration as minimum:
  * 2 CPU
  * 2 GB system memory
  * 25 GB of free hard drive space

  The Udacity provided virtual machine has ROS and Dataspeed DBW already installed, so you can skip the next two steps if you are using this.

* Follow these instructions to install ROS
  * [ROS Kinetic](http://wiki.ros.org/kinetic/Installation/Ubuntu) if you have Ubuntu 16.04.
  * [ROS Indigo](http://wiki.ros.org/indigo/Installation/Ubuntu) if you have Ubuntu 14.04.
* [Dataspeed DBW](https://bitbucket.org/DataspeedInc/dbw_mkz_ros)
  * Use this option to install the SDK on a workstation that already has ROS installed: [One Line SDK Install (binary)](https://bitbucket.org/DataspeedInc/dbw_mkz_ros/src/81e63fcc335d7b64139d7482017d6a97b405e250/ROS_SETUP.md?fileviewer=file-view-default)
* Download the [Udacity Simulator](https://github.com/udacity/CarND-Capstone/releases).

### Docker Installation
[Install Docker](https://docs.docker.com/engine/installation/)

Build the docker container
```bash
docker build . -t capstone
```

Run the docker file
```bash
docker run -p 4567:4567 -v $PWD:/capstone -v /tmp/log:/root/.ros/ --rm -it capstone
```

### Port Forwarding
To set up port forwarding, please refer to the [instructions from term 2](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/16cf4a78-4fc7-49e1-8621-3450ca938b77)

### Usage

1. Clone the project repository
```bash
git clone https://github.com/udacity/CarND-Capstone.git
```

2. Install python dependencies
```bash
cd CarND-Capstone
pip install -r requirements.txt
```
3. Make and run styx
```bash
cd ros
catkin_make
source devel/setup.sh
roslaunch launch/styx.launch
```
4. Run the simulator

### Real world testing
1. Download [training bag](https://s3-us-west-1.amazonaws.com/udacity-selfdrivingcar/traffic_light_bag_file.zip) that was recorded on the Udacity self-driving car.
2. Unzip the file
```bash
unzip traffic_light_bag_file.zip
```
3. Play the bag file
```bash
rosbag play -l traffic_light_bag_file/traffic_light_training.bag
```
4. Launch your project in site mode
```bash
cd CarND-Capstone/ros
roslaunch launch/site.launch
```
5. Confirm that traffic light detection works on real life images
