cmake_minimum_required(VERSION 3.0.2)
project(nmpc_controller)

## Compile as C++11, supported in ROS Kinetic and newer
add_compile_options(-std=c++11)
  
## Set default cmake build type to release
if(NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  roscpp
  std_msgs
  spirit_msgs
  spirit_utils
  grid_map_core
  grid_map_ros
  eigen_conversions
)

## System dependencies are found with CMake's conventions
find_package(Eigen3 REQUIRED)

catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS roscpp std_msgs spirit_msgs spirit_utils grid_map_core grid_map_ros eigen_conversions
  DEPENDS EIGEN3
  LIBRARIES nmpc_controller
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
#message(STATUS ${OSQP_INCLUDE_DIRS})
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIRS}
  /usr/local/include/coin-or
)
link_directories(/usr/local/lib)

## Declare a C++ library for control
add_library(nmpc_controller
  src/eval_g_leg.cpp
  src/eval_jac_g_leg.cpp
  src/eval_hess_g_leg.cpp
  src/eval_g_tail.cpp
  src/eval_jac_g_tail.cpp
  src/eval_hess_g_tail.cpp
  src/spirit_nlp.cpp
  src/nmpc_controller.cpp
)
target_link_libraries(nmpc_controller 
  Eigen3::Eigen 
  ipopt 
  ${catkin_LIBRARIES})
add_dependencies(nmpc_controller 
  ${nmpc_controller_EXPORTED_TARGETS} 
  ${catkin_EXPORTED_TARGETS})

#############
## Testing ##
#############

## Google test file for nmpc_controller library