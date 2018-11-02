cmake_minimum_required(VERSION 3.0)
project(function2 VERSION 2.2.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 14)

option(BUILD_TESTS "Build internal tests" ON)

add_library(cxx_function INTERFACE)

target_include_directories(cxx_function
  INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}>
    $<INSTALL_INTERFACE:include/cxx_function>)

if(BUILD_TESTS)
	file(GLOB ALL_TESTS
	            test/*.cpp
	            )
	foreach(simpletest ${ALL_TESTS})
		get_filename_component(test ${simpletest} NAME)
		message(INFO " Test: ${test}")
		add_executable(${test} ${simpletest})
		target_include_directories(${test} PRIVATE ${CMAKE_SOURCE_DIR})
	endforeach()
endif()

# Create an install target
install(TARGETS cxx_function
        EXPORT cxx_function-config)

install(EXPORT cxx_function-config
        FILE cxx_function-config.cmake
        NAMESPACE cxx_function::
        DESTINATION share/cxx_function/cmake)

install(FILES cxx_function.hpp
        DESTINATION include/cxx_function)
