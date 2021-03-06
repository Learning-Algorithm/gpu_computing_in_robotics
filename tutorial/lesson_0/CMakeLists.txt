cmake_minimum_required(VERSION 2.8 FATAL_ERROR)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/../../cmake")

project(GPU_Computing_in_Robotics_tutorial_lesson_0)

#Include cuda, glut directory, where headers sit
set(GLUT_glut_LIBRARY /home/lz/.conan/data/cuda/10.0.130/ar/stable/package/5ab84d6acfe1f23c4fae0ab88f26e3a396351ac9/cuda_toolkit/samples/common/inc/GL/freeglut.h)
set(GLUT_INCLUDE_DIR /home/lz/.conan/data/cuda/10.0.130/ar/stable/package/5ab84d6acfe1f23c4fae0ab88f26e3a396351ac9/cuda_toolkit/samples/common/inc/GL/)


# include pcl package
set(PCL_DIR /home/lz/.conan/data/pcl/1.10.0/ar/stable/package/9bfdcfa2bb925892ecf42e2a018a3f3529826676/share/pcl-1.10/)
# set(PCL_LIBRARIES /home/lz/.conan/data/pcl/1.10.0/ar/stable/package/9bfdcfa2bb925892ecf42e2a018a3f3529826676/lib)
set(PCL_INCLUDE_DIRS /home/lz/.conan/data/pcl/1.10.0/ar/stable/package/9bfdcfa2bb925892ecf42e2a018a3f3529826676/include/pcl-1.10)
set(PCL_LIBRARY_DIRS /home/lz/.conan/data/pcl/1.10.0/ar/stable/package/9bfdcfa2bb925892ecf42e2a018a3f3529826676/lib/)
set(EIGEN3_INCLUDE_DIRS /home/lz/Documents/eigen/include/)
set(EIGEN3_DIR /home/lz/Documents/eigen/include/)

set(ENV{PKG_CONFIG_PATH}  "/home/lz/Documents/eigen/prefix/share/pkgconfig")
find_package(PkgConfig REQUIRED)

# find_package(Eigen3 3.3.7 PATHS /home/lz/.conan/data/eigen/3.3.7/conan/stable/package/5ab84d6acfe1f23c4fae0ab88f26e3a396351ac9/include/eigen3/Eigen/)
# find_package(Eigen3 3.3.7 PATHS /home/lz/Documents/eigen/include)

pkg_search_module(Eigen3 3.3.90 REQUIRED eigen3)  # this looks for opencv.pc file
find_package(Eigen3 3.3.90)

find_package(OpenGL REQUIRED)
find_package(GLUT REQUIRED)
find_package(PCL 1.9 REQUIRED) #PATHS /home/lz/.conan/data/pcl/1.10.0/ar/stable/package/9bfdcfa2bb925892ecf42e2a018a3f3529826676)
find_package(CUDA REQUIRED)
find_package(CUDADevice 3.0 REQUIRED)
## OpenGL
# find_package(GLUT REQUIRED)
if (GLUT_FOUND)
    message(STATUS "Debug (lei): Libraries for glut found")
else()
    message(STATUS "Debug (lei): Failed to find glut.")
endif()
set_directory_properties( PROPERTIES COMPILE_DEFINITIONS "" )

include_directories(include)
include_directories(${PCL_INCLUDE_DIRS})
add_definitions(${PCL_DEFINITIONS})

include_directories(SYSTEM ${EIGEN3_INCLUDE_DIRS})
include_directories(SYSTEM ${GLUT_INCLUDE_DIR})

set(CUDA_NVCC_FLAGS "${CUDA_NVCC_FLAGS} ${CUDADEVICE_NVCC_FLAGS} -Xptxas -v")

cuda_add_executable(lesson_0
	src/lesson_0_main.cpp
	src/cudaWrapper.cpp
	src/lesson_0.cu
)
target_link_libraries(lesson_0
	${OPENGL_LIBRARIES}
	${GLUT_LIBRARY}
	${PCL_LIBRARIES}
)
