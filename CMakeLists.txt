project(self_collision_cartesio_addon)
cmake_minimum_required(VERSION 3.0)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake")
include(MacroInstallLib)
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake/Modules/")

find_package(catkin REQUIRED COMPONENTS
    cartesian_interface)

find_package(OpenSoT REQUIRED)

FIND_PACKAGE(FCL 0.6 REQUIRED)
find_package(Eigen3 REQUIRED)

include_directories(${catkin_INCLUDE_DIRS} ${EIGEN3_INCLUDE_DIRS})


# C++ 11
include(CheckCXXCompilerFlag)
check_cxx_compiler_flag("-std=c++11" COMPILER_SUPPORTS_CXX11)
check_cxx_compiler_flag("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
elseif(COMPILER_SUPPORTS_CXX0X)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
else()
    message(FATAL_ERROR "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()

# Generate messages in the 'msg' folder
#add_message_files(
#  FILES
#)

# Generate services in the 'srv' folder
#add_service_files(
#  FILES
#)

# Generate actions in the 'action' folder
#add_action_files(
#  FILES
#)

## Generate added messages and services with any dependencies listed here
#generate_messages(
#  DEPENDENCIES
#)

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if you package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
   INCLUDE_DIRS ${EIGEN3_INCLUDE_DIRS}
   LIBRARIES CartesioConvexHullAddon
#  CATKIN_DEPENDS other_catkin_pkg
   DEPENDS cartesian_interface
)


## Addons for libCartesianOpenSot
include_directories(${FCL_INCLUDE_DIRS})
add_library(CartesioSelfCollisionAvoidanceAddon SHARED src/SelfCollisionAvoidanceAddon.cpp)
target_link_libraries(CartesioSelfCollisionAvoidanceAddon PRIVATE ${OpenSoT_LIBRARIES}
                                                          ${catkin_LIBRARIES}
                                                          ${FCL_LIBRARIES})
install(TARGETS CartesioSelfCollisionAvoidanceAddon CartesioSelfCollisionAvoidanceAddon
	ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})







