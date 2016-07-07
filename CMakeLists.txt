cmake_minimum_required(VERSION 2.8.3)
project(imu_vn_100)

add_definitions("-std=c++11")

find_package(catkin REQUIRED COMPONENTS
  roscpp
  sensor_msgs
  diagnostic_updater
  tf
)
find_package(Boost REQUIRED)

catkin_package(
  INCLUDE_DIRS include vncpplib/include ext/vnproglib-1.1.0.115/include
  LIBRARIES imu_vn_100 vnproglib
  CATKIN_DEPENDS diagnostic_updater roscpp sensor_msgs 
  DEPENDS boost
)


# build the vnproflib first
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/ext/vnproglib-1.1.0.115)

include_directories(
  include
#  vncpplib/include
  ext/vnproglib-1.1.0.115/include
  ${catkin_INCLUDE_DIRS}
)

#add_library(imu_vn_100
#  vncpplib/src/arch/linux/vncp_services.c
#  vncpplib/src/vndevice.c
#  vncpplib/src/vn100.c
#  src/imu_vn_100.cpp
#)

add_library(imu_vn_100
    src/imu_vn_100.cpp
)

target_link_libraries(imu_vn_100
  vnproglib
  ${catkin_LIBRARIES}
)

add_dependencies(imu_vn_100
  ${${PROJECT_NAME}_EXPORTED_TARGETS}
  ${catkin_EXPORTED_TARGETS}
)

add_executable(imu_vn_100_cont_node
  src/imu_vn_100_cont.cpp)

target_link_libraries(imu_vn_100_cont_node
  ${catkin_LIBRARIES}
  vnproglib
  imu_vn_100
)
add_dependencies(imu_vn_100_cont_node
  ${${PROJECT_NAME}_EXPORTED_TARGETS}
  ${catkin_EXPORTED_TARGETS}
)
 ## Mark executables and/or libraries for installation
install(TARGETS imu_vn_100 imu_vn_100_cont_node
# ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

## Mark cpp header files for installation
install(DIRECTORY include 
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h" PATTERN "*.hpp"
  PATTERN ".svn" EXCLUDE
)

## Mark other files for installation (e.g. launch and bag files, etc.)
# install(FILES 
#   # myfile1
#   # myfile2
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
#)