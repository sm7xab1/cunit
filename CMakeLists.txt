cmake_minimum_required(VERSION 3.15)

option(AUTOMATED "compile CUnit automated interface [default=yes]" ON)
option(BASIC "compile CUnit basic interface [default=yes]" ON)
option(CONSOLE "compile CUnit console interface [default=yes]" ON)

SET_PROPERTY(GLOBAL PROPERTY TARGET_SUPPORTS_SHARED_LIBS TRUE)

project(${LIB_NAME} "")

include(GNUInstallDirs)
message("include dir = ${CMAKE_INSTALL_INCLUDEDIR}")
message("bin dir = ${CMAKE_INSTALL_BINDIR}")
message("lib dir = ${CMAKE_INSTALL_LIBDIR}")
message("info dir = ${CMAKE_INSTALL_INFODIR}")
message("man dir = ${CMAKE_INSTALL_MANDIR}")
message("doc dir = ${CMAKE_INSTALL_DOCDIR}")
message("data root dir = ${CMAKE_INSTALL_DATAROOTDIR}")

set(LIB_NAME cunit)


add_library(${LIB_NAME} SHARED "")

target_sources(${LIB_NAME}
  PRIVATE
  CUnit/Sources/Framework/CUError.c
  CUnit/Sources/Framework/MyMem.c
  CUnit/Sources/Framework/TestDB.c
  CUnit/Sources/Framework/TestRun.c
  CUnit/Sources/Framework/Util.c
)

if (AUTOMATED)
 target_sources(${LIB_NAME}
   PRIVATE
   CUnit/Sources/Automated/Automated.c
 )
endif()

if (BASIC)
 target_sources(${LIB_NAME}
   PRIVATE
   CUnit/Sources/Basic/Basic.c
 )
endif()

if (CONSOLE)
 target_sources(${LIB_NAME}
   PRIVATE
   CUnit/Sources/Console/Console.c
 )
endif()

target_include_directories(${LIB_NAME}
  PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/CUnit/Headers>
  $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
 )

 set(HEADERS
   ${CMAKE_CURRENT_LIST_DIR}/CUnit/Headers/Automated.h
   ${CMAKE_CURRENT_LIST_DIR}/CUnit/Headers/Basic.h
   ${CMAKE_CURRENT_LIST_DIR}/CUnit/Headers/CUCurses.h
   ${CMAKE_CURRENT_LIST_DIR}/CUnit/Headers/CUError.h
   ${CMAKE_CURRENT_LIST_DIR}/CUnit/Headers/CUnit.h
   ${CMAKE_CURRENT_LIST_DIR}/CUnit/Headers/CUnit_intl.h
   ${CMAKE_CURRENT_LIST_DIR}/CUnit/Headers/Console.h
   ${CMAKE_CURRENT_LIST_DIR}/CUnit/Headers/MyMem.h
   ${CMAKE_CURRENT_LIST_DIR}/CUnit/Headers/TestDB.h
   ${CMAKE_CURRENT_LIST_DIR}/CUnit/Headers/TestRun.h
   ${CMAKE_CURRENT_LIST_DIR}/CUnit/Headers/Util.h
   ${CMAKE_CURRENT_LIST_DIR}/CUnit/Headers/wxWidget.h
   )

   set(XSL_DTD
     ${CMAKE_CURRENT_LIST_DIR}/Share/CUnit-List.dtd
     ${CMAKE_CURRENT_LIST_DIR}/Share/CUnit-List.xsl
     ${CMAKE_CURRENT_LIST_DIR}/Share/CUnit-Run.dtd
     ${CMAKE_CURRENT_LIST_DIR}/Share/CUnit-Run.xsl
     ${CMAKE_CURRENT_LIST_DIR}/Share/Memory-Dump.dtd
     ${CMAKE_CURRENT_LIST_DIR}/Share/Memory-Dump.xsl
     )

install(TARGETS ${LIB_NAME} EXPORT ${LIB_NAME} DESTINATION ${CMAKE_INSTALL_LIBDIR})
install(EXPORT ${LIB_NAME} DESTINATION ${CMAKE_INSTALL_LIBDIR})
install(FILES ${HEADERS} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/CUnit)
install(FILES ${XSL_DTD} DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/CUnit)
install(FILES ${CMAKE_CURRENT_LIST_DIR}/cunit-config.cmake DESTINATION ${CMAKE_INSTALL_LIBDIR})
#install(EXPORT ${CMAKE_CURRENT_LIST_DIR}/cunit-config.cmake DESTINATION ${CMAKE_INSTALL_LIBDIR} FILE cunit-config.cmake)
install(EXPORT ${LIB_NAME} DESTINATION ${CMAKE_INSTALL_LIBDIR} FILE cunit-config.cmake)

#  ./include/CUnit
# ./include/CUnit/CUnit_intl.h
# ./include/CUnit/CUnit.h
# find: ‘./local/bin/AppProtection/.config’: Permission denied
# ./share/doc-base/libcunit1-doc.libcunit1
# ./share/CUnit
# ./share/CUnit/CUnit-List.xsl
# ./share/CUnit/CUnit-List.dtd
# ./share/CUnit/CUnit-Run.dtd
# ./share/CUnit/CUnit-Run.xsl
# ./share/doc/libcunit1-doc
# ./share/doc/libcunit1-doc/html/CUnit_doc.css
# ./share/doc/libcunit1-doc/html/headers/CUnit_intl.h
# ./share/doc/libcunit1-doc/html/headers/CUnit.h.gz
# ./share/doc/libcunit1-doc/examples/Demo_fprintf/CUnitExample.c
# ./share/doc/libcunit1
# ./share/doc/libcunit1-dev
# ./share/man/man3/CUnit.3.gz
# ./lib/x86_64-linux-gnu/libcunit.so.1.0.1
# ./lib/x86_64-linux-gnu/libcunit.so
# ./lib/x86_64-linux-gnu/libcunit.a
# ./lib/x86_64-linux-gnu/libcunit.so.1
# ./lib/x86_64-linux-gnu/pkgconfig/cunit.pc
