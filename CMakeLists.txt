cmake_minimum_required(VERSION 3.7)
project(QOAED CXX)

set(CMAKE_CXX_STANDARD 17)
set(TEST_SRC_DIR ${PROJECT_SOURCE_DIR}/test)
set(TEST_BIN_DIR ${PROJECT_BINARY_DIR}/bin)

file(MAKE_DIRECTORY ${PROJECT_BINARY_DIR}/bin)

# Search for pkg-config
#find_package(PkgConfig REQUIRED)
#pkg_search_module(X11 REQUIRED x11)

## Configure PCL 
# Search for PCL package
find_package(PCL 1.3 REQUIRED)

if(NOT "${PCL_LIBRARIES}" STREQUAL "")
    list(REMOVE_ITEM PCL_LIBRARIES "vtkproj4")
endif()


# Setting up PCL
include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})


include_directories(include)

add_executable(point_quadtree  ${TEST_SRC_DIR}/PointQuadtreeTest.cpp)
add_executable(point_octree    ${TEST_SRC_DIR}/PointOctreeTest.cpp)
add_executable(region_quadtree ${TEST_SRC_DIR}/RegionQuadtreeTest.cpp)
add_executable(region_octree   ${TEST_SRC_DIR}/RegionOctreeTest.cpp)
add_executable(pcl_lib         ${TEST_SRC_DIR}/PCLTest.cpp)
add_executable(tools           ${TEST_SRC_DIR}/ToolsTest.cpp)
add_executable(octree_demo     ${TEST_SRC_DIR}/OctreeDemo.cpp)

#target_link_libraries(point_octree ${PCL_LIBRARIES})

target_link_libraries(pcl_lib ${PCL_LIBRARIES})
target_link_libraries(octree_demo ${PCL_LIBRARIES})

set_target_properties(
  point_quadtree
  point_octree
  region_quadtree
  region_octree
  pcl_lib
  tools
  octree_demo

	PROPERTIES

	RUNTIME_OUTPUT_DIRECTORY "${TEST_BIN_DIR}"
  #LINK_LIBRARIES           "${PCL_LIBRARIES}"
)

