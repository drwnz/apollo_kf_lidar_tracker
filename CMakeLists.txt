cmake_minimum_required(VERSION 2.8.3)
project(apollo_kf_lidar_tracker)

find_package(catkin REQUIRED COMPONENTS
    roscpp
    std_msgs
    pcl_ros
    pcl_conversions
)

catkin_package(CATKIN_DEPENDS
    sensor_msgs
)

find_package(PCL 1.7 REQUIRED)


###########
## Build ##
###########

include_directories(
    nodes/euclidean_cluster/includes
    ${catkin_INCLUDE_DIRS}
    include
)

SET(CMAKE_CXX_FLAGS "-std=c++11 -O2 -g -Wall ${CMAKE_CXX_FLAGS}")

link_directories(${PCL_LIBRARY_DIRS})

# Apollo KF Tracking Node
#add_executable(apollo_kf_lidar_track
#    nodes/apollo_kf_lidar_track_node/apollo_kf_lidar_track.cpp
#)

# Apollo KF Tracking Node
add_executable(apollo_kf_lidar_track
				nodes/apollo_lidar_track/apollo_kf_lidar_track.cpp
				nodes/apollo_lidar_track/ApolloTracker.cpp
				nodes/apollo_lidar_track/feature_descriptor.cpp
				nodes/apollo_lidar_track/hm_tracker.cpp
        nodes/apollo_lidar_track/hungrian_matcher.cpp
        nodes/apollo_lidar_track/kalman_filter.cpp
        nodes/apollo_lidar_track/object_track.cpp
        nodes/apollo_lidar_track/track_object_distance.cpp
        nodes/apollo_lidar_track/tracked_object.cpp
				nodes/euclidean_cluster/Cluster.cpp
				)
target_include_directories(kf_lidar_track PRIVATE nodes/apollo_kf_lidar_track/includes)
target_link_libraries(apollo_kf_lidar_track ${catkin_LIBRARIES} ${PCL_LIBRARIES})
add_dependencies(apollo_kf_lidar_track ${catkin_EXPORTED_TARGETS})
