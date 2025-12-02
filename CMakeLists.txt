cmake_minimum_required(VERSION 3.28)
set(CMAKE_C_COMPILER "${CMAKE_SOURCE_DIR}/extra/mingw64/bin/gcc.exe" CACHE STRING "C compiler" FORCE)
set(CMAKE_CXX_COMPILER "${CMAKE_SOURCE_DIR}/extra/mingw64/bin/g++.exe" CACHE STRING "C++ compiler" FORCE)
project(SFML_Test LANGUAGES CXX)

include(FetchContent)
FetchContent_Declare(SFML
    GIT_REPOSITORY https://github.com/SFML/SFML.git
    GIT_TAG 3.0.2
    GIT_SHALLOW ON
    EXCLUDE_FROM_ALL
    SYSTEM)
FetchContent_MakeAvailable(SFML)

add_executable(main src/main.cpp)
target_compile_features(main PRIVATE cxx_std_17)
target_link_libraries(main PRIVATE SFML::Graphics SFML::Audio)
target_compile_options(main PRIVATE -Wall -Wextra)
target_link_options(main PRIVATE -static-libgcc -static-libstdc++)

set_target_properties(main PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/out
)

# Copy SFML DLLs to output directory
# add_custom_command(TARGET main POST_BUILD
#     COMMAND ${CMAKE_COMMAND} -E copy_if_different
#         $<TARGET_FILE:SFML::Graphics>
#         $<TARGET_FILE:SFML::Audio>
#         $<TARGET_FILE:SFML::Window>
#         $<TARGET_FILE:SFML::System>
#         ${CMAKE_SOURCE_DIR}/out
# )