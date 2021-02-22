cmake_minimum_required(VERSION 3.19)

project("My Project")

set(APP_NAME "My App")

add_executable(app MACOSX_BUNDLE "src/main.cpp")

set_target_properties(app PROPERTIES OUTPUT_NAME "${APP_NAME}")

install(TARGETS app
  BUNDLE DESTINATION . COMPONENT Runtime
  RUNTIME DESTINATION bin COMPONENT Runtime
  )

include(CPack)
