cmake_minimum_required(VERSION 3.10)
project(example_glfw_opengl2)



set(SOURCES
    main.cpp
    ${CMAKE_SOURCE_DIR}/imgui.cpp
    ${CMAKE_SOURCE_DIR}/imgui_demo.cpp
    ${CMAKE_SOURCE_DIR}/imgui_draw.cpp
    ${CMAKE_SOURCE_DIR}/imgui_tables.cpp
    ${CMAKE_SOURCE_DIR}/imgui_widgets.cpp
    ${CMAKE_SOURCE_DIR}/src/imgui_impl_glfw.cpp
    ${CMAKE_SOURCE_DIR}/src/imgui_impl_opengl2.cpp
)

add_executable(${PROJECT_NAME} MACOSX_BUNDLE ${SOURCES})

set_target_properties(${PROJECT_NAME} PROPERTIES
    CXX_STANDARD 11
    CXX_STANDARD_REQUIRED YES
    CXX_EXTENSIONS NO
)


target_include_directories(${PROJECT_NAME} PRIVATE 
        ${CMAKE_SOURCE_DIR}/src
    )

if(UNIX AND NOT APPLE)

    find_package(glfw3 REQUIRED)
    find_package(OpenGL REQUIRED)
    target_link_libraries(${PROJECT_NAME} PRIVATE glfw OpenGL::GL)
    target_compile_options(${PROJECT_NAME} PRIVATE -pthread)

elseif(APPLE)

    find_package(glfw3 REQUIRED)
    include_directories(${GLFW_INCLUDE_DIRS})
    link_libraries(${GLFW_LIBRARY_DIRS})

    find_package(OpenGL REQUIRED)
    include_directories(${OPENGL_INCLUDE_DIRS})

    find_package(GLEW REQUIRED)
    include_directories(${GLEW_INCLUDE_DIRS})
    find_library(COCOA_LIBRARY Cocoa)
    find_library(OpenGL_LIBRARY OpenGL)
    find_library(IOKIT_LIBRARY IOKit)
    find_library(COREVIDEO_LIBRARY CoreVideo)
    SET(EXTRA_LIBS ${COCOA_LIBRARY} ${OpenGL_LIBRARY} ${IOKIT_LIBRARY} ${COREVIDEO_LIBRARY})
    target_link_libraries(
        ${PROJECT_NAME}
        PRIVATE
        glfw
        ${OPENGL_LIBRARIES}
        ${EXTRA_LIBS}
    )

else()
    # Windows
    set(glfw3_DIR ${CMAKE_SOURCE_DIR}/External/GLfw3/lib/cmake/glfw3/)
    find_package(glfw3 REQUIRED )
    target_link_libraries(${PROJECT_NAME} PRIVATE glfw gdi32 opengl32 imm32 )
endif()



message("Build complete for ${CMAKE_SYSTEM_NAME}")
