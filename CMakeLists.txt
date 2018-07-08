cmake_minimum_required(VERSION 3.0)

add_subdirectory(file-util)
add_subdirectory(axml-parser)
add_subdirectory(mcpelauncher-apkinfo)
add_subdirectory(mcpelauncher-extract)
add_subdirectory(google-play-api)
add_subdirectory(playdl-signin-ui-qt)
add_subdirectory(mcpelauncher-ui-qt)

option(GAME_LAUNCHER_PATH "Additional paths in which the game launcher should be looked in. Relative to the application directory." )
if (GAME_LAUNCHER_PATH)
    target_compile_definitions(mcpelauncher-ui-qt PRIVATE -DGAME_LAUNCHER_PATH="${GAME_LAUNCHER_PATH}")
endif()
