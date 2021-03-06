cmake_minimum_required(VERSION 2.4.6)
include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)

rosbuild_init()

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#uncomment if you have defined messages
#rosbuild_genmsg()
#uncomment if you have defined services
#rosbuild_gensrv()

#common commands for building c++ executables and libraries
#rosbuild_add_library(${PROJECT_NAME} src/example.cpp)
#target_link_libraries(${PROJECT_NAME} another_library)
#rosbuild_add_boost_directories()
#rosbuild_link_boost(${PROJECT_NAME} thread)
#rosbuild_add_executable(example examples/example.cpp)
#target_link_libraries(example ${PROJECT_NAME})

rosbuild_add_library (pcl_cloud_algos
  src/pcl_cloud_algos/box_fit_algo.cpp
  src/pcl_cloud_algos/box_fit2_algo.cpp
  #src/pcl_cloud_algos/segment_differences.cpp
  #src/pcl_cloud_algos/global_rsd.cpp
  src/pcl_cloud_algos/depth_image_triangulation.cpp
  #src/pcl_cloud_algos/svm_classification.cpp
)
target_link_libraries (pcl_cloud_algos pcl_ias_sample_consensus)

rosbuild_add_executable (box_fit_node  src/pcl_cloud_algos/box_fit_algo.cpp)
get_target_property(prev_props box_fit_node COMPILE_FLAGS)
set_target_properties (box_fit_node PROPERTIES COMPILE_FLAGS "${prev_props} -DCREATE_NODE")

rosbuild_add_executable (box_fit2_node  src/pcl_cloud_algos/box_fit_algo.cpp src/pcl_cloud_algos/box_fit2_algo.cpp)
get_target_property(prev_props box_fit2_node COMPILE_FLAGS)
set_target_properties (box_fit2_node PROPERTIES COMPILE_FLAGS "${prev_props} -DCREATE_NODE -DNO_BOXFIT_NODE")

#rosbuild_add_executable (segment_differences_node  src/pcl_cloud_algos/segment_differences.cpp)

#rosbuild_add_executable (global_rsd_node  src/pcl_cloud_algos/global_rsd.cpp)
#get_target_property(prev_props global_rsd_node COMPILE_FLAGS)
#set_target_properties (global_rsd_node PROPERTIES COMPILE_FLAGS "${prev_props} -DCREATE_NODE")

rosbuild_add_executable (depth_image_triangulation_node  src/pcl_cloud_algos/depth_image_triangulation.cpp)
get_target_property(prev_props depth_image_triangulation_node COMPILE_FLAGS)
set_target_properties (depth_image_triangulation_node PROPERTIES COMPILE_FLAGS "${prev_props} -DCREATE_NODE")

#rosbuild_add_executable (svm_classification_node  src/pcl_cloud_algos/svm_classification.cpp)
#get_target_property(prev_props svm_classification_node COMPILE_FLAGS)
#set_target_properties (svm_classification_node PROPERTIES COMPILE_FLAGS "${prev_props} -DCREATE_NODE")

#for Quentin
rosbuild_add_executable (sigIllDemo  test/sigIllDemo.cpp)
target_link_libraries(sigIllDemo pcl_cloud_algos)
