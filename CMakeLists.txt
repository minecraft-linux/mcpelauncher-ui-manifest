cmake_minimum_required(VERSION 3.0)

string(TIMESTAMP BUILD_TIMESTAMP "%Y%m%d-%H%M%S" UTC)

find_program(GIT_EXEC git)
mark_as_advanced(GIT_EXEC ADVANCED)
function(git_commit_hash DIR OUT_VAL)
    if (GIT_EXEC)
        execute_process(
                COMMAND ${GIT_EXEC} log -1 --format=%h
                WORKING_DIRECTORY ${DIR}
                OUTPUT_VARIABLE OUTPUT
                OUTPUT_STRIP_TRAILING_WHITESPACE
        )
        set(${OUT_VAL} ${OUTPUT} PARENT_SCOPE)
    else()
        set(${OUT_VAL} "" PARENT_SCOPE)
    endif()
endfunction()
git_commit_hash(${CMAKE_SOURCE_DIR} MANIFEST_GIT_COMMIT_HASH)

set(DEB_OS_NAME "none" CACHE STRING "Specifies the target OS for building the debs (ubuntu-xenial, ubuntu-bionic, ubuntu-disco)")
mark_as_advanced(DEB_OS_NAME)

add_subdirectory(file-util)
add_subdirectory(axml-parser)
add_subdirectory(mcpelauncher-apkinfo)
add_subdirectory(mcpelauncher-common)
add_subdirectory(mcpelauncher-extract)
add_subdirectory(google-play-api)
add_subdirectory(playdl-signin-ui-qt)
add_subdirectory(mcpelauncher-ui-qt)

if (ENABLE_APPIMAGE_UPDATE_CHECK)
add_subdirectory(lib/AppImageUpdate)
endif()

set(GAME_LAUNCHER_PATH "" CACHE STRING "Additional paths in which the game launcher should be looked in. Relative to the application directory.")
if (GAME_LAUNCHER_PATH)
    target_compile_definitions(mcpelauncher-ui-qt PRIVATE -DGAME_LAUNCHER_PATH="${GAME_LAUNCHER_PATH}")
endif()
