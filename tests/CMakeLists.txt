include(FetchContent)
FetchContent_Declare(googletest
    GIT_REPOSITORY https://github.com/google/googletest.git
    GIT_TAG 58d77fa8070e8cec2dc1ed015d66b454c8d78850 # release 1.12.1
)
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE) # For Windows: Prevent overriding the parent project's compiler/linker settings
option(INSTALL_GMOCK "" OFF)
option(INSTALL_GTEST "" OFF)
FetchContent_MakeAvailable(googletest)
include(GoogleTest)

add_executable(KalmanFilterTest "")
target_sources(KalmanFilterTest
PRIVATE
    kalman_filter_test.cpp
)
target_link_libraries(KalmanFilterTest PRIVATE
    KalmanFilter
    GTest::gtest_main
)

gtest_discover_tests(KalmanFilterTest)
