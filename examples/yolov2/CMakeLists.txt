
cmake_minimum_required (VERSION 2.8)
project(YOLOV2)

set( INSTALL_DIR ${TENGINE_DIR}/install/ )
set( TENGINE_LIBS tengine)

if( ANDROID AND ("${CMAKE_SYSTEM_PROCESSOR}" STREQUAL "armv7-a"))
    set( PROTOBUF_LIB ${PROTOBUF_DIR}/arm32_lib/libprotobuf.so)
endif()
if( ANDROID AND ("${CMAKE_SYSTEM_PROCESSOR}" STREQUAL "aarch64"))
    set( PROTOBUF_LIB ${PROTOBUF_DIR}/arm64_lib/libprotobuf.so)
endif()

set( CODE_SRCS yolov2.cpp ../common/common.cpp)
set( BIN_EXE YOLOV2)
#opencv
find_package(OpenCV REQUIRED)

#include
include_directories(${INSTALL_DIR}/include
					${TENGINE_DIR}/include
					${TENGINE_DIR}/core/include
					${TENGINE_DIR}/operator/include
                    ${TENGINE_DIR}/examples/common)

#lib
link_directories( ${INSTALL_DIR}/lib )

#exe
add_executable(${BIN_EXE} ${CODE_SRCS})
if( ANDROID)
   target_link_libraries(${BIN_EXE} ${TENGINE_LIBS} ${PROTOBUF_LIB} ${OpenCV_LIBS})
else()
   target_link_libraries(${BIN_EXE} ${TENGINE_LIBS} ${OpenCV_LIBS})
endif()


