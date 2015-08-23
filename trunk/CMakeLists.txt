cmake_minimum_required(VERSION 2.8)
project(ZThread)

include (CheckIncludeFiles)
include (CheckLibraryExists)
include (CheckFunctionExists)

SET(VERSION "2.3.2")
SET(PTHREAD_LIBNAME "pthread")

if (APPLE)
	SET(CMAKE_FIND_FRAMEWORK "FIRST")
endif()

CHECK_INCLUDE_FILES ("bits/atomicity.h" HAVE_ATOMIC_GCC)

CHECK_INCLUDE_FILES ("asm/atomic.h" HAVE_ATOMIC_LINUX)

if(WIN32)
	#CHECK_PROTOTYPE_EXISTS(_beginthreadex "windows.h" HAVE_BEGINTHREADEX)
endif()

CHECK_INCLUDE_FILES("dlfcn.h" HAVE_DLFCN_H)

CHECK_INCLUDE_FILES("errno.h" HAVE_ERRNO_H)

CHECK_INCLUDE_FILES("inttypes.h" HAVE_INTTYPES_H)

CHECK_INCLUDE_FILES("memory.h" HAVE_MEMORY_H)

if(UNIX AND NOT APPLE)
	CHECK_LIBRARY_EXISTS(${PTHREAD_LIBNAME} "pthread_create" "" HAVE_POSIX_THREADS)
	CHECK_LIBRARY_EXISTS(${PTHREAD_LIBNAME} "pthread_keycreate" "" HAVE_PTHREADKEYCREATE)
	CHECK_LIBRARY_EXISTS(${PTHREAD_LIBNAME} "pthread_yield" "" HAVE_PTHREAD_YIELD)
elseif(APPLE)
	SET(HAVE_POSIX_THREADS 1) #Mac OS X has pthreads integrated into it's system librarys
endif()
#/* Defined if -lrt is needed for RT scheduling */
#/* #undef HAVE_SCHED_RT */

#/* Defined if sched_yield() is available */
#define HAVE_SCHED_YIELD 

CHECK_INCLUDE_FILES ("stdint.h" HAVE_STDINT_H)

CHECK_INCLUDE_FILES ("stdlib.h" HAVE_STDLIB_H)

CHECK_INCLUDE_FILES("strings.h" HAVE_STRINGS_H)

CHECK_INCLUDE_FILES("string.h" HAVE_STRING_H)

CHECK_INCLUDE_FILES("sys/stat.h" HAVE_SYS_STAT_H)

CHECK_INCLUDE_FILES("sys/types.h" HAVE_SYS_TYPES_H)

CHECK_INCLUDE_FILES ("unistd.h" HAVE_UNISTD_H)



CONFIGURE_FILE("src/config.cmake.h.in" "src/config.h")

ADD_SUBDIRECTORY("src/")

