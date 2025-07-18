### --- Fetch and Setup GoogleTest --- ###
FetchContent_Declare(
        googletest
        GIT_REPOSITORY https://github.com/google/googletest.git
        GIT_TAG main
        DOWNLOAD_EXTRACT_TIMESTAMP TRUE)

# Disable GoogleTest installation
set(INSTALL_GTEST OFF CACHE BOOL "Disable GoogleTest install")
# For Windows: Prevent overriding the parent project's compiler/linker settings
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)

# Download and make GoogleTest available
FetchContent_MakeAvailable(googletest)
# Enable GoogleTest integration (like gtest_discover_tests)
include(GoogleTest)


### --- Unit Testing Setup --- ###

# Collect all test source files
file(GLOB_RECURSE TEST_SOURCES CONFIGURE_DEPENDS ./*.cpp)
# or use this for only specific sources:
# set(TEST_SOURCES ...)

# Define test executable
add_executable(${PROJECT_NAME}_tests ${TEST_SOURCES})
# Link the test binary with the main library and GoogleTest
target_link_libraries(${PROJECT_NAME}_tests PRIVATE ${PROJECT_NAME} gtest_main)
# Automatically discover and register tests with CTest
gtest_discover_tests(${PROJECT_NAME}_tests)