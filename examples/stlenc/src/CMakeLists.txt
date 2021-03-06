cmake_minimum_required(VERSION 2.8.10)
cmake_policy(SET CMP0015 NEW)


### project definition

set(PROJECT prt_stlenc)
project(${PROJECT})


### external dependencies

find_package(Boost)
include_directories(${Boost_INCLUDE_DIR})
link_directories(${Boost_LIBRARY_DIRS})
add_definitions(-DBOOST_MINOR_VERSION=${Boost_MINOR_VERSION})


### prt dependencies

# if the prt location is not specified on the cmake command line, 
# we assume the example is part of the CityEngine SDK
if(NOT prt_DIR)
	set(prt_DIR "${PROJECT_SOURCE_DIR}/../../../cmake")
endif()
find_package(prt CONFIG)


### build target

add_library(${PROJECT} SHARED main.cpp STLEncoder.cpp)

include_directories(${PRT_INCLUDE_PATH})
target_link_libraries(${PROJECT} ${PRT_LIBRARY})


### install target

set(CMAKE_INSTALL_PREFIX "${PROJECT_SOURCE_DIR}/../install" CACHE PATH "default install prefix" FORCE) 

install(CODE "file(REMOVE_RECURSE ${CMAKE_INSTALL_PREFIX})")
install(TARGETS ${PROJECT} DESTINATION lib)
