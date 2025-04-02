cmake_minimum_required(VERSION 3.8)
project(image_processing)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(example_interfaces REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rosbag2_cpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(rosbag2_transport REQUIRED)
find_package(turtlesim REQUIRED)

add_executable(image_recorder src/image_bag_recorder.cpp)
ament_target_dependencies(image_recorder rclcpp rosbag2_cpp std_msgs)

install(TARGETS
  image_recorder
  DESTINATION lib/${PROJECT_NAME}
)

add_executable(data_generator_executable src/data_generator_executable.cpp)
ament_target_dependencies(data_generator_executable rclcpp rosbag2_cpp example_interfaces)

install(TARGETS
  data_generator_executable
  DESTINATION lib/${PROJECT_NAME}
)

add_executable(image_reader src/image_bag_reader.cpp)
ament_target_dependencies(image_reader rclcpp rosbag2_transport turtlesim)

install(TARGETS
  image_reader
  DESTINATION lib/${PROJECT_NAME}
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
