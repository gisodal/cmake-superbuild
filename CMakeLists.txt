cmake_minimum_required(VERSION 3.0)

if (CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
    set(CMAKE_INSTALL_PREFIX "${CMAKE_CURRENT_LIST_DIR}")
endif()

include (ExternalProject)

ExternalProject_Add(test1
    SOURCE_DIR "${CMAKE_CURRENT_LIST_DIR}/test1"
    CMAKE_ARGS
        -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}
        -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
)

# to rebuild test1 once a file has changed
file(GLOB_RECURSE SOURCES "${CMAKE_CURRENT_LIST_DIR}/test1/include/*" "${CMAKE_CURRENT_LIST_DIR}/test1/src/*")
ExternalProject_Add_Step(test1 forceconfigure
    COMMAND ${CMAKE_COMMAND} -E echo "Force configure of test1"
    DEPENDS ${SOURCES}
    DEPENDERS configure
)

# test2 depends on test1
ExternalProject_Add(test2
    SOURCE_DIR "${CMAKE_CURRENT_LIST_DIR}/test2"
    DEPENDS test1
    CMAKE_ARGS
        -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}
        -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
)

# to rebuild test2 once a file has changed
file(GLOB_RECURSE SOURCES "${CMAKE_CURRENT_LIST_DIR}/test2/include/*" "${CMAKE_CURRENT_LIST_DIR}/test2/src/*")
ExternalProject_Add_Step(test2 forceconfigure
    COMMAND ${CMAKE_COMMAND} -E echo "Force configure of test2"
    DEPENDS ${SOURCES}
    DEPENDERS configure
)

