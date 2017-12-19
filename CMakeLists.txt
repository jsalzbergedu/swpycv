cmake_minimum_required(VERSION 3.1.3)

set(Python_ADDITIONAL_VERSIONS 2.7 2.6 2.5)
find_package(PythonInterp REQUIRED)
find_package(PythonLibs REQUIRED)
include_directories(${PYTHON_INCLUDE_DIRS})


find_package(OpenCV REQUIRED)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenCV_CFLAGS}" )
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D_GNU_SOURCE")

add_library(swpycv SHARED swpycv.c)
target_link_libraries(swpycv ${OpenCV_LIBS})

add_custom_target(cp_lib ALL
  COMMAND cp -f ${CMAKE_BINARY_DIR}/libswpycv.so ${CMAKE_CURRENT_SOURCE_DIR}/swpycv/
  DEPENDS swpycv)

install(CODE "execute_process( COMMAND bash \"${CMAKE_CURRENT_SOURCE_DIR}\"/buildscript.sh \"${CMAKE_CURRENT_SOURCE_DIR}\" ${PYTHON_EXECUTABLE} install )" )
