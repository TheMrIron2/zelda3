cmake_minimum_required(VERSION 3.0)

project(zelda3)

file(GLOB_RECURSE SOURCES "src/*.c" "snes/*.c")
file(GLOB_RECURSE HEADERS "src/*.h" "snes/*.h")
list(FILTER SOURCES EXCLUDE REGEX "src/platform/win32/.*")
list(FILTER SOURCES EXCLUDE REGEX "src/opengl\\.(c|h)")
list(FILTER SOURCES EXCLUDE REGEX "src/gfx_psp\\.(c|h)")

add_executable(${PROJECT_NAME} ${SOURCES} ${HEADERS})

target_include_directories(${PROJECT_NAME} PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/snes
    ${CMAKE_CURRENT_SOURCE_DIR}/src
    ${CMAKE_CURRENT_SOURCE_DIR}/third_party/opus
    ${PSPDEV}/psp/include/SDL2
)

message(STATUS "Opus headers: ${CMAKE_CURRENT_SOURCE_DIR}/third_party/opus")

target_link_libraries(${PROJECT_NAME} PRIVATE
    SDL2
    GL
    pspvram
    pspaudio
    pspvfpu
    pspdisplay
    pspgu
    pspge
    pspdebug
    psphprm
    pspctrl
    ${CMAKE_CURRENT_SOURCE_DIR}/third_party/opus/libopus.a
    ${CMAKE_CURRENT_SOURCE_DIR}/third_party/opus/libopusfile.a
)

if(PSP)
    create_pbp_file(
        TARGET ${PROJECT_NAME}
        ICON_PATH "${CMAKE_CURRENT_SOURCE_DIR}/src/platform/psp/icon0.png"
        BACKGROUND_PATH "${CMAKE_CURRENT_SOURCE_DIR}/src/platform/psp/pic0.png"
        PREVIEW_PATH NULL
        TITLE "The Legend of Zelda: A Link to the Past"
        BUILD_PRX
    )
endif()
