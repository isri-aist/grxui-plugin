# @author Shin'ichiro Nakaoka

option(BUILD_EXT_GRXUI_PLUGIN "Building the GrxUI plugin" OFF)

if(NOT BUILD_EXT_GRXUI_PLUGIN)
  return()

elseif(BUILD_BOOST_PYTHON_MODULES)
  include_directories(BEFORE ${BOOST_PYTHON_MODULE_INCLUDE_DIRS})
  include_directories(${PYTHON_INCLUDE_DIR})
  set_property(DIRECTORY APPEND PROPERTY COMPILE_DEFINITIONS CNOID_USE_BOOST_PYTHON)

elseif(NOT BUILD_PYTHON_PLUGIN)
  message(FATAL_ERROR "The GrxUI plugin requires PythonPlugin or Boost.Python modules")
endif()

set(target CnoidGrxUIPlugin)

set(sources
  GrxUIPlugin.cpp
  GrxUIMenuView.cpp
  )

make_gettext_mofiles(${target} mofiles)
add_cnoid_plugin(${target} SHARED ${sources}  ${mofiles})
target_link_libraries(${target} CnoidPythonPlugin)
apply_common_setting_for_plugin(${target} "${headers}")

add_subdirectory(python)
