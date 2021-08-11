cmake_minimum_required(VERSION 3.5)
# cmd to check version of CMake $cmake --version
project(hello)
enable_language(Fortran)

# `make install` copy built stuff to the install directory and point find_package() to that directory.
if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
    set(CMAKE_INSTALL_PREFIX ${PROJECT_BINARY_DIR}/package
        CACHE PATH "Select installation path" FORCE
        )
endif()
message(STATUS "Install prefix: ${CMAKE_INSTALL_PREFIX}")

# CMake understands most Fortran compilers including:
# GNU, Intel, Absoft, PGI, PathScale, and IBM (XL and VisualAge)
if (CMAKE_Fortran_COMPILER_ID MATCHES "GNU")
	set(dialect "-ffree-form -std=f2008 -fimplicit-none")
	set(bounds "-fbounds-check")
endif()

if (CMAKE_Fortran_COMPILER_ID MATCHES "Intel")
	set(dialect "-stand f08 -free -implicitnone")
	set(bounds "-check bounds")
endif()

if(CMAKE_Fortran_COMPILER_ID MATCHES "PGI")
	set(dialect "-Mfreeform -Mdclchk -Mstandard -Mallocatable=03")
	set(bounds "-C")
endif()
# CMAKE_Fortran_FLAGS -- used for all builds
# CMAKE_Fortran_FLAGS_DEBUG --  Used only for debugging build
# CMAKE_Fortran_FLAGS_RELEASE -- used only for releasing builds
set(CMAKE_Fortran_FLAGS_DEBUG "${CMAKE_Fortran_FLAGS_DEBUG} ${bounds}")
set(CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} ${dialect}")

#
file(GLOB_RECURSE srcs90 src/*.f90 src/*.h)
# CMake understands Fortran module dependencies and will compile the modules in the correct order.
add_executable(hello90 ${srcs90} )

file(GLOB_RECURSE srcs77 src/*.f)
add_executable(hello77 ${srcs77})

install(TARGETS hello90 hello77
    EXPORT hellotargets
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
    )
