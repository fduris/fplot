# CMake project file for FPLOT
cmake_minimum_required(VERSION 3.24)
project(
    fplot 
    LANGUAGES Fortran
    VERSION 1.8.2
)

# MT project: force shared library build
set(BUILD_SHARED_LIBS ON CACHE BOOL "Build shared libraries" FORCE)

# Configure everything
add_subdirectory(configure)

# Source
add_subdirectory(src)

# MT project integration - output shared library to MT root directory
get_filename_component(MT_DIR "${PROJECT_SOURCE_DIR}/../.." ABSOLUTE)
set_target_properties(${PROJECT_NAME} PROPERTIES
    OUTPUT_NAME fplot
    LIBRARY_OUTPUT_DIRECTORY ${MT_DIR}
    RUNTIME_OUTPUT_DIRECTORY ${MT_DIR}
    ARCHIVE_OUTPUT_DIRECTORY ${MT_DIR}
)

# Install
install(
  EXPORT ${PROJECT_NAME}-targets
  NAMESPACE ${PROJECT_NAME}::
  DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}"
)

# Testing
option(BUILD_TESTING "Build tests")
include(CTest)
message(STATUS "Build FPLOT tests: ${BUILD_TESTING}")
if (BUILD_TESTING)
    enable_testing()
    add_subdirectory(test)
endif()

# Examples
option(BUILD_FPLOT_EXAMPLES "Build FPLOT examples")
message(STATUS "Build FPLOT examples: ${BUILD_FPLOT_EXAMPLES}")
if (BUILD_FPLOT_EXAMPLES)
    add_subdirectory(examples)
endif()