#root/components/cameras/CMakeLists.txt

###############################################################################
# CONFIGURE CORE PATH
###############################################################################
set (CAMERAS_PATH ${PROJECT_SOURCE_DIR}/components/cameras)

###############################################################################
# CREATE FILES SET
###############################################################################


file(GLOB CAMERAS_CAMERA_FILES		"${CAMERAS_PATH}/include/cameras/ip_camera.hpp"
									"${CAMERAS_PATH}/src/ip_camera.cpp"
									"${CAMERAS_PATH}/include/cameras/ip_video_camera.hpp"
									"${CAMERAS_PATH}/src/ip_video_camera.cpp"
									)
									
file(GLOB CAMERAS_PTZ_FILES			"${CAMERAS_PATH}/include/cameras/ip_video_camera_PTZ.hpp"
									"${CAMERAS_PATH}/src/ip_video_camera_PTZ.cpp"
									"${CAMERAS_PATH}/include/cameras/ip_video_camera_PTZ_Trend_Net7865.hpp"
									"${CAMERAS_PATH}/src/ip_video_camera_PTZ_Trend_Net7865.cpp"
									"${CAMERAS_PATH}/include/cameras/ip_onvif.hpp"
									"${CAMERAS_PATH}/src/ip_onvif.cpp"
									"${CAMERAS_PATH}/include/cameras/ip_fixa.hpp"
									"${CAMERAS_PATH}/src/ip_fixa.cpp"
									"${CAMERAS_PATH}/include/cameras/ip_onvif_ptz.hpp"
									"${CAMERAS_PATH}/src/ip_onvif_ptz.cpp"
									)

file(GLOB CAMERAS_UTIL_FILES		"${CAMERAS_PATH}/include/cameras/cameras_defs.hpp"
									)

							
file(GLOB CORE_INCLUDES				"${CAMERAS_PATH}/include/cameras/*.hpp")

###############################################################################
# CREATE SOURCE GROUPS
###############################################################################
source_group("Cameras"	FILES ${CAMERAS_CAMERA_FILES})
source_group("PTZ"		FILES ${CAMERAS_PTZ_FILES})
source_group("Util"		FILES ${CAMERAS_UTIL_FILES})

###############################################################################
# ADD LIBRARY CORE
###############################################################################

#Add library core into SSF project with its project
add_library(ssf_cameras 	${CAMERAS_CAMERA_FILES}
							${CAMERAS_PTZ_FILES}
							${CAMERAS_UTIL_FILES}
							)
							
target_include_directories(ssf_cameras PUBLIC ${CAMERAS_PATH}/include/)

target_link_libraries(ssf_cameras ssf_core)
						
#Configure folder to core library
set_target_properties(ssf_cameras PROPERTIES FOLDER COMPONENTS)

target_compile_definitions(ssf_cameras PUBLIC CAMERAS_API_EXPORTS)

#Install configuration
install (TARGETS ssf_cameras DESTINATION bin)
install (FILES ${CAMERAS_INCLUDES} DESTINATION include)

#Link External Libraries
ssf_link_boost(ssf_cameras system filesystem log log_setup thread date_time)


###############################################################################
# SET TESTS FOR LIBRARY CORE
###############################################################################

if(BUILD_TESTS)	
	add_subdirectory(test)
endif()

if(BUILD_APPS)	
	add_subdirectory(app)
endif()
