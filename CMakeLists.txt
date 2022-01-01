# Build/install CppUnitLite
project(CppUnitLite CXX C)
cmake_minimum_required(VERSION 3.1)

set(${PROJECT_NAME}_MAJOR_VERSION 1)
set(${PROJECT_NAME}_MINOR_VERSION 0)
set(${PROJECT_NAME}_PATCH_VERSION 0)
set(${PROJECT_NAME}_VERSION
    ${CppUnitLite_MAJOR_VERSION}.${CppUnitLite_MINOR_VERSION}.${CppUnitLite_PATCH_VERSION}
)

# Get access to various system directories.
include(GNUInstallDirs)

add_subdirectory(src)

# Add all targets to the build-tree export set
export(TARGETS ${PROJECT_NAME}
       FILE "${PROJECT_BINARY_DIR}/${PROJECT_NAME}Targets.cmake")
install(
  EXPORT "${PROJECT_NAME}Targets"
  FILE "${PROJECT_NAME}Targets.cmake"
  DESTINATION lib/cmake/${PROJECT_NAME})

# Export the package for use from the build-tree (this registers the build-tree
# with a global CMake-registry)
export(PACKAGE ${PROJECT_NAME})

# ##############################################################################
# Package Configuration

# Helper functions for generating the CppUnitLiteConfig.cmake file correctly.
include(CMakePackageConfigHelpers)

# Set the variables to be used in the cmake Config file.
if(WIN32 AND NOT CYGWIN)
  set(INSTALL_CMAKE_DIR CMake/${PROJECT_NAME})
else()
  set(INSTALL_CMAKE_DIR lib/cmake/${PROJECT_NAME})
endif()
set(INSTALL_LIB_DIR lib)
set(INSTALL_BIN_DIR bin/${PROJECT_NAME})
set(INSTALL_INCLUDE_DIR include/${PROJECT_NAME})

# Configure the config file which is used for `find_package`.
configure_package_config_file(
  ${CMAKE_CURRENT_SOURCE_DIR}/${PROJECT_NAME}Config.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
  INSTALL_DESTINATION "${INSTALL_CMAKE_DIR}"
  PATH_VARS INSTALL_CMAKE_DIR INSTALL_LIB_DIR INSTALL_BIN_DIR
            INSTALL_INCLUDE_DIR
  INSTALL_PREFIX ${CMAKE_INSTALL_PREFIX})

# Create version file
write_basic_package_version_file(
  ${CMAKE_CURRENT_BINARY_DIR}/CppUnitLiteConfigVersion.cmake
  VERSION ${CppUnitLite_VERSION}
  COMPATIBILITY SameMajorVersion)

# Set all the install paths
set(${PROJECT_NAME}_CMAKE_INSTALL_DIR
    ${CMAKE_INSTALL_PREFIX}/${INSTALL_CMAKE_DIR})
set(${PROJECT_NAME}_LIB_INSTALL_DIR
    ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR})
set(${PROJECT_NAME}_BIN_INSTALL_DIR
    ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_BINDIR})
set(${PROJECT_NAME}_INCLUDE_INSTALL_DIR
    ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_INCLUDEDIR})

# Install CMake scripts to the standard CMake script directory.
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
        DESTINATION "${${PROJECT_NAME}_CMAKE_INSTALL_DIR}")
