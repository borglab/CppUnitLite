# Build/install CppUnitLite
project(CppUnitLite CXX C)
cmake_minimum_required(VERSION 3.1)

# Get access to various system directories.
include(GNUInstallDirs)

file(GLOB cppunitlite_headers "*.h")
file(GLOB cppunitlite_src "*.cpp")

# Find Boost as the only dependency.
find_package(Boost 1.65)

# Build library.
add_library(CppUnitLite STATIC ${cppunitlite_src} ${cppunitlite_headers})
target_link_libraries(CppUnitLite PUBLIC Boost::boost) # boost/lexical_cast.h

# Install library.
install(FILES ${cppunitlite_headers} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/CppUnitLite)
install(TARGETS CppUnitLite ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR})
