#
#
# Project script for stdshaders
#
#
project(game_shader_dx9 C CXX)

set(SRCS
  "BaseVSShader.cpp"
	"Bloom.cpp"
	"screenspace_general.cpp"
  "BaseVSShader.h"
	"common_fxc.h"
	"common_hlsl_cpp_consts.h"
	"common_ps_fxc.h"
	"common_vertexlitgeneric_dx9.h"
	"common_vs_fxc.h"
	"shader_constant_register_map.h"

  # Examples
  "example_model_dx9.cpp"
	"example_model_dx9_helper.cpp"
	"example_model_dx9_helper.h")

# Preprocessor defs
set(DEFINES	-DSTDSHADER_DX9_DLL_EXPORT -DFAST_MATERIALVAR_ACCESS -DGAME_SHADER_DLL -DUSE_ACTUAL_DX)

# Generic include dirs
set(INCLUDE_DIRS fxctmp9)

# Set the variable of target for stuff
set(TARGET game_shader_dx9)

# Set the output file name
set(OUTPUT_FILE_NAME game_shader_dx9)

# If this is a shared lib
set(SHARED_LIB 1)

# If this is a static lib
set(STATIC_LIB 0)

# Include this to handle all the defines
include(${CMAKESCRIPTS_DIR}/library-base.cmake)

# Links dirs to search for link libs in
set(POSIX_LINK_DIRS		${PUBLIC_LIB}/linux32/ ${COMMON_LIB}/linux32/)

# Other static libs we need
set(WINDOWS_LINK_LIBS tier3.lib tier2.lib choreoobjects.lib dmxloader.lib matsys_controls.lib particles.lib
	vtf.lib bitmap.lib vgui_controls.lib)

set(WINDOWS_LINK_LIBS
	tier1
	${ROOT}//lib/public/libprotobuf.lib
	${ROOT}//lib/public/bitmap.lib
	${ROOT}//lib/public/choreoobjects.lib
	mathlib
	${ROOT}//lib/public/matsys_controls.lib
	${ROOT}//lib/public/particles.lib
	${ROOT}//lib/public/dmxloader.lib
	${ROOT}//lib/public/tier2.lib
	${ROOT}//lib/public/tier3.lib
	${ROOT}//lib/public/vtf.lib
	${ROOT}//lib/common/libcrypto.lib
	${ROOT}//lib/public/libz.lib
	${ROOT}//lib/common/libcurl.lib
  ${ROOT}/lib/public/shaderlib.lib
	vstdlib
	steam_api
	tier0)

set(POSIX_LINK_LIBS
	${ROOT}//lib/public/linux32/libprotobuf.a
	${ROOT}//lib/public/linux32/bitmap.a
	${ROOT}//lib/public/linux32/choreoobjects.a
	${ROOT}//lib/public/linux32/matsys_controls.a
	${ROOT}//lib/public/linux32/particles.a
	${ROOT}//lib/public/linux32/dmxloader.a
	${ROOT}//lib/public/linux32/tier2.a
	${ROOT}//lib/public/linux32/tier3.a
	${ROOT}//lib/public/linux32/vtf.a
	${ROOT}//lib/common/linux32/libcrypto.a
	${ROOT}//lib/public/linux32/libz.a
	${ROOT}//lib/common/linux32/libcurl.a
	${ROOT}//lib/common/linux32/libcurlssl.a
	${ROOT}//lib/common/linux32/libssl.a
  ${ROOT}/lib/public/linux32/shaderlib.a
	steam_api
	mathlib
	tier1
	vstdlib
	tier0
	m
	c
	dl
	rt
	pthread)

if(DEFINED WINDOWS)
	target_link_libraries(game_shader_dx9 ${WINDOWS_LINK_LIBS})
endif(DEFINED WINDOWS)

if(DEFINED POSIX)
	target_link_libraries(game_shader_dx9 ${POSIX_LINK_LIBS})
endif(DEFINED POSIX)

install(TARGETS game_shader_dx9
		RUNTIME DESTINATION ${MOD_DIR}/bin
		LIBRARY DESTINATION ${MOD_DIR}/bin)
