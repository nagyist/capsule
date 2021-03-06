cmake_minimum_required(VERSION 2.8)

if(APPLE)
  # MACOSX_RPATH is now enabled by default
  cmake_policy(SET CMP0042 NEW)
endif()

set(capsulerun_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src)
set(capsulerun_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/include)

set(capsulerun_INCLUDE_DIRS SYSTEM)

include_directories(${capsulerun_INCLUDE_DIRS} ${capsulerun_INCLUDE_DIR})

set(capsulerun_SRC
${capsulerun_SOURCE_DIR}/shared/main.cpp)

if(WIN32)
  list(APPEND capsulerun_SRC
    ${capsulerun_SOURCE_DIR}/windows/main.cpp
  )
endif()

if(APPLE)
  list(APPEND capsulerun_SRC
    ${capsulerun_SOURCE_DIR}/macos/main.cpp
  )
endif()

if(${CMAKE_SYSTEM_NAME} STREQUAL "Linux")
  list(APPEND capsulerun_SRC
    ${capsulerun_SOURCE_DIR}/linux/main.cpp
  )
endif()

add_executable(capsulerun ${capsulerun_SRC})

if(WIN32)
  add_dependencies(capsulerun capsule_deps)
  target_link_libraries(capsulerun ${DEVIARE_INPROC_LIBRARY})
endif()
