# ros_installation_for_raspberry_pi4

![](https://github.com/smiletoeveryone/ros_installation_for_raspberry_pi4/blob/master/Screenshot_20191020-174718_VNC%20Viewer.jpg)

follow the steps you could install ros in your raspberry pi4/pi3 

swapfile_size_modify

-------------------------------------------------------------------------------------

1. run terminal in the raspberry pi

2. sudo vim /etc/dphys-swapfile

modify the line as "CONF_SWAPSIZE=2048".

// default is "CONF_SWAPSIZE=100(mb)".

3. sudo /etc/init.d/dphys-swapfile stop

// stop the swap service.

4. sudo /etc/init.d/dphys-swapfile start

// restart the service.

5. free -m

// check out the information of memory consumption and swap

---------------------------------------------------------------------------------------

1. sudo sh -c 'echo "deb  http://packages.ros.org/ros/ubuntu  $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'  // save ros packages repository link in the list 

2. sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
3. sudo apt update

4. sudo apt-get install -y python-rosdep python-rosinstall-generator python-wstool python-rosinstall build-essential  cmake
5. sudo rosdep init

6. rosdep update 

7. mkdir ~/ros_catkin_ws

8. cd ~/ros_catkin_ws

9. rosinstall_generator desktop --rosdistro melodic --deps --wet-only --tar > melodic-desktop-wet.rosinstall

10. wstool init -j8 src melodic-desktop-wet.rosinstall

11. mkdir -p ~/ros_catkin_ws/external_src

12. cd ~/ros_catkin_ws/external_src

13. wget http://sourceforge.net/projects/assimp/files/assimp-3.1/assimp-3.1.1_no_test_models.zip/download -O assimp-3.1.1_no_test_models.zip

14. unzip assimp-3.1.1_no_test_models.zip

15. cd assimp-3.1.1

16. cmake .

17. make //take about half an hour

18. sudo apt-get install  libogre-1.9-dev

19. cd ~/ros_catkin_ws

20. find -type f -print0 | xargs -0 grep 'boost::posix_time::milliseconds' | cut -d: -f1 | sort -u
-----------------------------------------------------------------------------
in newer boost versions accepts only an integer argument, but the actionlib package in ROS, gives it a float on several places.

Open them in your text editor and search for the
'boost::posix_time::milliseconds' function call.

and replace calls like this:

boost::posix_time::milliseconds(loop_duration.toSec() * 1000.0f));

with:

boost::posix_time::milliseconds(int(loop_duration.toSec() * 1000.0f)));

and these:

boost::posix_time::milliseconds(1000.0f)

with:
boost::posix_time::milliseconds(1000)
-----------------------------------------------------------------------------
22. rosdep install --from-paths src --ignore-src --rosdistro melodic -y

23. sudo ./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release --install-space /opt/ros/melodic -j2

24. echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc

25. source /opt/ros/melodic/setup.bash

26. roscore  //running the ros framework for checking the installation whether is fine or not

***run turtlesim for an initial test

1. rosrun turtlesim turtlesim_node

2. rosrun turtlesim turtle_teleop_key

3. rosservice call /reset //reset your turtle
