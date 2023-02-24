cmake_minimum_required(VERSION 2.8.3)
project(imu_gps_localization)

add_compile_options(-std=c++14)

## Find catkin macros and libraries
find_package(catkin REQUIRED COMPONENTS
  roscpp
  nav_msgs
)

find_package(Python3 COMPONENTS Development NumPy)
include_directories(${Python3_INCLUDE_DIRS} ${Python3_NumPy_INCLUDE_DIRS})


catkin_package()

add_subdirectory(imu_gps_localizer)

include_directories(
  ${catkin_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIR}
  imu_gps_localizer/include
  ros_wrapper/include
)

add_library(ros_wrapper_lib
	ros_wrapper/src/localization_wrapper.cpp
)
target_link_libraries(ros_wrapper_lib
  ${catkin_LIBRARIES}
  imu_gps_localizer_lib
  glog
)

## Localization node.
add_executable(${PROJECT_NAME}_node ros_wrapper/src/localization_node.cpp)
target_link_libraries(${PROJECT_NAME}_node
  ros_wrapper_lib
)
target_link_libraries(${PROJECT_NAME}_node Python3::Python Python3::NumPy)




