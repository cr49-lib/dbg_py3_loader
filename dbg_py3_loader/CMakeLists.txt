# CMakeList.txt: dbg_py3_loader 的 CMake 项目，在此处包括源代码并定义
# 项目特定的逻辑。
#

# 将源代码添加到此项目的可执行文件。

# 添加Python3的开发套件
find_package(Python3 COMPONENTS Development)

# 包含Python3的头文件
include_directories(${Python3_INCLUDE_DIRS})

# 包含Python3的库
link_libraries(${Python3_LIBRARIES})


# 包含./includes
include_directories(includes)

# 包含x64dbg的lib文件
add_library(x64dbg INTERFACE)
target_include_directories(x64dbg INTERFACE "includes/pluginsdk")
target_link_directories(x64dbg INTERFACE "includes/pluginsdk")

# 查找src文件夹下所有的.cpp、.c和.hpp文件
file(GLOB SRC_FILES "src/*.cpp" "src/*.c" "src/*.hpp")

add_library(dbg_py3_loader SHARED ${SRC_FILES})
target_link_libraries(dbg_py3_loader PRIVATE x64dbg)

# 不同平台的后缀名
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    set_target_properties(dbg_py3_loader PROPERTIES SUFFIX ".dp64")
else()
    set_target_properties(dbg_py3_loader PROPERTIES SUFFIX ".dp32")
endif()

if (CMAKE_VERSION VERSION_GREATER 3.12)
  set_property(TARGET dbg_py3_loader PROPERTY CXX_STANDARD 20)
endif()