cmake_minimum_required(VERSION 3.0.0)
project(imguitest VERSION 0.1.0)

add_executable(imguitest "")

set(STATIC_LINK TRUE)
message(STATUS "STATIC_LINK: ${STATIC_LINK}")

include(CTest)
enable_testing()
set(CMAKE_CXX_FLAGS "-std=c++20 -Wall -Wextra -fdiagnostics-color=always")
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
add_definitions(-DCMAKE_EXPORT_COMPILE_COMMANDS=ON)

get_filename_component(Mingw_Path ${CMAKE_CXX_COMPILER} PATH)
set(Mingw_Path_Alt "C:/Users/cdmillers2/Downloads/VSCode/buildtools/msys2/mingw64/bin")

set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)

set_target_properties(imguitest PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_LIST_DIR}/bin)
target_link_options(imguitest PRIVATE -mwindows)

add_subdirectory("examples/example_glfw_opengl3")
add_subdirectory("backends")

include_directories(
    ${CMAKE_CURRENT_LIST_DIR}
    ${CMAKE_CURRENT_LIST_DIR}/backends
    C:/Users/cdmillers2/Downloads/VSCode/buildtools/msys2/mingw64/include/GLFW
    C:/Users/cdmillers2/Downloads/VSCode/buildtools/msys2/mingw64/include/GL
)

target_sources(imguitest
    PRIVATE
        ${CMAKE_CURRENT_LIST_DIR}/imgui.cpp
        ${CMAKE_CURRENT_LIST_DIR}/imgui_draw.cpp
        ${CMAKE_CURRENT_LIST_DIR}/imgui_tables.cpp
        ${CMAKE_CURRENT_LIST_DIR}/imgui_widgets.cpp
        ${CMAKE_CURRENT_LIST_DIR}/imgui_demo.cpp
    #PUBLIC
)

find_library(GLFW3_LIBRARY glfw3 HINTS
    C:/Users/cdmillers2/Downloads/VSCode/buildtools/msys2/mingw64/include/GLFW
    C:/Users/cdmillers2/Downloads/VSCode/buildtools/msys2/mingw64/lib
    C:/Users/cdmillers2/Downloads/VSCode/buildtools/msys2/mingw64/bin
)

find_package(OpenGL REQUIRED)

if(STATIC_LINK)
    set(GLFW3_LIBRARY_STATIC C:/Users/cdmillers2/Downloads/VSCode/buildtools/msys2/mingw64/lib/libglfw3.a)
    set(CMAKE_EXE_LINKER_FLAGS " -static")

    target_link_libraries(imguitest gcc stdc++ ${GLFW3_LIBRARY_STATIC} OpenGL::GL)
else()
    target_link_libraries(imguitest gcc stdc++ ${GLFW3_LIBRARY} OpenGL::GL)
    set(CMAKE_INSTALL_SYSTEM_RUNTIME_LIBS ${Mingw_Path}/libgcc_s_seh-1.dll ${Mingw_Path}/libstdc++-6.dll ${Mingw_Path}/libwinpthread-1.dll ${Mingw_Path_Alt}/glfw3.dll)

    add_custom_command(TARGET imguitest POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_INSTALL_SYSTEM_RUNTIME_LIBS} $<TARGET_FILE_DIR:imguitest>
        COMMAND_EXPAND_LISTS)
endif(STATIC_LINK)