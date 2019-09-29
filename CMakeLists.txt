cmake_minimum_required(VERSION 3.12.4)
get_directory_property(hasParent PARENT_DIRECTORY)
if(NOT hasParent)
	option(unittests "unittests" OFF)
	get_filename_component(_PARENT_DIR ${CMAKE_CURRENT_SOURCE_DIR} DIRECTORY)
	set_property(GLOBAL PROPERTY GLOBAL_FETCHDEPS_BASE ${_PARENT_DIR}/al2o3 )
	include(FetchContent)
	FetchContent_Declare( al2o3 GIT_REPOSITORY https://github.com/DeanoC/al2o3 GIT_TAG master )
	FetchContent_GetProperties(al2o3)
	if(NOT al2o3_POPULATED)
		FetchContent_Populate(al2o3)
		add_subdirectory(${al2o3_SOURCE_DIR} ${al2o3_BINARY_DIR})
	endif()
	INIT_AL2O3(${CMAKE_CURRENT_SOURCE_DIR})
endif ()

SET_MIN_VERSIONS()
set(LibName gfx_spirvcross)
project(${LibName})

set(Interface
		../spirvcross/barrier.hpp
		../spirvcross/external_interface.h
		../spirvcross/image.hpp
		../spirvcross/internal_interface.hpp
		../spirvcross/sampler.hpp
		../spirvcross/thread_group.hpp
		)

set(Src
		GLSL.std.450.h
		spirv.h
		spirv.hpp
		spirv_cfg.cpp
		spirv_cfg.hpp
		spirv_common.hpp
		spirv_cpp.cpp
		spirv_cpp.hpp
		spirv_cross.cpp
		spirv_cross.hpp
		spirv_cross_c.cpp
		spirv_cross_c.h
		spirv_cross_containers.hpp
		spirv_cross_error_handling.hpp
		spirv_cross_parsed_ir.cpp
		spirv_cross_parsed_ir.hpp
		spirv_cross_util.cpp
		spirv_cross_util.hpp
		spirv_glsl.cpp
		spirv_hlsl.cpp
		spirv_msl.cpp
		spirv_glsl.hpp
		spirv_hlsl.hpp
		spirv_msl.hpp
		spirv_parser.cpp
		spirv_parser.hpp
		spirv_reflect.cpp
		spirv_reflect.hpp
		)
set(Deps
		)

ADD_LIB(${LibName} "${Interface}" "${Src}" "${Deps}")
target_include_directories(${LibName} PUBLIC src) # for shaderc
