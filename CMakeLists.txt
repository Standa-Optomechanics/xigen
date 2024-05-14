CMAKE_MINIMUM_REQUIRED(VERSION 2.8.12)

PROJECT(xigen)

#set the right version here !

SET (XIGEN_VERSION_MAJOR 1)
SET (XIGEN_VERSION_MINOR 1)
SET (XIGEN_VERSION_PATCH 3)

# Pass version as macro to xigen compilation

ADD_DEFINITIONS(-DXIGEN_VERSION_MAJOR=${XIGEN_VERSION_MAJOR})
ADD_DEFINITIONS(-DXIGEN_VERSION_MINOR=${XIGEN_VERSION_MINOR})
ADD_DEFINITIONS(-DXIGEN_VERSION_PATCH=${XIGEN_VERSION_PATCH})


# Detect correct C++11 flag

IF(NOT ${CMAKE_SYSTEM_NAME} STREQUAL Windows)

    INCLUDE(CheckCXXCompilerFlag)

    CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)

    CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)

    IF(COMPILER_SUPPORTS_CXX11)

        SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

    ELSEIF(COMPILER_SUPPORTS_CXX0X)

        SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")

    ELSE()

        MESSAGE(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")

    ENDIF()

ENDIF()

IF(${CMAKE_SYSTEM_NAME} STREQUAL Windows AND ${CMAKE_SYSTEM_VERSION} EQUAL 6.1)

    # HACK: Make cryptopp replace missing (till Windows 8) GetOverlappedResultEx() on Windows7 with somehting compatible

    ADD_DEFINITIONS(-D_WIN32_WINNT=0x0601)

ENDIF()

IF(${CMAKE_SYSTEM_NAME} STREQUAL FreeBSD OR ${CMAKE_SYSTEM_NAME} STREQUAL Darwin)

    SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -stdlib=libc++")

ENDIF()


enable_testing()

add_subdirectory(src)
add_subdirectory(tests)
