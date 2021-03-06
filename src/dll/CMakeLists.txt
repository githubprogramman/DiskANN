add_library(${PROJECT_NAME} SHARED dllmain.cpp ../partition_and_pq.cpp ../logger.cpp ../utils.cpp ../index.cpp ../math_utils.cpp ../aux_utils.cpp ../ann_exception.cpp)
if (MSVC)
	add_definitions(-D_USRDLL -D_WINDLL -DDISKANN_DLL)
	add_compile_options(/MD)
	target_link_options(${PROJECT_NAME} PRIVATE /DLL /MACHINE:X64 /DEBUG:FULL "/INCLUDE:_tcmalloc")
	target_link_options(${PROJECT_NAME} PRIVATE $<$<CONFIG:Debug>:/IMPLIB:${CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG}/${PROJECT_NAME}.lib>
										$<$<CONFIG:Release>:/IMPLIB:${CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE}/${PROJECT_NAME}.lib>
					   )
	target_link_libraries(${PROJECT_NAME} debug  ${PROJECT_SOURCE_DIR}/dependencies/windows/tcmalloc/libtcmalloc_minimal.lib)
	target_link_libraries(${PROJECT_NAME} optimized ${PROJECT_SOURCE_DIR}/dependencies/windows/tcmalloc/libtcmalloc_minimal.lib)
					

	add_custom_command(TARGET 
							${PROJECT_NAME} 
							POST_BUILD 
							COMMAND ${CMAKE_COMMAND} -E copy ${INTEL_ROOT}redist/intel64/compiler/libiomp5md.dll "$<$<CONFIG:debug>:${CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG}>$<$<CONFIG:release>:${CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE}>" )
	add_custom_command(TARGET 
							${PROJECT_NAME} 
							POST_BUILD 
							COMMAND ${CMAKE_COMMAND} -E copy ${INTEL_ROOT}redist/intel64/mkl/mkl_avx.dll "$<$<CONFIG:debug>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG}\">$<$<CONFIG:release>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE}\">" )
	add_custom_command(TARGET 
							${PROJECT_NAME} 
							POST_BUILD 
							COMMAND ${CMAKE_COMMAND} -E copy ${INTEL_ROOT}redist/intel64/mkl/mkl_avx2.dll "$<$<CONFIG:debug>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG}\">$<$<CONFIG:release>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE}\">" )
	add_custom_command(TARGET 
							${PROJECT_NAME} 
							POST_BUILD 
							COMMAND ${CMAKE_COMMAND} -E copy ${INTEL_ROOT}redist/intel64/mkl/mkl_avx512.dll "$<$<CONFIG:debug>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG}\">$<$<CONFIG:release>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE}\">" )
	add_custom_command(TARGET 
							${PROJECT_NAME} 
							POST_BUILD 
							COMMAND ${CMAKE_COMMAND} -E copy ${INTEL_ROOT}redist/intel64/mkl/mkl_core.dll "$<$<CONFIG:debug>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG}\">$<$<CONFIG:release>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE}\">" )
	add_custom_command(TARGET 
							${PROJECT_NAME} 
							POST_BUILD 
							COMMAND ${CMAKE_COMMAND} -E copy ${INTEL_ROOT}redist/intel64/mkl/mkl_def.dll "$<$<CONFIG:debug>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG}\">$<$<CONFIG:release>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE}\">" )
	add_custom_command(TARGET 
							${PROJECT_NAME} 
							POST_BUILD 
							COMMAND ${CMAKE_COMMAND} -E copy ${INTEL_ROOT}redist/intel64/mkl/mkl_intel_thread.dll "$<$<CONFIG:debug>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG}\">$<$<CONFIG:release>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE}\">" )
	add_custom_command(TARGET 
							${PROJECT_NAME} 
							POST_BUILD 
							COMMAND ${CMAKE_COMMAND} -E copy ${INTEL_ROOT}redist/intel64/mkl/mkl_rt.dll "$<$<CONFIG:debug>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG}\">$<$<CONFIG:release>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE}\">" )
	add_custom_command(TARGET 
							${PROJECT_NAME} 
							POST_BUILD 
							COMMAND ${CMAKE_COMMAND} -E copy ${PROJECT_SOURCE_DIR}/dependencies/windows/tcmalloc/libtcmalloc_minimal.dll "$<$<CONFIG:debug>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG}\">$<$<CONFIG:release>:\"${CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE}\">" )


endif()



