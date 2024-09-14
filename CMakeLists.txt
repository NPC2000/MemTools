cmake_minimum_required(VERSION 3.29)
project(MemTools)

add_compile_options("$<$<C_COMPILER_ID:MSVC>:/utf-8>")
add_compile_options("$<$<CXX_COMPILER_ID:MSVC>:/utf-8>")

set(CMAKE_CXX_STANDARD 20)

include_directories(.)
include_directories(dmaLibs/include)

# DMA 工具编译
add_library(dmaMemTools SHARED
        DmaMemoryTools.cpp
        MemoryToolsBase.cpp
)
# 本机内存工具编译
add_library(directMemTools SHARED
        DirectMemoryTools.cpp
        MemoryToolsBase.cpp
)
# dump内存工具编译
add_library(dumpMemTools SHARED
        DumpMemoryTools.cpp
        MemoryToolsBase.cpp
)

# 测试代码
add_executable(test
        main.cpp
        DmaMemoryTools.cpp
        DumpMemoryTools.cpp
        DirectMemoryTools.cpp
        MemoryToolsBase.cpp
)

# dump
add_executable(dumpMem
        dump.cpp
        DirectMemoryTools.cpp
        MemoryToolsBase.cpp
)

# DMA 工具需要的依赖
target_link_libraries(dmaMemTools
        "${CMAKE_SOURCE_DIR}/dmaLibs/leechcore.lib"
        "${CMAKE_SOURCE_DIR}/dmaLibs/vmm.lib"
        ws2_32
)

# 测试代码依赖
target_link_libraries(test
        "${CMAKE_SOURCE_DIR}/dmaLibs/leechcore.lib"
        "${CMAKE_SOURCE_DIR}/dmaLibs/vmm.lib"
        ws2_32
)