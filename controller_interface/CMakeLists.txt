cmake_minimum_required(VERSION 3.5)
project(controller_interface)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra)
endif()

find_package(ament_cmake REQUIRED)
find_package(hardware_interface REQUIRED)
find_package(rclcpp_lifecycle REQUIRED)

add_library(controller_interface SHARED src/controller_interface.cpp)
target_include_directories(
  controller_interface
  PRIVATE
  include)
ament_target_dependencies(
  controller_interface
  "hardware_interface"
  "rclcpp_lifecycle"
)
# Causes the visibility macros to use dllexport rather than dllimport,
# which is appropriate when building the dll but not consuming it.
target_compile_definitions(controller_interface PRIVATE "CONTROLLER_INTERFACE_BUILDING_DLL")

install(DIRECTORY include/
  DESTINATION include
)
install(TARGETS controller_interface
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_export_dependencies(
  hardware_interface
  rclcpp_lifecycle
)
ament_export_include_directories(
  include
)
ament_export_libraries(
  controller_interface
)
ament_package()
