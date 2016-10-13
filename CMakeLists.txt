cmake_minimum_required(VERSION 2.8.3)
project(slam_karto)

find_package(catkin REQUIRED
  COMPONENTS
    cmake_modules
    dynamic_reconfigure
    message_filters
    nav_msgs
    open_karto
    rosconsole
    roscpp
    sensor_msgs
    sparse_bundle_adjustment
    tf
    visualization_msgs
)
find_package(Eigen REQUIRED)

generate_dynamic_reconfigure_options(
  cfg/SlamKarto.cfg
)

include_directories(${catkin_INCLUDE_DIRS} ${EIGEN_INCLUDE_DIRS})
add_definitions(${EIGEN_DEFINITIONS})

catkin_package()

add_executable(slam_karto src/slam_karto.cpp src/spa_solver.cpp)
target_link_libraries(slam_karto ${catkin_LIBRARIES})

# make sure configure headers are built before using them
add_dependencies(slam_karto ${PROJECT_NAME}_gencfg)

install(TARGETS slam_karto
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
