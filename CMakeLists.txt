cmake_minimum_required(VERSION 2.8.3)
project(orb_slam2)

find_package(catkin REQUIRED roscpp message_filters cv_bridge sensor_msgs image_transport cmake_modules)
find_package(Boost REQUIRED )

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  -Wall  -O3 -march=native ")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall  -O3 -march=native")

# Check C++11 or C++0x support
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
   set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
   add_definitions(-DCOMPILEDWITHC11)
   message(STATUS "Using flag -std=c++11.")
elseif(COMPILER_SUPPORTS_CXX0X)
   set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
   add_definitions(-DCOMPILEDWITHC0X)
   message(STATUS "Using flag -std=c++0x.")
else()
   message(FATAL_ERROR "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()

LIST(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake_modules)

find_package(OpenCV 3.0.0 REQUIRED)
message(STATUS "OpenCV found " ${OpenCV_LIBS})
if(NOT OpenCV_FOUND)
   find_package(OpenCV 2.4.3 REQUIRED)
      message(STATUS "OpenCV > 2.4.3 found.")
   if(NOT OpenCV_FOUND)
      message(FATAL_ERROR "OpenCV > 2.4.3 not found.")
   endif()
endif()

find_package(Eigen3 3.1.0 REQUIRED)
find_package(Pangolin REQUIRED)

catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS roscpp message_filters cv_bridge sensor_msgs image_transport cmake_modules)

include_directories(
${PROJECT_SOURCE_DIR}
${PROJECT_SOURCE_DIR}/include
${Pangolin_INCLUDE_DIRS}
)

set(LIBS 
${catkin_LIBRARIES}
${OpenCV_LIBS} 
${EIGEN3_LIBS}
${Pangolin_LIBRARIES}
${PROJECT_SOURCE_DIR}/lib/libDBoW2.so
${PROJECT_SOURCE_DIR}/lib/libg2o.so
${PROJECT_SOURCE_DIR}/lib/libORB_SLAM2.so
)

# Node for monocular camera

add_executable(mono
src/ros_mono.cc)
target_link_libraries(mono ${catkin_LIBRARIES} ${Boost_LIBRARIES} ${LIBS})


# Node for monocular camera (Augmented Reality Demo)
add_executable(mono_ar
src/AR/ros_mono_ar.cc
src/AR/ViewerAR.h
src/AR/ViewerAR.cc)
target_link_libraries(mono_ar ${catkin_LIBRARIES} ${Boost_LIBRARIES} ${LIBS})

# Node for stereo camera
add_executable(stereo
src/ros_stereo.cc)
target_link_libraries(stereo ${catkin_LIBRARIES} ${Boost_LIBRARIES} ${LIBS})

# Node for RGB-D camera
add_executable(rgbd
src/ros_rgbd.cc)
target_link_libraries(rgbd ${catkin_LIBRARIES} ${Boost_LIBRARIES} ${LIBS})																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																										

# install the include folder
install(
  DIRECTORY config cmake_modules launch lib Thirdparty Vocabulary
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)
