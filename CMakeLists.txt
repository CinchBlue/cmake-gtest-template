# PROJECT HEADER
cmake_minimum_required(VERSION 3.7)
project(test_gmock)

# C++ standard
set(CMAKE_CXX_STANDARD 11)

################################################################################
# Get Google Test/Mock from GitHub
################################################################################
# Get the Google Test/Google Mock repository from GitHub.
include(ExternalProject)
ExternalProject_Add(gtest GIT_REPOSITORY https://github.com/google/googletest
        PREFIX ${CMAKE_CURRENT_BINARY_DIR}/dep/gtest
        INSTALL_COMMAND "" )
ExternalProject_Get_Property(gtest source_dir binary_dir)

# Create a libgtest target to be used as a dependency by test programs.
add_library(libgtest IMPORTED STATIC GLOBAL)
add_dependencies(libgtest test)
set_target_properties(libgtest PROPERTIES
        "IMPORTED_LOCATION" "${binary_dir}/googlemock/gtest/libgtest.a"
        "IMPORTED_LINK_INTERFACE_LIBRARIES" "${CMAKE_THREAD_LIBS_INIT}")

# Create a libgmock target to be used as a dependency by test programs.
add_library(libgmock IMPORTED STATIC GLOBAL)
add_dependencies(libgmock gtest)
set_target_properties(libgmock PROPERTIES
        "IMPORTED_LOCATION" "${binary_dir}/googlemock/libgmock.a"
        "IMPORTED_LINK_INTERFACE_LIBRARIES" "${CMAKE_THREAD_LIBS_INIT}" )

# Crate a libgmock_main
add_library(libgmock_main IMPORTED STATIC GLOBAL)
add_dependencies(libgmock_main libgmock)
set_target_properties(libgmock_main PROPERTIES
        "IMPORTED_LOCATION" "${binary_dir}/googlemock/libgmock_main.a"
        "IMPORTED_LINK_INTERFACE_LIBRARIES" "${CMAKE_THREAD_LIBS_INIT}" )
include_directories(
        "${source_dir}/googletest/include"
        "${source_dir}/googlemock/include")

################################################################################
# Add subdirectories
################################################################################
# Sourcefiles for primary executable
set(SOURCE_FILES main.cpp)
add_executable(test_gmock ${SOURCE_FILES})

# Subdirectories
add_subdirectory(src)
add_subdirectory(tst)
