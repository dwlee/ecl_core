##############################################################################
# Cmake
##############################################################################

cmake_minimum_required(VERSION 2.8.3)
project(ecl_eigen)

##############################################################################
# Catkin
##############################################################################

find_package(catkin REQUIRED)

##############################################################################
# Internal vs External Eigen
##############################################################################

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

if(DEFINED ECL_EIGEN_IS_INTERNAL_3_1_2)
#    set(ECL_EIGEN_IS_INTERNAL_3_1_2 1)
    message(STATUS "Using ecl's internal eigen.")
    catkin_package(
            INCLUDE_DIRS include/eigen3.1.2;include/eigen3.1.2/Eigen   # the second directory ensures the unsupported modules are available
            )
else()
    find_package(Eigen)
    set(ECL_EIGEN_IS_EXTERNAL 1 CACHE BOOL "Using the system's eigen implementation.")
    if(EIGEN_INCLUDE_DIR)
        catkin_package(
            DEPENDS Eigen
        )
    else()
        message(STATUS "Eigen not found...using ecl's internal eigen.")
        set(ECL_EIGEN_IS_INTERNAL_3_1_2 1 CACHE BOOL "Using ecl's internal eigen implementation version 3.1.2")
        catkin_package(
            INCLUDE_DIRS include/eigen3.1.2;include/eigen3.1.2/Eigen   # the second directory ensures the unsupported modules are available
        )
    endif()
endif()

##############################################################################
# Configure header macros
##############################################################################

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/include/ecl/eigen.hpp.in ${CATKIN_DEVEL_PREFIX}/include/ecl/eigen.hpp) 
install(FILES ${CATKIN_DEVEL_PREFIX}/include/ecl/eigen.hpp DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}/ecl)

##############################################################################
# Actual project configuration
##############################################################################

add_subdirectory(include)
add_subdirectory(license)


