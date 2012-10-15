cmake_minimum_required(VERSION 2.8)
project(Cinder-KissFft)

SET(CMAKE_BUILD_TYPE release) 
SET(CMAKE_C_COMPILER "/usr/bin/clang")
SET(CMAKE_CXX_COMPILER "/usr/bin/clang++")
SET(CMAKE_LINKER  "/usr/bin/llvm-ld")

add_definitions(-D_UNICODE)
add_definitions(-DUNICODE)
add_definitions(-D__linux__) 

find_package(Boost COMPONENTS system filesystem thread random)
find_package(GLUT REQUIRED)
find_package(OpenGL REQUIRED)
find_package(Qt4 REQUIRED QtCore QtGui QtOpenGL)

include(${QT_USE_FILE})
find_package(CURL REQUIRED)

set(CINDER_ROOT "/home/bchandra/workspace/Cinder")
set(OPENNI_ROOT "/home/bchandra/kinect/OpenNI")
set(OPENNI_INCLUDE ${OPENNI_ROOT}/Include)

include_directories(
	${CINDER_ROOT}/include 
	/usr/include/boost
	/usr/include/cairo
	${CINDER_ROOT}/src/libtess2
	${CINDER_ROOT}/src/AntTweakBar
	${QT_INCLUDES}
	${OPENNI_INCLUDE}
)


set(CINDER_LIB_DIR ${CINDER_ROOT}/build/cinder)
set(CINDER_LIBRARIES
	${CINDER_LIB_DIR}/app/libapp.a
	${CINDER_LIB_DIR}/gl/libgl.a
	${CINDER_LIB_DIR}/cairo/libcairo.a
	${CINDER_LIB_DIR}/libcinder.a
	${CINDER_LIB_DIR}/ip/libip.a
	${CINDER_LIB_DIR}/params/libparams.a
	${CINDER_LIB_DIR}/../libtess2/libtess2.a
	${CINDER_LIB_DIR}/../AntTweakBar/libAntTweakBar.a
	)

set(CAIRO_LIBRARIES
	/usr/lib/i386-linux-gnu/libcairo.a
	/usr/lib/i386-linux-gnu/libpixman-1.a
	)
	
set(V4L2_LIBRARIES
	/usr/lib/i386-linux-gnu/libv4l1.so.0
	/usr/lib/i386-linux-gnu/libv4l2.so.0
	/usr/lib/i386-linux-gnu/libv4lconvert.so.0
	)	
	
set(OPENNI_LIBRARIES
    /usr/lib/libOpenNI.so
    )
    
set(OPENCV_LIBRARIES
    /usr/lib/libopencv_gpu.so.2.3.1
    /usr/lib/libopencv_video.so.2.3.1
    /usr/lib/libopencv_objdetect.so.2.3.1
    /usr/lib/libopencv_imgproc.so.2.3.1
    /usr/lib/libopencv_contrib.so.2.3.1
    /usr/lib/libopencv_core.so.2.3.1
    )
   

set(Cinder-KissFft_LINK_LIBRARIES 
	${CINDER_LIBRARIES}
	${CAIRO_LIBRARIES}
	${OPENGL_LIBRARIES}
	${QT_LIBRARIES}
	${CURL_LIBRARIES}
	${Boost_LIBRARIES}
	${V4L2_LIBRARIES}
	${OPENNI_LIBRARIES}
	${OPENCV_LIBRARIES}
)

add_subdirectory(src/kiss)
add_library(cinder-kissfft
    src/KissFFT.cpp
    src/KissFFT.h
)

target_link_libraries(cinder-kissfft src/kiss/libkiss_fft.a)
include_directories(src)