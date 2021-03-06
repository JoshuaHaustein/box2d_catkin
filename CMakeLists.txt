cmake_minimum_required(VERSION 2.8.3)
project(box2d_catkin)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED)

## System dependencies are found with CMake's conventions
# find_package(Boost REQUIRED COMPONENTS system)
# We need to copy the header files to one place, because they are all over the place

file(REMOVE_RECURSE include/Box2D)
file(GLOB_RECURSE HeaderFiles ${CMAKE_CURRENT_LIST_DIR}/Box2D/Box2D/Box2D/*.h)
foreach(FilePath ${HeaderFiles})
        string(REPLACE ${CMAKE_CURRENT_LIST_DIR}/Box2D/Box2D/Box2D/ ${CMAKE_CURRENT_LIST_DIR}/include/Box2D/ IncludeFileDir ${FilePath})
        get_filename_component(FileName ${IncludeFileDir} NAME)
        string(REPLACE ${FileName} "" TargetDir ${IncludeFileDir})
        file(COPY ${FilePath} DESTINATION ${TargetDir})
endforeach()

## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if you package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
  INCLUDE_DIRS include
  LIBRARIES Box2D # no matter whether we build a shared or static library, we always get libBox2D
#  CATKIN_DEPENDS other_catkin_pkg
#  DEPENDS system_lib
)

###########
## Build ##
###########
set(BOX2D_INSTALL_BY_DEFAULT ON)
set(BOX2D_BUILD_SHARED FALSE) # target Box2D_shared
set(BOX2D_BUILD_STATIC TRUE) # target Box2D
set(BOX2D_VERSION 2.3.1)
set(LIB_INSTALL_DIR lib)
## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(include)

# Add the cmake file from box2d.
# This cmake file defines two targets Box2D_shared (shared library) and Box2D (static library).
# The problem is that they are both producing a library libBox2D (Box2D_shared is renamed),
# although they have different target names. This results in an error message when
# building dependent projects and building Box2D_shared only. Hence, until this is resolved
# we have to work with a static library.
add_subdirectory(Box2D/Box2D/Box2D)
## Let the static library be built with position independent code
set_property(TARGET Box2D PROPERTY POSITION_INDEPENDENT_CODE ON)
## Add cmake target dependencies of the library
## as an example, code may need to be generated before libraries
## either from message generation or dynamic reconfigure
# add_dependencies(box2d_catkin ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

## Declare a C++ executable
# add_executable(box2d_catkin_node src/box2d_catkin_node.cpp)

## Add cmake target dependencies of the executable
## same as for the library above
# add_dependencies(box2d_catkin_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

## Specify libraries to link a library or executable target against
# target_link_libraries(box2d_catkin_node
#   ${catkin_LIBRARIES}
# )

#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executable scripts (Python etc.) for installation
## in contrast to setup.py, you can choose the destination
# install(PROGRAMS
#   scripts/my_python_script
#   DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark executables and/or libraries for installation
# install(TARGETS box2d_catkin box2d_catkin_node
#   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark cpp header files for installation
# install(DIRECTORY include/${PROJECT_NAME}/
#   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#   FILES_MATCHING PATTERN "*.h"
#   PATTERN ".svn" EXCLUDE
# )

## Mark other files for installation (e.g. launch and bag files, etc.)
# install(FILES
#   # myfile1
#   # myfile2
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )

#############
## Testing ##
#############

## Add gtest based cpp test target and link libraries
# catkin_add_gtest(${PROJECT_NAME}-test test/test_box2d_catkin.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)
