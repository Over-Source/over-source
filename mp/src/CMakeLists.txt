# Root CMake script - adapted from work on other projects.
cmake_minimum_required(VERSION 3.5)
project(Source-2013)

#set(CMAKE_CONFIGURATION_TYPES "Debug;Release" CACHE STRING "" FORCE)
set(CMAKE_EXPORT_COMPILE_COMMANDS "ON")

option(DEV_BUILD "Enables development builds" ON)
option(BUILD_64BIT "Specifies this build as 64-bit. Experiemental." OFF)
option(HL2 "Build HL2 game stuff" ON)
option(EP1 "Build HL2:EP1 game stuff" ON)
option(HL2MP "Build HL2MP game stuff" ON)

# Definitions that should always be enabled
add_definitions(-DFRAME_POINTER_OMISSION_DISABLED -DRAD_TELEMETRY_DISABLED)
add_definitions(-DVERSION_SAFE_STEAM_API_INTERFACES -UDPROTECTED_THINGS_DISABLE -DPROTECTED_THINGS_DISABLE)

# Set dedicated server vars
if(SERVER_BUILD)
	add_definitions(-DDEDICATED)
endif(SERVER_BUILD)

if(NOT DEV_BUILD)
	add_definitions(-DDEV_BUILD)
endif(NOT DEV_BUILD)

# Platform specific
if(NOT BUILD_64BIT)
	add_definitions(-DPLATFORM_32BITS -D_x86_ -D_X86_)
else()
	add_definitions(-DPLATFORM_64BITS -D_x64_ -D_X64_ -D_x86_64_)
endif(NOT BUILD_64BIT)


#
# WINDOWS
#
if(DEFINED CMAKE_HOST_WIN32)
	add_definitions(-DCOMPILER_MSVC)
endif()

#
# WINDOWS VARS
#
if(DEFINED CMAKE_HOST_WIN32)
	set(WINDOWS_BUILD 1)
	set(WINDOWS 1)
	add_definitions(-D_DLL_EXT=.dll -D_EXTERNAL_DLL_EXT=.dll)
	if(BUILD_64BIT)
		message(WARNING "64-bit builds are experiemental!")
		set(WIN64_BUILD 1)
		set(PLATFORM_64BITS 1)
		add_definitions(-DWIN64 -D_WIN64 -DPLATFORM_WINDOWS_PC64)
		add_definitions(-DCOMPILER_MSVC64)
		set(PLATFORM_SUBDIR win64)
	else()
		set(WIN32_BUILD 1)
		set(PLATFORM_32BITS 1)
		add_definitions(-DWIN32 -D_WIN32 -DPLATFORM_WINDOWS_PC32)
		add_definitions(-DCOMPILER_MSVC32)
		set(PLATFORM_SUBDIR win32)
	endif(BUILD_64BIT)
endif()

#
# UNIX VARS
#
if(DEFINED CMAKE_HOST_UNIX)
	set(POSIX 1)
	set(LINUX 1)
	add_definitions(-D_DLL_EXT=.so -D_EXTERNAL_DLL_EXT=.so)
	add_definitions(-DLINUX -D_LINUX -DPOSIX -D_POSIX -DSOURCE1=1)
	# Common defs for all unix builds
	add_definitions(
		-DUSE_SDL 
		-DUSE_WEBM_FOR_REPLAY 
		-DGNUC 
		-DVPROF_LEVEL=1
		-DNO_HOOK_MALLOC
		-DNO_MALLOC_OVERRIDE
		-DVPCGAMECAPS=HL2MP 
		-DGL_GLEXT_PROTOTYPES 
		-DDX_TO_GL_ABSTRACTION
	)
	add_definitions(-DGNUC -DCOMPILER_GCC)

	if(BUILD_64BIT)
		message("Building for POSIX64")
		message(WARNING "64-bit builds are experiemental!")
		set(PLATFORM_64BITS 1)
		set(LINUX64 1)
		set(POSIX64 1)
		add_definitions(-DPLATFORM_64BITS -D_x64_ -D_X64_ -D__x86_64__)
		add_definitions(-DLINUX64 -D_LINUX64 -DPOSIX64 -D_POSIX64)
		set(PLATFORM_SUBDIR posix64)
		set(CMAKE_TOOLCHAIN_FILE "${POSIX64_TOOLCHAIN}")
		# Set the lib path
		set(CMAKE_LIBRARY_PATH "${CMAKE_LIBRARY_PATH} /usr/lib64")
	else()
		message("Building for POSIX32")
		set(PLATFORM_32BITS 1)
		set(LINUX32)
		set(POSIX32)
		add_definitions(-DPLATFORM_32BITS -D_x86_ -D_X86_)
		add_definitions(-DLINUX32 -D_LINUX32 -DPOSIX32 -D_LINUX32)
		set(PLATFORM_SUBDIR posix32)
		set(CMAKE_TOOLCHAIN_FILE "${POSIX32_TOOLCHAIN}")
		set(CMAKE_LIBRARY_PATH "${CMAKE_LIBRARY_PATH} /usr/lib32")
	endif(BUILD_64BIT)
endif()

# SDL2 will forcefully include AVX headers unless told not to.
add_definitions(-DSDL_DISABLE_IMMINTRIN_H)

# Change this to your mod directory or specify it on the command line
if(NOT DEFINED ${MOD_DIR})
	set(MOD_DIR mod_hl2mp)
endif()

set(CMAKESCRIPTS_DIR ${CMAKE_HOME_DIRECTORY}/cmake/)
set(ROOT_DIR ${CMAKE_HOME_DIRECTORY})
set(ROOT ${ROOT_DIR})
set(SRCDIR ${ROOT_DIR})
set(PUBLIC ${ROOT_DIR}/public)
set(ROOT ${ROOT_DIR})
set(TOP ${ROOT_DIR})
set(UTILS_COMMON ${ROOT_DIR}/utils/common)
set(UTILS ${ROOT}/utils)
set(COMMON_LIB ${ROOT}/lib/common)
set(PUBLIC_LIB ${ROOT}/lib/public)
include(cmake/source-directories.cmake)
include(cmake/source-base.cmake)

# Configure output dirs
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${ROOT}/../game/${MOD_DIR}/bin)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${ROOT}/../game/${MOD_DIR}/bin)

# Don't use lib*
set(CMAKE_SHARED_LIBRARY_PREFIX "")

# All the project dirs!
add_subdirectory(mathlib)
add_subdirectory(tier1)
add_subdirectory(raytrace)
add_subdirectory(vgui2/vgui_controls)
add_subdirectory(materialsystem/stdshaders)
add_subdirectory(game/client)
add_subdirectory(game/server)
add_subdirectory(utils/lzma)

# @TODO: Linux support for these.
if (NOT DEFINED POSIX)
	add_subdirectory(fgdlib)
	add_subdirectory(utils/vbsp)
	add_subdirectory(utils/vrad)
	add_subdirectory(utils/vrad_launcher)
	add_subdirectory(utils/vvis)
	add_subdirectory(utils/vvis_launcher)
endif()
