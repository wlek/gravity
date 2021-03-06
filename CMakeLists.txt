cmake_minimum_required(VERSION 3.0)

SET(COMPILER_DIR src/compiler/)
SET(RUNTIME_DIR src/runtime/)
SET(SHARED_DIR src/shared/)
SET(UTILS_DIR src/utils/)

SET(UNITTEST_SRC src/cli/unittest.c)
SET(GRAVITY_SRC src/cli/gravity.c)

AUX_SOURCE_DIRECTORY(${COMPILER_DIR} COMPILER_FILES)
AUX_SOURCE_DIRECTORY(${RUNTIME_DIR} RUNTIME_FILES)
AUX_SOURCE_DIRECTORY(${SHARED_DIR} SHARED_FILES)
AUX_SOURCE_DIRECTORY(${UTILS_DIR} UTILS_FILES)

include_directories(${COMPILER_DIR} ${RUNTIME_DIR} ${SHARED_DIR} ${UTILS_DIR})

SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O2 -std=c11")

SET(SRC_FILES ${COMPILER_FILES} ${RUNTIME_FILES} ${SHARED_FILES} ${UTILS_FILES})

ADD_LIBRARY(libgravity OBJECT ${SRC_FILES})

if(WIN32)
    LIST(APPEND LIBS "m" "Shlwapi")
elseif(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    LIST(APPEND LIBS "m")
elseif(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    LIST(APPEND LIBS "m" "rt")
endif()

ADD_EXECUTABLE(unittest ${UNITTEST_SRC} $<TARGET_OBJECTS:libgravity>)
TARGET_LINK_LIBRARIES(unittest ${LIBS})


ADD_EXECUTABLE(gravity ${GRAVITY_SRC} $<TARGET_OBJECTS:libgravity>)
TARGET_LINK_LIBRARIES(gravity ${LIBS})


