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
set(catkin_LIBRARIES /opt/ros/kinetic/lib/libimage_transport.so;/opt/ros/kinetic/lib/libclass_loader.so;/usr/lib/libPocoFoundation.so;/usr/lib/aarch64-linux-gnu/libdl.so;/opt/ros/kinetic/lib/libroslib.so;/opt/ros/kinetic/lib/librospack.so;/usr/lib/aarch64-linux-gnu/libpython2.7.so;/usr/lib/aarch64-linux-gnu/libboost_program_options.so;/usr/lib/aarch64-linux-gnu/libtinyxml.so;/opt/ros/kinetic/lib/libcv_bridge.so;/opt/ros/kinetic/lib/libmessage_filters.so;/opt/ros/kinetic/lib/libroscpp.so;/usr/lib/aarch64-linux-gnu/libboost_signals.so;/usr/lib/aarch64-linux-gnu/libboost_filesystem.so;/opt/ros/kinetic/lib/libroscpp_serialization.so;/opt/ros/kinetic/lib/libxmlrpcpp.so;/opt/ros/kinetic/lib/librosconsole.so;/opt/ros/kinetic/lib/librosconsole_log4cxx.so;/opt/ros/kinetic/lib/librosconsole_backend_interface.so;/usr/lib/aarch64-linux-gnu/liblog4cxx.so;/usr/lib/aarch64-linux-gnu/libboost_regex.so;/opt/ros/kinetic/lib/librostime.so;/opt/ros/kinetic/lib/libcpp_common.so;/usr/lib/aarch64-linux-gnu/libboost_system.so;/usr/lib/aarch64-linux-gnu/libboost_thread.so;/usr/lib/aarch64-linux-gnu/libboost_chrono.so;/usr/lib/aarch64-linux-gnu/libboost_date_time.so;/usr/lib/aarch64-linux-gnu/libboost_atomic.so;/usr/lib/aarch64-linux-gnu/libpthread.so;/usr/lib/aarch64-linux-gnu/libconsole_bridge.so)
target_link_libraries(template_trackgpu3 ${OpenCV_LIBS} ${catkin_LIBRARIES}) # Now it works?



## Adding ros-kinetic-opencv cause error?
# change Catkin Libraries : do contain ros-opencv3 information (with out /opt/ros/kinetic/lib/libopencv_core3.so.3.2.0;)
set(catkin_LIBRARIES /opt/ros/kinetic/lib/libopencv_core3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_features2d3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_flann3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_highgui3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_imgcodecs3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_imgproc3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_ml3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_objdetect3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_photo3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_shape3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_stitching3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_superres3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_video3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_videoio3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_videostab3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_viz3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_aruco3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_bgsegm3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_bioinspired3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_ccalib3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_cvv3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_datasets3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_dpm3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_face3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_fuzzy3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_hdf3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_line_descriptor3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_optflow3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_phase_unwrapping3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_plot3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_reg3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_rgbd3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_saliency3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_stereo3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_structured_light3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_surface_matching3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_text3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_xfeatures2d3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_ximgproc3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_xobjdetect3.so.3.2.0;/opt/ros/kinetic/lib/libopencv_xphoto3.so.3.2.0;)

target_link_libraries(template_trackgpu4 ${OpenCV_LIBS} ${catkin_LIBRARIES}) # Do not work!!!

## Just delete core3.so works!!

string(REPLACE "/opt/ros/kinetic/lib/libopencv_core3.so.3.2.0;" "" catkin_LIBRARIES "${catkin_LIBRARIES}")
target_link_libraries(template_trackgpu5 ${OpenCV_LIBS} ${catkin_LIBRARIES}) # it works !!!

