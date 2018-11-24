cmake_minimum_required(VERSION 2.8.8)

# Project name
project(codecpp)

# Include our cmake files
SET(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmakeModules/)


# Include libraries
find_package(Eigen3 REQUIRED)
find_package(Boost REQUIRED COMPONENTS system serialization system filesystem thread program_options date_time regex timer chrono)



# Include our header files
include_directories(
	arrays
        ${EIGEN3_INCLUDE_DIR}
        ${Boost_INCLUDE_DIRS}
)


# Try to compile with c++11
# http://stackoverflow.com/a/25836953
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
elseif(COMPILER_SUPPORTS_CXX0X)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
else()
    message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()

# Enable compile optimizations
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3")

# Enable debug flags
#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g3  -Wall")


# SSE3 is needed by libviso2 (its a cpu arch attribute)
# If we are on ARM we need to find an alternative
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -msse -msse2 -msse3")
# Include all thirdparty source files in build



#####################################
# Main class files
#####################################

# Create the main executable, and link libraries
add_executable(main_array
        arrays/main_array.cpp
)

target_link_libraries(main_array
        ${Boost_LIBRARIES}
        )

