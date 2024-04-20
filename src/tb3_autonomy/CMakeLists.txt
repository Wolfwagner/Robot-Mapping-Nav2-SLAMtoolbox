cmake_minimum_required(VERSION 3.5)
project(tb3_autonomy)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Add compiler flags
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# Find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclcpp_action REQUIRED)
find_package(nav2_msgs REQUIRED)
find_package(behaviortree_cpp_v3 REQUIRED)
find_package(yaml-cpp REQUIRED)
find_package(tf2 REQUIRED)
find_package(tf2_geometry_msgs REQUIRED)

# Add include directories
include_directories(include)

# Install directories
install(DIRECTORY bt_xml launch
        DESTINATION share/${PROJECT_NAME})

# Install C++ behaviors
set(BEHAVIOR_SOURCES
    src/navigation_behaviors.cpp)

set(TARGET_DEPENDS
    rclcpp
    behaviortree_cpp_v3
    yaml-cpp
    rclcpp_action
    nav2_msgs
    tf2
    tf2_geometry_msgs)

add_executable(autonomy_node src/autonomy_node.cpp ${BEHAVIOR_SOURCES})
ament_target_dependencies(autonomy_node ${TARGET_DEPENDS})
target_link_libraries(autonomy_node ${YAML_CPP_LIBRARIES})

install(TARGETS autonomy_node
        DESTINATION lib/${PROJECT_NAME})

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
