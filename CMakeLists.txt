cmake_minimum_required(VERSION 2.8.3)
project(gpu_problem)

## Add support for C++11, supported in ROS Kinetic and newer
add_definitions(-std=c++11)


find_package(OpenCV REQUIRED)
#NO_MODULE # should be optional, tells CMake to use config mode
#PATHS /usr/local # look here
#NO_DEFAULT_PATH) # and don't look anywhere else

find_package( CUDA REQUIRED )
find_package(catkin REQUIRED COMPONENTS # In my package I did not use these
#  roscpp
#  rospy
#  std_msgs
#  sensor_msgs
  image_transport
  cv_bridge
  message_filters
)


catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES obj_detect
#  CATKIN_DEPENDS roscpp rospy std_msgs
#  DEPENDS system_lib
)

###########
## Build ##
###########

include_directories(SYSTEM ${OpenCV_INCLUDE_DIRS})
include_directories(
include ${catkin_INCLUDE_DIRS}
)

## check libraries 
MESSAGE(WARNING "My Catkin lib Include ${catkin_LIBRARIES}")
MESSAGE(WARNING "install path ${OpenCV_INSTALL_PATH}")
MESSAGE(WARNING "include dirs ${OpenCV_INCLUDE_DIRS} ")
MESSAGE(WARNING "include dirs ${catkin_INCLUDE_DIRS}")

## same source but different dependency
add_executable(template_trackgpu1 src/TempTrackGPU.cpp)
add_executable(template_trackgpu2 src/TempTrackGPU.cpp)
add_executable(template_trackgpu3 src/TempTrackGPU.cpp)
add_executable(template_trackgpu4 src/TempTrackGPU.cpp)
add_executable(template_trackgpu5 src/TempTrackGPU.cpp)

## adding catkin_libraries cause error
target_link_libraries(template_trackgpu1 ${OpenCV_LIBS} ${catkin_LIBRARIES}) # This does not work 
target_link_libraries(template_trackgpu2 ${OpenCV_LIBS}) # This does work


## Adding cv_bridge, image_transport is ok!!
# change Catkin Libraries : do not contain ros-opencv3 information ( contain cv_bridge ) 
set(catkin_LIBRARIES )
target_link_libraries(template_trackgpu3 ${OpenCV_LIBS} ${catkin_LIBRARIES}) # Now it works?



## Adding ros-kinetic-opencv cause error?
# change Catkin Libraries : do contain ros-opencv3 information (with out /opt/ros/kinetic/lib/libopencv_core3.so.3.2.0;)
set(catkin_LIBRARIES )

target_link_libraries(template_trackgpu4 ${OpenCV_LIBS} ${catkin_LIBRARIES}) # Do not work!!!

## Just delete core3.so works!!

#string(REPLACE "/opt/ros/kinetic/lib/libopencv_core3.so.3.2.0;" "" catkin_LIBRARIES "${catkin_LIBRARIES}")
target_link_libraries(template_trackgpu5 ${OpenCV_LIBS} ${catkin_LIBRARIES}) # it works !!!

