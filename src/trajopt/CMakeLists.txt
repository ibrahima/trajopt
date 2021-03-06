include_directories(
  ${BULLET_INCLUDE_DIRS}  
  ${Boost_INCLUDE_DIRS}
  ${Eigen_INCLUDE_DIRS}
  ${OpenRAVE_INCLUDE_DIRS}
  ${JSON_INCLUDE_DIRS}
)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-deprecated-declarations")
add_library(trajopt SHARED 
	json_marshal.cpp
	problem_description.cpp
	bullet_collision_checker.cpp 
	kinematic_constraints.cpp 
	robot_and_dof.cpp 
	utils.cpp
	rave_utils.cpp
	collision_avoidance.cpp
	collision_checker.cpp
	plot_callback.cpp
)
target_link_libraries(trajopt ${BULLET_LIBRARIES} ${OpenRAVE_BOTH_LIBRARIES} ${Boost_SYSTEM_LIBRARY} sco utils json osgviewer)

# set(CMAKE_SHARED_LINKER_FLAGS "-export-symbols-regex,'^foo_'")

#add_library(trajoptplugin SHARED plugin.cpp)
#set_target_properties(trajoptplugin PROPERTIES COMPILE_FLAGS "${OpenRAVE_CXX_FLAGS}" LINK_FLAGS "${OpenRAVE_LINK_FLAGS}")
#target_link_libraries(trajoptplugin ${OpenRAVE_LIBRARIES} trajopt)

include_directories(${PYTHON_NUMPY_INCLUDE_DIR})
boost_python_module(ctrajoptpy trajoptpy.cpp)
target_link_libraries(ctrajoptpy trajopt ${Boost_SYSTEM_LIBRARY} )

add_subdirectory(test)

