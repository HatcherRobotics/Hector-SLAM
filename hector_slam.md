#use lidar(hokuyo UTM-30LX) and PC to SLAM
##1.hardware
I use ubuntu18.04,ROS melodic.<br/>
hokuyo UTM-30LX needs to connect with a 12v
batteand a switch.<br/>
connect lidar and PC through USB2.0
##2.download
`mkdir catkin_ws`<br/>
`cd catkin_ws`<br/>
`mkdir src`<br/>
`cd src`<br/>
`git clone https://github.com/ros-drivers/urg_node.git`
<br/>
`git clone https://github.com/tu-darmstadt-ros-pkg/hector_slam.git`
<br/>
##2.catkin_make
`cd ..`
<br/>
`catkin_make`<br/>
`vim ~/.bashrc`<br/>
`source /home/hatcher/catkin_ws/devel/setup.bash`
// "hatcher"should be replaced by
your own username,check te attribute of the folder
 to find your username.Add this code to the bottom of ~/.bashrc
 <br/>
 ##4.launch
 `cd src/urg_node/launch`
 <br/>
 `roslaunch urg_lidar.launch`
 <br/>
 `cd ..`<br/>
 `cd ..`<br/>
 `mkdir launch`<br/>
 `cd launch`<br/>
 `vim tutorial.launch`
 //add the following code block to the file you vim<br/>

    <launch>

    <include file="$(find hector_mapping)/launch/mapping_default.launch">
     <arg name="base_frame" value="base_frame"/>
     <arg name="odom_frame" value="base_frame"/>
    </include>

    <include file="$(find hector_geotiff)/launch/geotiff_mapper.launch">
      <arg name="trajectory_source_frame_name" value="base_frame"/>
    </include>

    <node pkg="tf" type="static_transform_publisher"       name="base_frame_to_laser_broadcaster" args="0 0 0 0 0 0 base_frame laser 100"/>

    <node pkg="rviz" type="rviz" name="rviz"
      args="-d $(find hector_slam_launch)/rviz_cfg/mapping_demo.rviz"/>

    </launch>
 //save the file and exit<br/>
 `roslaunch tutorial.launch`
 //with all those steps done,you could see a map on rviz,
 walk around in your room with lidar and PC,you could build a map
 of your room.
