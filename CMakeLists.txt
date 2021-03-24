cmake_minimum_required(VERSION 3.5)
project(qualisys_ros2)

# Default to C99
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(nav_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(gazebo REQUIRED)

## Declare a C++ library
add_library(gazebo_model_pose
    src/gazebo_model_pose.cpp
)

target_link_libraries(gazebo_model_pose
  ${GAZEBO_LIBRARIES}
)

target_include_directories(gazebo_model_pose PUBLIC
  ${GAZEBO_INCLUDE_DIRS}
)

add_library(qualisys
    src/qualisys/Markup.cpp
    src/qualisys/Network.cpp
    src/qualisys/RTPacket.cpp
    src/qualisys/RTProtocol.cpp
)

target_include_directories(qualisys PUBLIC
  include/qualisys
)


## Declare a C++ executable
## With catkin_make all packages are built within a single CMake context
## The recommended prefix ensures that target names across packages don't collide
add_executable(qualisys_node src/qualisys_node.cpp)

## Rename C++ executable without prefix
## The above recommended prefix causes long target names, the following renames the
## target back to the shorter version for ease of user use
## e.g. "rosrun someones_pkg node" instead of "rosrun someones_pkg someones_pkg_node"
set_target_properties(qualisys_node PROPERTIES OUTPUT_NAME node PREFIX "")


## Specify libraries to link a library or executable target against
target_link_libraries(qualisys_node
  qualisys
  ${catkin_LIBRARIES}
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # uncomment the line when a copyright and license is not present in all source files
  #set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # uncomment the line when this package is not in a git repo
  #set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
