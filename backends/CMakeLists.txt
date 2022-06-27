target_sources(imguitest
    PRIVATE
        ${CMAKE_CURRENT_LIST_DIR}/imgui_impl_glfw.cpp
        ${CMAKE_CURRENT_LIST_DIR}/imgui_impl_opengl3.cpp
    #PUBLIC
)