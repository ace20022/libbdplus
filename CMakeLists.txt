cmake_minimum_required(VERSION 3.0)

project(libbdplus VERSION 0.1.2 LANGUAGES C)

#find_package(libpgp-error 1.2.7 REQUIRED NO_MODULE)

if(MSVC)
  #set(CMAKE_DEBUG_POSTFIX "d")
endif()

set(SRCS
  ${CMAKE_CURRENT_SOURCE_DIR}/cmake/libbdplus.def
  ${CMAKE_CURRENT_SOURCE_DIR}/cmake/bdplus-version.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cmake/headers_fixup.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cmake/config.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/file/configfile.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/file/dirs_win32.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/file/file_default.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdplus.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdplus_config.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdsvm/diff.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdsvm/dlx.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdsvm/event.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdsvm/interface.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdsvm/loader.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdsvm/segment.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdsvm/sha1.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdsvm/slot.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdsvm/trap.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdsvm/trap_helper.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/internal.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/util/logging.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/util/mutex.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/util/strutl.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/file/configfile.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/file/dirs.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/file/file.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/file/filesystem.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/file/file_default.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdplus.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdplus_config.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdplus_data.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdsvm/diff.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdsvm/dlx.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdsvm/dlx_internal.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdsvm/event.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdsvm/interface.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdsvm/loader.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdsvm/segment.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdsvm/sha1.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdsvm/slot.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdsvm/slot_data.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdsvm/trap.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/bdsvm/trap_helper.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/libbdplus/internal.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/util/attributes.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/util/logging.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/util/macro.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/util/mutex.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/util/strutl.h
)

ADD_LIBRARY(libbdplus SHARED ${SRCS})
SET_TARGET_PROPERTIES(libbdplus PROPERTIES LINK_FLAGS "/DEF:\"libbdplus.def\"")

target_compile_definitions(libbdplus
  PRIVATE
  HAVE_CONFIG_H
  PKGDATADIR=""
  _CRT_SECURE_NO_WARNINGS
  _CRT_NONSTDC_NO_WARNINGS
  fseeko=_fseeki64
  inline=__inline
)
  
target_include_directories(libbdplus
  PRIVATE
  $<BUILD_INTERFACE:.;cmake;src;src/util;${CMAKE_CURRENT_BINARY_DIR}>
  INTERFACE
  $<INSTALL_INTERFACE:include/libbdplus>
)

target_link_libraries(libbdplus PRIVATE "D:/Code/libgcrypt-1.7.6_cmake/build/Debug/libgcryptd.lib"
                                        "D:/Code/libpgp-error/build/Debug/libpgp-errord.lib")

set(TEST_SRCS
  ${CMAKE_CURRENT_SOURCE_DIR}/src/examples/bdplus_test.c
)

ADD_EXECUTABLE(bdplus_test ${TEST_SRCS})
target_compile_definitions(bdplus_test
  PRIVATE
  HAVE_CONFIG_H
  _CRT_SECURE_NO_WARNINGS
  _CRT_NONSTDC_NO_WARNINGS)
  
target_include_directories(bdplus_test
  PRIVATE
  $<BUILD_INTERFACE:.;cmake;src;${CMAKE_CURRENT_BINARY_DIR}>
  INTERFACE
)
add_dependencies(bdplus_test libbdplus)
target_link_libraries(bdplus_test PRIVATE libbdplus)

set(DUMP_SRCS
  ${CMAKE_CURRENT_SOURCE_DIR}/src/examples/convtab_dump.c
)

ADD_EXECUTABLE(convtab_dump ${DUMP_SRCS})
target_compile_definitions(convtab_dump
  PRIVATE
  HAVE_CONFIG_H
  _CRT_SECURE_NO_WARNINGS
  _CRT_NONSTDC_NO_WARNINGS)
  
target_include_directories(convtab_dump
  PRIVATE
  $<BUILD_INTERFACE:.;cmake;src;${CMAKE_CURRENT_BINARY_DIR}>
  INTERFACE
)
add_dependencies(convtab_dump libbdplus)
target_link_libraries(convtab_dump PRIVATE libbdplus)