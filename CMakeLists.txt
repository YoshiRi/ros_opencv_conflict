cmake_minimum_required(VERSION 2.8.3)
project(gpu_problem)

## Add support for C++11, supported in ROS Kinetic and newer
add_definitions(-std=c++11)


find_package(OpenCV REQUIRED
NO_MODULE # should be optional, tells CMake to use config mode
PATHS /usr/local # look here
NO_DEFAULT_PATH) # and don't look anywhere else

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

## debugging
MESSAGE(WARNING "My Catkin lib Include ${catkin_LIBRARIES}")
MESSAGE(WARNING "install path ${OpenCV_INSTALL_PATH}")
MESSAGE(WARNING "include dirs ${OpenCV_INCLUDE_DIRS} ")
MESSAGE(WARNING "include dirs ${catkin_INCLUDE_DIRS}")


add_executable(template_trackgpu1 src/TempTrackGPU.cpp)
add_executable(template_trackgpu2 src/TempTrackGPU.cpp)
add_executable(template_trackgpu3 src/TempTrackGPU.cpp)


target_link_libraries(template_trackgpu1 ${OpenCV_LIBS} ${catkin_LIBRARIES}) # This does not work 
target_link_libraries(template_trackgpu2 ${OpenCV_LIBS}) # This does work

set(catkin_LIBRAIRIES opencv_cudev; opencv_cudawarping; opencv_cudastereo; opencv_cudaoptflow; opencv_cudaobjdetect; opencv_cudalegacy; opencv_cudaimgproc; opencv_cudafilters; opencv_cudafeatures2d; opencv_cudacodec; opencv_cudabgsegm; opencv_cudaarithm;)
MESSAGE(WARNING "Updated catkin lib Include ${catkin_LIBRARIES}")

target_link_libraries(template_trackgpu3 ${OpenCV_LIBS} ${catkin_LIBRARIES}) # Now it works?


