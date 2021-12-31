file(GLOB cppunitlite_headers "*.h")
file(GLOB cppunitlite_src "*.cpp")

# Find Boost as the only dependency.
find_package(Boost 1.65)

# Create library.
add_library(CppUnitLite STATIC ${cppunitlite_src} ${cppunitlite_headers})

# Link Boost as the dependency.
target_link_libraries(CppUnitLite PUBLIC Boost::boost) # boost/lexical_cast.h

# Set the public header files.
set_target_properties(CppUnitLite PROPERTIES PUBLIC_HEADER
                                             "${cppunitlite_headers}")

# Install library.
install(
  TARGETS ${PROJECT_NAME}
  EXPORT "${PROJECT_NAME}Targets"
  RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}" COMPONENT bin
  LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}" COMPONENT lib
  PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/CppUnitLite)
