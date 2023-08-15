cmake_minimum_required(VERSION 3.20)

project(LearnOpenMesh)

set_property(GLOBAL PROPERTY USE_FOLDERS ON)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(OUT_RUN_PATH ${CMAKE_CURRENT_LIST_DIR}/../bin.x64)
set(OUT_LIB_PATH ${CMAKE_CURRENT_LIST_DIR}/../lib.x64)

aux_source_directory(${CMAKE_CURRENT_LIST_DIR}/src SRC)

file(GLOB INCLUDE ${CMAKE_CURRENT_LIST_DIR}/include/*.h)

source_group("Header Files" FILES ${INCLUDE})
source_group("Source Files" FILES ${SRC})


add_executable(${PROJECT_NAME}
    ${SRC}
    ${INCLUDE}
)

target_include_directories(${PROJECT_NAME} PUBLIC
    ${CMAKE_CURRENT_LIST_DIR}/include
    ${CMAKE_CURRENT_LIST_DIR}/openMeshLibrary/include
)

target_link_directories(${PROJECT_NAME} PUBLIC
    ${CMAKE_CURRENT_LIST_DIR}/openMeshLibrary/lib
)

target_link_libraries(${PROJECT_NAME} PUBLIC
	OpenMeshCore.lib
	OpenMeshTools.lib
)

set(CONFS_TYPES Debug Release RelWithDebInfo MinSizeRel)
list(APPEND CONFS_TYPES "")
foreach(type IN LISTS CONFS_TYPES)
    set(conf "")
    
    if(type)
        string(TOUPPER _${type} conf)
    endif()

    set_target_properties(${PROJECT_NAME} PROPERTIES
        RUNTIME_OUTPUT_DIRECTORY${conf} ${OUT_RUN_PATH} #dll exe
        PDB_OUTPUT_DIRECTORY${conf} ${OUT_RUN_PATH}     #pdb
    )
endforeach()