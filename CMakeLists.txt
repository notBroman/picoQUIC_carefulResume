cmake_minimum_required(VERSION 3.19)
project(careful_resume LANGUAGES C)

set(CMAKE_C_STANDARD 17)
set(CMAKE_C_STANDARD_REQUIRED ON)
#set(CMAKE_BUILD_TYPE DEBUG)

include(FetchContent)
FetchContent_Declare(picoquic
  GIT_REPOSITORY https://github.com/notBroman/picoquic.git
  GIT_TAG e86ef9f #release v1.14
)
FetchContent_GetProperties(picoquic)
if(NOT picoquic_POPULATED)
  set(PICOQUIC_FETCH_PTLS Y CACHE INTERNAL "fetch PTLS library")
endif()
FetchContent_MakeAvailable(picoquic)

include_directories(SYSTEM ${picoquic_SOURCE_DIR})

add_library(server src/pico_server.c)
add_library(client src/pico_client.c)

add_executable(careful.out src/main.c)

target_link_libraries(careful.out 
                      server
                      client)
