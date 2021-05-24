cmake_minimum_required(VERSION 3.7)

file(READ "version.txt" FILE_VER)
file(READ "revision.txt" FILE_REV)

# менять верисю пакета тут
project(memcached_functions_mysql VERSION ${FILE_VER} LANGUAGES C)

option(HAVE_MY_BOOL "mysql has my_bool type" OFF)
if (HAVE_MY_BOOL)
  add_definitions(-DHAVE_TYPE_MY_BOOL)
endif()

set(sources
	src/add.c
	src/append.c
	src/decrement.c
	src/delete.c
	src/get.c
	src/get_cas.c
	src/increment.c
	src/mget.c
	src/prefix.c
	src/prepend.c
	src/replace.c
	src/servers.c
	src/set.c
	src/stats.c
	src/util.c
	src/version.c
)

include_directories("/usr/include/mysql")

set(CMAKE_POSITION_INDEPENDENT_CODE ON)

add_library(memcached_functions_mysql SHARED ${sources})

option(STATIC_STDCPP "static linking of libstdc++" ON)

if (STATIC_STDCPP)
    target_link_libraries(memcached_functions_mysql LINK_PRIVATE -static-libgcc -static-libstdc++ -l:libstdc++.a memcached)
else()
    target_link_libraries(memcached_functions_mysql LINK_PRIVATE memcached)
endif()
