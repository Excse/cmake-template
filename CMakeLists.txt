### --- Project Initialization and Configuration --- ###
cmake_minimum_required(VERSION 3.20)              # Require CMake 3.20 or later
project(your_project VERSION 1.0.0 LANGUAGES CXX) # Define project name, version, and language

# Use C++20 and enforce it
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Option to enable/disable testing:
option(BUILD_TESTING "Enable testing" ON)
# Option to enable/disable executable:
option(BUILD_EXECUTABLE "Create executable" ON)
# Option to enable/disable installation:
option(MAKE_INSTALLABLE "Make the library installable" ON)
# Option to control static vs shared library
option(BUILD_SHARED_LIBS "Build shared libraries" ON)

### --- CMake Modules for Installation and Fetching --- ###
include(CMakePackageConfigHelpers) # Helper for creating config files for package managers
include(GNUInstallDirs)            # Define standard install directories (lib, include, etc.)
include(FetchContent)              # For downloading and using external dependencies


### --- Define Library Target --- ##

# Create the main library from source file
add_library(${PROJECT_NAME} src/library.cpp)
# Create an alias for the library: your_project::your_project
add_library(${PROJECT_NAME}::${PROJECT_NAME} ALIAS ${PROJECT_NAME})

# Setup include directories for consumers and installers
target_include_directories(
        ${PROJECT_NAME}
        PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include> # For build tree
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)      # For install tree

# Define public headers
set(PUBLIC_HEADERS
        include/${PROJECT_NAME}/library.hpp)

# Set the properties of the resulting library
set_target_properties(${PROJECT_NAME}
        PROPERTIES
        VERSION ${PROJECT_VERSION}
        SOVERSION ${PROJECT_VERSION_MAJOR}
        PUBLIC_HEADER "${PUBLIC_HEADERS}")


### --- Define Executable Target --- ###

if(BUILD_EXECUTABLE)
    # Create the main executable from the main source file
    add_executable(${PROJECT_NAME}_app src/main.cpp)
    # Link the library with the application
    target_link_libraries(${PROJECT_NAME}_app PRIVATE ${PROJECT_NAME})
    # Includes the headers to the application
    target_include_directories(${PROJECT_NAME}_app PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/include)
endif()


### --- Install Rules --- ###

if(MAKE_INSTALLABLE)
    # Install the library and its headers
    install(TARGETS ${PROJECT_NAME}
            EXPORT ${PROJECT_NAME}Targets
            LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
            ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
            RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
            PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME})

    # Install the target export file (for find_package)
    install(EXPORT ${PROJECT_NAME}Targets
            FILE ${PROJECT_NAME}Targets.cmake
            NAMESPACE ${PROJECT_NAME}::
            DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME})

    # Generate version file for package configuration
    write_basic_package_version_file(
            "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake"
            VERSION ${PROJECT_VERSION}
            COMPATIBILITY AnyNewerVersion)

    # Creates a config file for package configuration
    configure_package_config_file(
            "${CMAKE_CURRENT_SOURCE_DIR}/cmake/Config.cmake.in"
            "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
            INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME})

    # Install the generated config and config version file
    install(FILES
            "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
            "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake"
            DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME})
endif ()

### --- Unit Testing Setup --- ###

if(BUILD_TESTING)
    # Enable testing framework
    enable_testing()
    # Adds the test directory to the build
    add_subdirectory(tests)
endif()