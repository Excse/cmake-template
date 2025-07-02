### --- Fetch and Setup GoogleTest --- ###
FetchContent_Declare(
        googletest
        URL https://github.com/google/googletest/releases/download/v1.17.0/googletest-1.17.0.tar.gz
        DOWNLOAD_EXTRACT_TIMESTAMP TRUE)

# For Windows: Prevent overriding the parent project's compiler/linker settings
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)

FetchContent_MakeAvailable(googletest) # Download and make GoogleTest available
include(GoogleTest)                    # Enable GoogleTest integration (like gtest_discover_tests)


### --- Unit Testing Setup --- ###

# Collect all test source files
file(GLOB TEST_SOURCES CONFIGURE_DEPENDS ./*.cpp)
# or use this for only specific sources:
# set(TEST_SOURCES ...)

# Define test executable
add_executable(${PROJECT_NAME}_tests ${TEST_SOURCES})
# Link the test binary with the main library and GoogleTest
target_link_libraries(${PROJECT_NAME}_tests PRIVATE ${PROJECT_NAME} gtest_main)
# Automatically discover and register tests with CTest
gtest_discover_tests(${PROJECT_NAME}_tests)