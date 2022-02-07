cmake_minimum_required(VERSION 3.0.2)
project(leo_fw)

find_package(catkin REQUIRED COMPONENTS
  dynamic_reconfigure
  leo_msgs
  nav_msgs
  roscpp
  sensor_msgs
)

catkin_python_setup()

generate_dynamic_reconfigure_options(
  cfg/FirmwareMessageConverter.cfg
)

catkin_package()

include_directories(
  ${catkin_INCLUDE_DIRS}
)


add_executable(firmware_message_converter
  src/firmware_message_converter.cpp
)

add_dependencies(firmware_message_converter
  ${PROJECT_NAME}_gencfg
)

target_link_libraries(firmware_message_converter
  ${catkin_LIBRARIES}
)

install(
  DIRECTORY firmware
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

catkin_install_python(
  PROGRAMS 
    scripts/update
    scripts/flash
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(TARGETS firmware_message_converter
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)