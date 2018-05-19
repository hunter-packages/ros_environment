cmake_minimum_required(VERSION 2.8.3)

include(cmake/HunterGate.cmake)
HunterGate(
  URL "https://github.com/ruslo/hunter/archive/v0.21.3.tar.gz"
  SHA1 "67346d2d3958fe293a33f3eda59da295a9af9bf5"
)

project(ros_environment)
hunter_add_package(catkin)
find_package(catkin REQUIRED)

catkin_package()

set(ROS_VERSION "1")
set(ROS_DISTRO "melodic")

# allow overriding the distro name
if(NOT $ENV{ROS_DISTRO_OVERRIDE} STREQUAL "")
  set(ROS_DISTRO "$ENV{ROS_DISTRO_OVERRIDE}")
endif()

set(
  hooks
  "1.ros_distro"
  "1.ros_etc_dir"
  "1.ros_package_path"
  "1.ros_version"
)
if(CMAKE_HOST_UNIX)
  set(shell "sh")
else()
  set(shell "bat")
endif()
foreach(hook ${hooks})
  catkin_add_env_hooks("${hook}" SHELLS ${shell} DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/env-hooks)
endforeach()
