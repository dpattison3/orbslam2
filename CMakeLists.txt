cmake_minimum_required(VERSION 2.8.3)

project(orb_slam2)
find_package(catkin REQUIRED COMPONENTS
  roscpp
  sensor_msgs
  image_transport
  message_filters
  cv_bridge
  cmake_modules)

set(BUILD_FLAGS "-std=c++11 -Wuninitialized -Wall -Wextra")
set(CMAKE_C_FLAGS "-std=c++11 -Wuninitialized -Wall -Wextra -O3 -march=native ")
set(CMAKE_CXX_FLAGS "-std=c++11 -Wuninitialized -Wall -Wextra -O3 -march=native")

LIST(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake_modules)

find_package(OpenCV 2.4.3 REQUIRED)
find_package(Eigen3 3.1.0 REQUIRED)
find_package(Pangolin REQUIRED)
find_package(Boost REQUIRED)

catkin_package()

include_directories(include
${PROJECT_SOURCE_DIR}
${PROJECT_SOURCE_DIR}/src/
${PROJECT_SOURCE_DIR}/include/
${Pangolin_INCLUDE_DIRS}
${catkin_INCLUDE_DIRS}
)


set(LIBS
${catkin_LIBRARIES}
${OpenCV_LIBS}
${EIGEN3_LIBS}
${Pangolin_LIBRARIES}
${PROJECT_SOURCE_DIR}/Thirdparty/DBoW2/lib/libDBoW2.so
${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o.so
${Boost_LIBRARIES}
#${PROJECT_SOURCE_DIR}/lib/libORB_SLAM2.so
)

# Node for monocular camera
#add_executable(mono
#src/ros_mono.cc
#)

#target_link_libraries(mono
#${LIBS}
#)

# Node for RGB-D camera
#add_executable(rgbd
#src/ros_rgbd.cc
#)

#target_link_libraries(rgbd
#${LIBS}
#)

# Node for stereo camera
add_executable(stereo_slam
src/ros_stereo.cc
src/Converter.cc
src/KeyFrame.cc
src/Map.cc
src/ORBextractor.cc
src/Sim3Solver.cc
src/Frame.cc
src/KeyFrameDatabase.cc
src/MapDrawer.cc
src/ORBmatcher.cc
src/System.cc
src/FrameDrawer.cc
src/LocalMapping.cc
src/MapPoint.cc
src/PnPsolver.cc
src/Tracking.cc
src/Initializer.cc
src/LoopClosing.cc
src/Optimizer.cc
src/Viewer.cc
)

target_link_libraries(stereo_slam
${OpenCV_LIBRARIES}
${OpenCV_LIBS}
${LIBS}
)

