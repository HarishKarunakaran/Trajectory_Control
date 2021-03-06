cmake_minimum_required(VERSION 2.8.3)
project(fetch_traj)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
  sensor_msgs
  tf
  actionlib
  actionlib_msgs
  control_msgs
  trajectory_msgs
  geometry_msgs
  cmake_modules
  trac_ik_lib
)

## System dependencies are found with CMake's conventions
find_package(Eigen REQUIRED)
find_package(orocos_kdl REQUIRED)
#find_package(Boost REQUIRED COMPONENTS system)


catkin_package(
  INCLUDE_DIRS include
  LIBRARIES fetch_traj
  CATKIN_DEPENDS roscpp rospy std_msgs sensor_msgs actionlib trac_ik_lib
  DEPENDS system_lib orocos_kdl
)

include_directories(include)
include_directories(
  ${catkin_INCLUDE_DIRS}
  ${Eigen_INCLUDE_DIRS}
  ${orocos_kdl_INCLUDE_DIRS}
)

## Declare a C++ library
add_library(${PROJECT_NAME}
   src/fetch_traj.cpp
)

target_link_libraries(${PROJECT_NAME}
   ${catkin_LIBRARIES}
)

add_dependencies(${PROJECT_NAME} ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

add_executable(${PROJECT_NAME}_node src/fetch_traj_node.cpp)


add_dependencies(${PROJECT_NAME}_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

target_link_libraries(${PROJECT_NAME}_node
   ${PROJECT_NAME}
   ${catkin_LIBRARIES}
   ${Eigen_LIBRARIES}
   ${GAZEBO_LIBRARIES}
   ${orocos_kdl_LIBRARIES}
 )

install(TARGETS ${PROJECT_NAME} ${PROJECT_NAME}_node
   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY include/${PROJECT_NAME}/
   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
   FILES_MATCHING PATTERN "*.h"
   PATTERN ".svn" EXCLUDE
)

install(DIRECTORY launch DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
install(DIRECTORY include/${PROJECT_NAME}/ DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION})

SET(CMAKE_BUILD_TYPE RelWithDebInfo)

