cmake_minimum_required(VERSION 3.5)
project(my_robot)

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(cv_bridge REQUIRED)
find_package(image_transport REQUIRED)
find_package(OpenCV REQUIRED)

# Добавьте пути к заголовочным файлам и библиотекам wiringPi
include_directories(/usr/local/include/)
link_directories(/usr/local/lib/)

add_executable(lidar_node src/lidar_node.cpp)
ament_target_dependencies(lidar_node rclcpp sensor_msgs std_msgs)

add_executable(camera_node src/camera_node.cpp)
ament_target_dependencies(camera_node rclcpp sensor_msgs std_msgs cv_bridge image_transport OpenCV)

add_executable(ultrasonic_node src/ultrasonic_node.cpp)
target_include_directories(ultrasonic_node PRIVATE /usr/local/include)
target_link_libraries(ultrasonic_node wiringPi)
ament_target_dependencies(ultrasonic_node rclcpp sensor_msgs std_msgs)

install(TARGETS
  lidar_node
  camera_node
  ultrasonic_node
  DESTINATION lib/${PROJECT_NAME})

# Добавьте эти строки в конце файла CMakeLists.txt
install(DIRECTORY launch/
  DESTINATION share/${PROJECT_NAME}/launch
)

ament_package()
