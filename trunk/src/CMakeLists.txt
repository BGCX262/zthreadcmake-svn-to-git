SET(ZHREAD_SOURCE_FILES AtomicCount.cxx ConcurrentExecutor.cxx Condition.cxx CountingSemaphore.cxx FastMutex.cxx FastRecursiveMutex.cxx Monitor.cxx Mutex.cxx PoolExecutor.cxx PriorityCondition.cxx PriorityInheritanceMutex.cxx PriorityMutex.cxx PrioritySemaphore.cxx RecursiveMutex.cxx RecursiveMutexImpl.cxx Semaphore.cxx SynchronousExecutor.cxx Thread.cxx ThreadImpl.cxx ThreadLocalImpl.cxx ThreadOps.cxx ThreadQueue.cxx ThreadedExecutor.cxx Time.cxx)

include_directories(${ZThread_SOURCE_DIR}/include)

if(UNIX AND NOT APPLE)
	MESSAGE(STATUS "Compilig ZThread for POSIX")
	ADD_DEFINITIONS(-DZT_POSIX)
elseif(APPLE)
	MESSAGE(STATUS "Compilig ZThread for Mac")
	ADD_DEFINITIONS(-DZT_MACOS)
elseif(WIN32)
	MESSAGE(STATUS "Compilig ZThread for WinDOS")
	ADD_DEFINITIONS(-DZT_WIN32)
endif()

add_library(ZThread ${ZHREAD_SOURCE_FILES})

#Setup all the additional dependencies

set(ZTHREAD_LIBRARIES "")
if(UNIX AND NOT APPLE)
	list(APPEND ZTHREAD_LIBRARIES "pthread" "rt")
elseif(APPLE)
	FIND_LIBRARY(CORESERVICES_LIB NAMES CoreServices)
	list(APPEND ZTHREAD_LIBRARIES ${CORESERVICES_LIB})
endif()
list(APPEND ZTHREAD_LIBRARIES "ZThread")
SET(ZTHREAD_LIBRARIES ${ZTHREAD_LIBRARIES} CACHE INTERNAL "" )