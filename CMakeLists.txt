cmake_minimum_required(VERSION 3.25.0)

project(KalmanFilter VERSION 0.1.0)
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_LIST_DIR}/cmake)

include(CMakePackageConfigHelpers)
include(GNUInstallDirs)

set(gcc_like_cxx "$<COMPILE_LANG_AND_ID:CXX,ARMClang,AppleClang,Clang,GNU>")
set(msvc_cxx "$<COMPILE_LANG_AND_ID:CXX,MSVC>")

set(PROJECT_WARNING_OPTIONS
    "$<${gcc_like_cxx}:$<BUILD_INTERFACE:-Wall;-Wextra;-Wshadow;-Wunused>>"
    "$<${msvc_cxx}:$<BUILD_INTERFACE:-W3>>"
)

add_compile_definitions(
    $<$<CONFIG:DEBUG>:KalmanFilter_DEBUG>
)

set(GIT_COMMIT_ID_FILE ${CMAKE_CURRENT_BINARY_DIR}/git_commit_id.txt)
add_custom_target(GitCommitIDFile
    COMMAND ${CMAKE_COMMAND}
        -DGIT_COMMIT_ID_FILE=${GIT_COMMIT_ID_FILE}
        -P ${PROJECT_SOURCE_DIR}/scripts/MakeGitCommitIDFile.cmake
    BYPRODUCTS ${GIT_COMMIT_ID_FILE}
    COMMENT "Updating ${GIT_COMMIT_ID_FILE}"
    VERBATIM
)

find_package(Eigen3 3.3 REQUIRED CONFIG)

add_subdirectory(doc EXCLUDE_FROM_ALL)
add_subdirectory(source)

option(KalmanFilter_ENABLE_TESTING "Enable testing for KalmanFilter" ON)
if (${KalmanFilter_ENABLE_TESTING})
    enable_testing()
    add_subdirectory(tests)
endif()

export(EXPORT KalmanFilterTargets
    NAMESPACE KalmanFilter::
)
install(EXPORT KalmanFilterTargets
    NAMESPACE KalmanFilter::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/KalmanFilter
)

write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/KalmanFilterConfigVersion.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion 
)
configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/KalmanFilterConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/KalmanFilterConfig.cmake
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/KalmanFilter
)
install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/KalmanFilterConfig.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/KalmanFilterConfigVersion.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/KalmanFilter
)

export(PACKAGE KalmanFilter)

include(CPack)
