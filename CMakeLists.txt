cmake_minimum_required(VERSION 2.8)
project(ldso)

# Set default build type if not specified otherwise.
# See https://cmake.org/pipermail/cmake/2012-May/050243.html
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Choose the type of build." FORCE)
  message(STATUS "Setting build type to '${CMAKE_BUILD_TYPE}' as none was specified.")
  # Set the possible values of build type for cmake-gui
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release"
    "MinSizeRel" "RelWithDebInfo")
endif()

# C++ standard
set( CMAKE_CXX_FLAGS "-std=c++11 -O3 -g -w ${CMAKE_CXX_FLAGS}" )
set(CMAKE_BUILD_TYPE "Release")

# costum modules
LIST(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)

# TODO: change this to not output outside build dir
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

# required libs
find_package(Eigen3 REQUIRED)
#find_package(OpenCV 4.5.3 EXACT REQUIRED)
#find_package(OpenCV 3.4.14 EXACT REQUIRED PATHS /opt/homebrew/opt/opencv@3)
find_package(OpenCV REQUIRED)
find_package(Glog REQUIRED)
find_package(Pangolin REQUIRED)

find_package(LibQPEP REQUIRED)
message("The include directory of LibQPEP is: ${LibQPEP_INCLUDE_DIRS}")
message("The linking directory of LibQPEP is: ${LibQPEP_LIBRARIES}")
include_directories(${LibQPEP_INCLUDE_DIRS})

# optional libs
find_package(LibZip QUIET)

set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pthread")

# decide if we have ziplib.
if (LIBZIP_LIBRARY)
  message("--- found ziplib (${LIBZIP_VERSION}), compiling with zip capability.")
  add_definitions(-DHAS_ZIPLIB=1)
  include_directories( ${LIBZIP_INCLUDE_DIR_ZIP} ${LIBZIP_INCLUDE_DIR_ZIPCONF} )
else()
  message("--- not found ziplib (${LIBZIP_LIBRARY}), compiling without zip capability.")
  set(LIBZIP_LIBRARY "")
endif()

include_directories(
        ${EIGEN3_INCLUDE_DIR}
        ${OpenCV_INCLUDE_DIR}
        ${Glog_INCLUDE_DIRS}
        ${Pangolin_INCLUDE_DIRS}

        ${PROJECT_SOURCE_DIR}/include
        ${PROJECT_SOURCE_DIR}/thirdparty/
        ${PROJECT_SOURCE_DIR}/thirdparty/g2o/
        ${PROJECT_SOURCE_DIR}/thirdparty/DBoW3/
	${PROJECT_SOURCE_DIR}/thirdparty/sse2neon/
)

set( THIRD_PARTY_LIBS
        ${OpenCV_LIBS}
        ${GLOG_LIBRARY}
        ${Pangolin_LIBRARIES}
        ${PROJECT_SOURCE_DIR}/thirdparty/g2o/lib/libg2o${CMAKE_SHARED_LIBRARY_SUFFIX}
        ${PROJECT_SOURCE_DIR}/thirdparty/DBoW3/build/src/libDBoW3${CMAKE_SHARED_LIBRARY_SUFFIX}
        ${LIBZIP_LIBRARY}
        ${LibQPEP_LIBRARIES}
)

add_subdirectory(src)
add_subdirectory(examples)
