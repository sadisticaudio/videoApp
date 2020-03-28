cmake_minimum_required(VERSION 3.13)
project(videoApp C CXX)
set(CMAKE_CXX_STANDARD 14)

add_subdirectory(lib/glfw)

add_definitions(-DGL_SILENCE_DEPRECATION)

list(APPEND SOURCES
    src/main.cpp
)

if(APPLE)
    list(APPEND EXTRA_LIBS
        "-framework OpenGL"
    )

    configure_file(
        ${CMAKE_CURRENT_SOURCE_DIR}/assets/MacOSXBundleInfo.plist.in
        ${CMAKE_CURRENT_BINARY_DIR}/assets/MacOSXBundleInfo.plist
    )



elseif(WIN32)
    list(APPEND EXTRA_LIBS
        "-lglu32 -lopengl32"
    )
    set(CMAKE_EXE_LINKER_FLAGS "-std=gnu99 -static -static-libgcc -static-libstdc++ -mwindows")
else()
    list(APPEND EXTRA_LIBS
        "-lGL -lGLU -lX11"
    )
endif()

add_executable(videoApp src/main.cpp)
target_link_libraries(videoApp glfw ${EXTRA_LIBS})