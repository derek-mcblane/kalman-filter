add_library(KalmanFilter INTERFACE) 
target_sources(KalmanFilter 
PUBLIC
FILE_SET HEADERS FILES
    kalman_filter.h
)
target_include_directories(KalmanFilter
INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)
target_link_libraries(KalmanFilter
INTERFACE
    Eigen3::Eigen
)
install(TARGETS KalmanFilter EXPORT KalmanFilterTargets
    FILE_SET HEADERS
    INCLUDES DESTINATIION ${CMAKE_INSTALL_INCLUDEDIR}
)
