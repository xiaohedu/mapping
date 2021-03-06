cmake_minimum_required(VERSION 2.8.3)
project(mapping)

## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)
SET(CMAKE_CXX_FLAGS "-std=c++11 -O2 -g -Wall ${CMAKE_CXX_FLAGS}")

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  geometry_msgs
  nav_msgs
  pcl_ros
  roscpp
  rospy
  sensor_msgs
  std_msgs
  tf
)

## System dependencies are found with CMake's conventions
find_package(Boost REQUIRED COMPONENTS system)
find_package(OpenCV 3 REQUIRED)
find_package(PCL REQUIRED)
include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

find_package(OpenMP)
if (OPENMP_FOUND)
	set (CMAKE_C_FLAG "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
	set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
endif()

find_package(Eigen3 REQUIRED COMPONENTS system)
include_directories(${Eigen_INCLUDE_DIRS})


catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES mapping
#  CATKIN_DEPENDS geometry_msgs nav_msgs pcl_ros roscpp rospy sensor_msgs std_msgs tf
#  DEPENDS system_lib
)

include_directories(
# include
  ${catkin_INCLUDE_DIRS}
)

add_executable(gyro_odometry src/gyro_odometry.cpp)
target_link_libraries(gyro_odometry
    ${catkin_LIBRARIES}
)

add_executable(data_save src/data_save.cpp)
target_link_libraries(data_save
    ${catkin_LIBRARIES}
    ${PCL_LIBRARIES}
)

add_executable(rm_cluster src/rm_cluster.cpp)
target_link_libraries(rm_cluster
    ${catkin_LIBRARIES}
    ${PCL_LIBRARIES}
)
