target_include_directories(imguitest 
    PRIVATE
        # ${CMAKE_SOURCE_DIR}/backends
        ${CMAKE_SOURCE_DIR}
        C:/Users/cdmillers2/Downloads/VSCode/buildtools/msys2/mingw64/include
)
target_sources(imguitest
    PRIVATE
        ${CMAKE_CURRENT_LIST_DIR}/main.cpp
    #PUBLIC
)