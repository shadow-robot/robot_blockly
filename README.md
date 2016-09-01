# robot_blockly

------

This package has been renamed to meet the ROS naming conventions http://wiki.ros.org/ROS/Patterns/Conventions

------

`robot_blockly` is a ROS package that provides web-based visualization and block programming tools for robots and drones.

*Note: The package has been renamed from ros_rosimple to robot_blockly to meet http://wiki.ros.org/ROS/Patterns/Conventions*
![](img/ROSimple-peek.png)
![](img/ROSimple-code.png)

### Installation:

Pre-requisites:

```
sudo apt-get install python-pip
sudo pip install rosdep rosinstall_generator wstool rosinstall
sudo pip install autobahn trollius
```

Then create a new workspace:
```
mkdir -p blockly/src
cd blockly/src
wstool init

If you have added an ssh key for your machine to your github account, use:
wstool set robot_blockly --git git@github.com:shadow-robot/robot_blockly

Otherwise run:
wstool set robot_blockly --git https://github.com/shadow-robot/robot_blockly.git

wstool up
```

To build the workspace:
```
cd ../
catkin_make
```

To start the backend and built-in HTTP server:
```
source devel/setup.bash
roslaunch robot_blockly robot_blockly.launch 
```

In order to include a blocks library for additional blocks (e.g. [sr_blockly](https://github.com/shadow-robot/sr_blockly)), clone the repository into your workspace and run:
```
source devel/setup.bash
roslaunch robot_blockly robot_blockly.launch block_packages:=[sr_blockly_blocks]
```

Point your browser to: [http://localhost:8000/pages/blockly.html](http://localhost:8000/pages/blockly.html)

### Create your own blocks
- Default blockly blocks are stored in the blockly repository which is cloned as a Git submodule.
- Shadow's blocks package can be found [here](https://github.com/shadow-robot/sr_blockly)
- If you want to create your own blocks, you can either add them to an existing ROS package or create a new package.
- Blocks can be generated by opening [http://localhost:8000/blockly/demos/blockfactory/index.html](http://localhost:8000/blockly/demos/blockfactory/index.html)
- Design you own block and then add the metadata to: `blocks` and `generators` in the block repository. An xml snippet should be included in any Javascript file of the block repository, please see [sr_blockly](https://github.com/shadow-robot/sr_blockly/tree/master/sr_blockly_blocks/toolbox) for an example of how to do this.
- catkin_make will automatically merge all Javascript files into a single file. So in case if you have changed Javascript file you need to run catkin_make.

#### Dynamic data loading from server
Sometimes it is convinient to load data from the server for a component.
In order to implement server side processing web service modules were introduced.
You need to create the file **web_service_module.py** in your library package and put in there the class **WebServiceModule**.
This class can have functions which accept a parameter object and return a result object.

API **Blockly.callWebService** provides the possibility to call such methods by name and pass a parameter from Javascript.

Also **Blockly.waitForComponentToLoad** allows you to call such web modules and wait for it's execution before loading workspace.
It can be used to configure blocks depending on the server data.
An example can be found [here](https://github.com/shadow-robot/sr_blockly/blob/master/sr_blockly_blocks/blocks/trajectory_named_waypoint.js).

### License
blockly has been built based on [blockly](http://github.com/google/blockly), [ACE](http://github.com/erlerobot/ace-builds) and Bootstrap. Refer to their sources for the corresponding licenses.

Unless specified, the rest of the code is freed under a GPLv3 License.

### Documentation
- [Erle Robotics blockly docs](http://erlerobotics.com/docs/Robot_Operating_System/ROS/Blockly/Intro.html)
- [ROS Wiki](http://wiki.ros.org/blockly)


# Robots where blockly has been implemented:
- [Erle-Spider](http://erlerobotics.com/blog/product/erle-spider-the-ubuntu-drone-with-legs/)
- [Erle-Rover](https://erlerobotics.com/blog/product/erle-rover/) (Work in progress)

Do you need help getting your robot supported? Launch your questions at http://forum.erlerobotics.com.
