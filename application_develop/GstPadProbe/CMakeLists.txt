# created by Ricardo Lu in 08/29/2021

cmake_minimum_required(VERSION 3.10)

project(GstPadProbe)

set(CMAKE_CXX_STANDARD 11)

set(OpenCV_DIR "/opt/thundersoft/opencv-4.2.0/lib/cmake/opencv4")
find_package(OpenCV REQUIRED)

include(FindPkgConfig)
pkg_check_modules(GST    REQUIRED gstreamer-1.0)
pkg_check_modules(GSTAPP REQUIRED gstreamer-app-1.0)
pkg_check_modules(GLIB   REQUIRED glib-2.0)
pkg_check_modules(GFLAGS REQUIRED gflags)

include_directories(
    ${PROJECT_SOURCE_DIR}/inc
    ${GST_INCLUDE_DIRS}
    ${GSTAPP_INCLUDE_DIRS}
    ${GLIB_INCLUDE_DIRS}
    ${GFLAGS_INCLUDE_DIRS}
    ${OpenCV_INCLUDE_DIRS}
)

link_directories(
    ${GST_LIBRARY_DIRS}
    ${GSTAPP_LIBRARY_DIRS}
    ${GLIB_LIBRARY_DIRS}
    ${GFLAGS_LIBRARY_DIRS}
    ${OpenCV_LIBRARY_DIRS}
)

OPTION(COMPILE_FILE_SOURCE "build filesrc" OFF)
OPTION(COMPILE_RTSP_SOURCE "build rtspsrc" OFF)

if(COMPILE_FILE_SOURCE)
add_definitions(-DFILE_SOURCE)
endif(COMPILE_FILE_SOURCE)

if(COMPILE_RTSP_SOURCE)
add_definitions(-DRTSP_SOURCE)
endif(COMPILE_RTSP_SOURCE)

add_executable(${PROJECT_NAME}
    src/VideoPipeline.cpp
    src/main.cpp
)

target_link_libraries(${PROJECT_NAME}
    ${GST_LIBRARIES}
    ${GSTAPP_LIBRARIES}
    ${GLIB_LIBRARIES}
    ${GFLAGS_LIBRARIES}
    ${OpenCV_LIBRARIES}
    qtimlmeta
)