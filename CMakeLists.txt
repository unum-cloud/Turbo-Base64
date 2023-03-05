cmake_minimum_required(VERSION 3.14 FATAL_ERROR)
project(tb64 LANGUAGES CXX C)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED YES)
set(CMAKE_CXX_EXTENSIONS NO)

# Make Release by default
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release)
endif()

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pedantic")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fno-exceptions")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fstrict-aliasing")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -O3 -DNDEBUG")

if (CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fmax-errors=1")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -march=native")
endif()

if(CMAKE_SYSTEM_NAME STREQUAL "Darwin")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17")
    set(CMAKE_OSX_DEPLOYMENT_TARGET "11" CACHE STRING "Minimum OS X deployment version")
    set(CMAKE_OSX_ARCHITECTURES "x86_64" "universal2" "arm64" CACHE STRING "Minimum OS X deployment version")
endif()

set(INCLUDE_FILES "turbob64c.c" "turbob64d.c" "turbob64v128.c")

if (NOT CMAKE_SYSTEM_NAME STREQUAL "Darwin")
  list(APPEND INCLUDE_FILES "turbob64v256.c")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -march=haswell -march=corei7-avx -mtune=corei7-avx -mno-aes -falign-loops")
endif()

if (${HAS_AVX512})
  list(APPEND INCLUDE_FILES "turbob64v512.c")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -march=skylake-avx512 -mavx512vl -falign-loops")
endif()

add_library(tb64 ${INCLUDE_FILES})
