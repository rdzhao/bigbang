project(bigbang)
cmake_minimum_required(VERSION 2.8)

list(APPEND CMAKE_CXX_FLAGS "-fopenmp -std=c++11 -fpermissive")
set(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake/;${CMAKE_MODULE_PATH}")

include(geo_sim_sdk)
include_geo_sim_sdk()
link_geo_sim_sdk()
include_directories(${PROJECT_SOURCE_DIR})

# BOOST
set(Boost_USE_STATIC_LIBS ON)
set(Boost_USE_MULTITHREADED ON)
set(Boost_USE_STATIC_RUNTIME OFF)
find_package(Boost COMPONENTS filesystem system program_options REQUIRED)

# EIGEN3
find_package(Eigen3 REQUIRED)
if(EIGEN3_FOUND)
    message("-- Eigen version: ${EIGEN3_VERSION}")
    include_directories(${EIGEN3_INCLUDE_DIR})
endif(EIGEN3_FOUND)

# UMFPACK
find_package(UMFPACK REQUIRED)
if(UMFPACK_FOUND)
    include_directories(${UMFPACK_INCLUDES})
endif(UMFPACK_FOUND)

# SUITESPARSE
find_package(SuiteSparse REQUIRED)
if(SuiteSparse_FOUND)
    include_directories(${SuiteSparse_INCLUDE_DIRS})
endif(SuiteSparse_FOUND)

# LBFGS
find_package(LBFGS REQUIRED)
if(LBFGS_FOUND)
    include_directories(${LBFGS_INCLUDE_DIRS})
endif(LBFGS_FOUND)

# ARPACK
find_package(Arpack REQUIRED)

# ALGLIB
find_package(ALGLIB)
if(ALGLIB_FOUND)
    message("-- Alglib @ ${ALGLIB_INCLUDES}")
    include_directories(${ALGLIB_INCLUDES})
endif(ALGLIB_FOUND)

# CUDA
find_package(CUDA)
if(CUDA_FOUND)
    add_definitions(-DUSE_CUDA)
    include_directories(${CUDA_INCLUDE_DIRS})
endif(CUDA_FOUND)

add_subdirectory(src)
add_subdirectory(examples)
add_subdirectory(tmp)

