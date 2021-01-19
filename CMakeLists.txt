
cmake_minimum_required(VERSION 2.8.12)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -fPIC  -Wall" )
project(fastdds_demo)

# Find requirements
find_package(fastcdr REQUIRED)
find_package(fastrtps REQUIRED)

include(CheckCXXCompilerFlag)
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_COMPILER_IS_CLANG OR
        CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    check_cxx_compiler_flag(-std=c++11 SUPPORTS_CXX11)
    if(NOT SUPPORTS_CXX11)
        message(FATAL_ERROR "Compiler doesn't support C++11")
    endif()
endif()

message(STATUS "Configuring HelloWorld...")
add_library(HelloWorld_lib HelloWorld.cxx)
target_link_libraries(HelloWorld_lib fastcdr fastrtps)

add_executable(HelloWorld HelloWorldPubSubTypes.cxx HelloWorldPublisher.cxx HelloWorldSubscriber.cxx HelloWorldPubSubMain.cxx)
target_link_libraries(HelloWorld fastcdr fastrtps
        HelloWorld_lib )



