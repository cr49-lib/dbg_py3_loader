# CMakeList.txt: dbg_py3_loader 的 CMake 项目，在此处包括源代码并定义
# 项目特定的逻辑。
#

# 将源代码添加到此项目的可执行文件。

# 包含./includes
include_directories(includes)

add_library(dbg_py3_loader SHARED "src/exports.cpp")


if (CMAKE_VERSION VERSION_GREATER 3.12)
  set_property(TARGET dbg_py3_loader PROPERTY CXX_STANDARD 20)
endif()

