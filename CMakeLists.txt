cmake_minimum_required(VERSION 3.19)

project("PythonBundle")

set(APP_NAME "PyApp")

set(Python3_USE_STATIC_LIBS TRUE)
find_package (Python3 3.11 EXACT COMPONENTS Development REQUIRED)
if(Python3_FOUND)
    message ("python3 found libs: ${Python3_LIBRARIES} incl: ${Python3_INCLUDE_DIRS}")
else()
    message (FATAL_ERROR "Cannot find Python3, make sure the development packages of Python are available on the system")
endif()

add_executable(app MACOSX_BUNDLE "src/main.cpp")
target_include_directories(app PUBLIC ${Python3_INCLUDE_DIRS})
target_link_options(app PUBLIC ${Python3_LINK_OPTIONS})
target_link_libraries(app PUBLIC ${Python3_LIBRARIES})
set_target_properties(app PROPERTIES OUTPUT_NAME "${APP_NAME}")

install(DIRECTORY ${CMAKE_SOURCE_DIR}/pythonbuild/ DESTINATION ${APP_NAME}.app/Contents/Resources/python
    USE_SOURCE_PERMISSIONS
    COMPONENT Runtime
    PATTERN exe_with_* EXCLUDE
)

install(TARGETS app
  BUNDLE DESTINATION . COMPONENT Runtime
  RUNTIME DESTINATION bin COMPONENT Runtime
)

install(CODE "
  include(InstallRequiredSystemLibraries)
  include(BundleUtilities)
  fixup_bundle(\"\${CMAKE_INSTALL_PREFIX}/${APP_NAME}.app\" \"\" \"\" IGNORE_ITEM \"Python;exe_with_z64;exe_with_zip\")"
  COMPONENT Runtime
)

set(CPACK_GENERATOR "DragNDrop")
include(CPack)
