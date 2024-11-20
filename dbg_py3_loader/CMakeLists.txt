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

#add_library(dbg_py3_loader SHARED "src/exports.cpp")

include("cmake/msvc-static-runtime.cmake")
include("cmake/msvc-configurations.cmake")

message(STATUS "Fetching x64dbg...")
FetchContent_Declare(x64dbg
	URL
		"https://downloads.sourceforge.net/project/x64dbg/snapshots/snapshot_2023-06-10_18-05.zip"
	URL_HASH
		SHA1=04468bd61fb36d6b10d17f342f03ef12f5b2ce62
)
FetchContent_MakeAvailable(x64dbg)


include("cmake/x64dbg.cmake")

set(PluginTemplate_SOURCES
	"src/plugin.cpp"
	"src/pluginmain.cpp"
	"src/plugin.h"
	"src/pluginmain.h"
	cmake.toml
)

x64dbg_plugin(PluginTemplate)

target_sources(PluginTemplate PRIVATE ${PluginTemplate_SOURCES})
source_group(TREE ${CMAKE_CURRENT_SOURCE_DIR} FILES ${PluginTemplate_SOURCES})


if (CMAKE_VERSION VERSION_GREATER 3.12)
  set_property(TARGET dbg_py3_loader PROPERTY CXX_STANDARD 20)
endif()

