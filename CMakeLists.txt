cmake_minimum_required(VERSION 3.14 FATAL_ERROR)
include(CheckCXXCompilerFlag)
project(tb64 LANGUAGES CXX C)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED YES)
set(CMAKE_CXX_EXTENSIONS NO)

# Make Release by default
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -pedantic")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fno-exceptions")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -falign-loops")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fstrict-aliasing")
set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -O3")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pedantic")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fno-exceptions")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -falign-loops")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fstrict-aliasing")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -O3")

if(CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fmax-errors=1 -fomit-frame-pointer -march=native")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fmax-errors=1 -fomit-frame-pointer -march=native")
endif()

if(CMAKE_SYSTEM_NAME STREQUAL "Darwin")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17")
  set(CMAKE_OSX_DEPLOYMENT_TARGET "11" CACHE STRING "Minimum OS X deployment version")
  set(CMAKE_OSX_ARCHITECTURES "x86_64" "universal2" "arm64" CACHE STRING "Minimum OS X deployment version")
endif()

if(${CMAKE_SYSTEM_PROCESSOR} MATCHES "aarch64")
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -march=armv8-a")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -march=armv8-a")
endif()

set(INCLUDE_FILES "turbob64c.c" "turbob64d.c" "turbob64v128.c")

CHECK_CXX_COMPILER_FLAG("-mavx" COMPILER_SUPPORTS_AVX)
if(NOT CMAKE_SYSTEM_NAME STREQUAL "Darwin" AND COMPILER_SUPPORT_AVX)
  list(APPEND INCLUDE_FILES "turbob64v256.c")
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -mssse3 -mavx -mavx2 -mno-aes")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -mssse3 -mavx -mavx2 -mno-aes")
endif()

CHECK_CXX_COMPILER_FLAG("-mavx512" COMPILER_SUPPORTS_AVX512)
if(COMPILER_SUPPORTS_AVX512)
  list(APPEND INCLUDE_FILES "turbob64v512.c")
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -march=skylake-avx512 -mavx512vl")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -march=skylake-avx512 -mavx512vl")
endif()

add_library(tb64 ${INCLUDE_FILES})
