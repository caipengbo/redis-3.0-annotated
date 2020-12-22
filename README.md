# Redis3.0详细注释

为了便于使用Clion调试（阅读）源码，为Redis项目配置cmake。

由于阅读过程参考的书籍[《Redis 设计与实现》](http://redisbook.com/)，因此源码版本与本书保持一致，采用3.0版本。
等阅读完本版本代码后，将对最新的Redis版本进行阅读。

# 各源文件简介

| 文件                                                              | 作用                                                              |
|------------------------------------------------------------------|-------------------------------------------------------------------|
| ``adlist.c`` 、 ``adlist.h``                                      | 双端链表数据结构的实现。                                          |
| ``ae.c`` 、 ``ae.h`` 、 ``ae_epoll.c`` 、 ``ae_evport.c`` 、``ae_kqueue.c`` 、 ``ae_select.c``       | 事件处理器，以及各个具体实现。                                    |                                                                |
| ``anet.c`` 、 ``anet.h``                                          | Redis 的异步网络框架，内容主要为对 socket 库的包装。              |
| ``aof.c``                                                         | AOF 功能的实现。                                                  |
| ``asciilogo.h``                                                   | 保存了 Redis 的 ASCII LOGO 。                                     |
| ``bio.c`` 、 ``bio.h``                                            | Redis 的后台 I/O 程序，用于将 I/O 操作放到子线程里面执行，减少 I/O 操作对主线程的阻塞。|                                  |
| ``bitops.c``                                                      | 二进制位操作命令的实现文件。                                      |
| ``blocked.c``                                                     | 用于实现 BLPOP 命令和 WAIT 命令的阻塞效果。                       |
| ``cluster.c`` 、 ``cluster.h``                                    | Redis 的集群实现。                                                |
| ``config.c`` 、 ``config.h``                                      | Redis 的配置管理实现，负责读取并分析配置文件，然后根据这些配置修改 Redis 服务器的各个选项。|
| ``crc16.c`` 、 ``crc64.c`` 、 ``crc64.h``                         | 计算 CRC 校验和。                                                 |
| ``db.c``                                                          | 数据库实现。                                                      |
| ``debug.c``                                                       | 调试实现。                                                        |
| ``dict.c`` 、 ``dict.h``                                          | 字典数据结构的实现。                                              |
| ``endianconv.c`` 、 ``endianconv.h``                              | 二进制的大端、小端转换函数。                                      |
| ``fmacros.h``                                                     | 一些移植性方面的宏。                                              |
| ``help.h``                                                        | ``utils/generate-command-help.rb`` 程序自动生成的命令帮助信息。   |
| ``hyperloglog.c``                                                 | HyperLogLog 数据结构的实现。                                      |
| ``intset.c`` 、 ``intset.h``                                      | 整数集合数据结构的实现，用于优化 SET 类型。                       |
| ``lzf_c.c`` 、 ``lzf_d.c`` 、 ``lzf.h`` 、 ``lzfP.h``             | Redis 对字符串和 RDB 文件进行压缩时使用的 LZF 压缩算法的实现。    |
| ``Makefile`` 、 ``Makefile.dep``                                  | 构建文件。                                                        |
| ``memtest.c``                                                     | 内存测试。                                                        |
| ``mkreleasehdr.sh``                                               | 用于生成释出信息的脚本。                                          |
| ``multi.c``                                                       | Redis 的事务实现。                                                |
| ``networking.c``                                                  | Redis 的客户端网络操作库，用于实现命令请求接收、发送命令回复等工作，文件中的函数大多为 write 、 read 、 close 等函数的包装，以及各种协议的分析和构建函数。|
| ``notify.c``                                                      | Redis 的数据库通知实现。                                          |
| ``object.c``                                                      | Redis 的对象系统实现。                                            |
| ``pqsort.c`` 、 ``pqsort.h``                                      | 快速排序（QuickSort）算法的实现。                                 |
| ``pubsub.c``                                                      | 发布与订阅功能的实现。                                            |
| ``rand.c`` 、 ``rand.h``                                          | 伪随机数生成器。                                                  |
| ``rdb.c`` 、 ``rdb.h``                                            | RDB 持久化功能的实现。                                            |
| ``redisassert.h``                                                 | Redis 自建的断言系统。                                            |
| ``redis-benchmark.c``                                             | Redis 的性能测试程序。                                            |
| ``redis.c``                                                       | 负责服务器的启动、维护和关闭等事项。                              |
| ``redis-check-aof.c`` 、 ``redis-check-dump.c``                   | RDB 文件和 AOF 文件的合法性检查程序。                             |
| ``redis-cli.c``                                                   | Redis 客户端的实现。                                              |
| ``redis.h``                                                       | Redis 的主要头文件，记录了 Redis 中的大部分数据结构，包括服务器状态和客户端状态。|
| ``redis-trib.rb``                                                 | Redis 集群的管理程序。                                            |
| ``release.c`` 、 ``release.h``                                    | 记录和生成 Redis 的释出版本信息。                                 |
| ``replication.c``                                                 | 复制功能的实现。                                                  |
| ``rio.c`` 、 ``rio.h``                                            | Redis 对文件 I/O 函数的包装，在普通 I/O 函数的基础上增加了显式缓存、以及计算校验和等功能。|
| ``scripting.c``                                                   | 脚本功能的实现。                                                  |
| ``sds.c`` 、 ``sds.h``                                            | SDS 数据结构的实现，SDS 为 Redis 的默认字符串表示。               |
| ``sentinel.c``                                                    | Redis Sentinel 的实现。                                           |
| ``setproctitle.c``                                                | 进程环境设置函数。                                                |
| ``sha1.c`` 、 ``sha1.h``                                          | SHA1 校验和计算函数。                                             |
| ``slowlog.c`` 、 ``slowlog.h``                                    | 慢查询功能的实现。                                                |
| ``solarisfixes.h``                                                | 针对 Solaris 系统的补丁。                                         |
| ``sort.c``                                                        | SORT 命令的实现。                                                 |
| ``syncio.c``                                                      | 同步 I/O 操作。                                                   |
| ``testhelp.h``                                                    | 测试辅助宏。                                                      |
| ``t_hash.c`` 、 ``t_list.c`` 、 ``t_set.c`` 、 ``t_string.c`` 、 ``t_zset.c``  | 定义了 Redis 的各种数据类型，以及这些数据类型的命令。             |                                                                  |
| ``util.c`` 、 ``util.h``                                          | 各种辅助函数。                                                    |
| ``valgrind.sup``                                                  | valgrind 的suppression文件。                                      |
| ``version.h``                                                     | 记录了 Redis 的版本号。                                           |
| ``ziplist.c`` 、 ``ziplist.h``                                    | ZIPLIST 数据结构的实现，用于优化 LIST 类型。                      |
| ``zipmap.c`` 、 ``zipmap.h``                                      | ZIPMAP 数据结构的实现，在 Redis 2.6 以前用与优化 HASH 类型，  Redis 2.6 开始已经废弃。|
| ``zmalloc.c`` 、 ``zmalloc.h``                                    | 内存管理程序。                                                    |

# 配置cmake

## 1. deps/hiredis目录下新增`CMakeLists.txt`

```cmake
add_library(hiredis STATIC
        hiredis.c
        net.c
        dict.c
        net.c
        sds.c
        async.c
        )
```

## 2. deps/linenoise目录下新增`CMakeLists.txt`

```cmake
add_library(linenoise
        linenoise.c
        )
```

## 3. deps/lua目录下新增`CMakeLists.txt`

```cmake
set(LUA_SRC
        src/lauxlib.c
        src/liolib.c
        src/lopcodes.c
        src/lstate.c
        src/lobject.c
        src/print.c
        src/lmathlib.c
        src/loadlib.c
        src/lvm.c
        src/lfunc.c
        src/lstrlib.c
        src/lua.c
        src/linit.c
        src/lstring.c
        src/lundump.c
        src/luac.c
        src/ltable.c
        src/ldump.c
        src/loslib.c
        src/lgc.c
        src/lzio.c
        src/ldblib.c
        src/strbuf.c
        src/lmem.c
        src/lcode.c
        src/ltablib.c
        src/lua_struct.c
        src/lapi.c
        src/lbaselib.c
        src/lua_cmsgpack.c
        src/ldebug.c
        src/lparser.c
        src/lua_cjson.c
        src/llex.c
        src/ltm.c
        src/ldo.c
        )

add_library(lua STATIC ${LUA_SRC})
```

## 4. deps目录下新增`CMakeLists.txt`

```cmake
add_subdirectory(hiredis)
add_subdirectory(linenoise)
add_subdirectory(lua)
```

## 5. redis-3.0-annotated目录下新增`CMakeLists.txt5`

```cmake
cmake_minimum_required(VERSION 3.0 FATAL_ERROR)

project(redis-3.0-annotated-cmake-in-clion VERSION 3.0)

if (NOT CMAKE_BUILD_TYPE)
    message(STATUS "No build type defined; defaulting to 'Debug'")
    set(CMAKE_BUILD_TYPE "Debug" CACHE STRING
            "The type of build. Possible values are: Debug, Release, RelWithDebInfo and MinSizeRel.")
endif()

message(STATUS "Host is: ${CMAKE_HOST_SYSTEM}.  Build target is: ${CMAKE_SYSTEM}")
get_filename_component(REDIS_ROOT "${CMAKE_CURRENT_SOURCE_DIR}" ABSOLUTE)
message(STATUS "Project root directory is: ${REDIS_ROOT}")

# Just for debugging when handling a new platform.
if (false)
    message("C++ compiler supports these language features:")
    foreach(i ${CMAKE_CXX_COMPILE_FEATURES})
        message("  ${i}")
    endforeach()
endif()

message(STATUS "Generating release.h...")
execute_process(
        COMMAND sh -c ./mkreleasehdr.sh
        WORKING_DIRECTORY ${REDIS_ROOT}/src/
)
add_subdirectory(deps)

set(SRC_SERVER
        src/adlist.c src/ae.c src/anet.c src/dict.c src/redis.c src/sds.c src/zmalloc.c
        src/lzf_c.c src/lzf_d.c src/pqsort.c src/zipmap.c src/sha1.c src/ziplist.c src/release.c src/networking.c src/util.c src/object.c src/db.c src/replication.c src/rdb.c src/t_string.c src/t_list.c src/t_set.c src/t_zset.c src/t_hash.c src/config.c src/aof.c src/pubsub.c src/multi.c src/debug.c src/sort.c src/intset.c src/syncio.c src/cluster.c src/crc16.c src/endianconv.c src/slowlog.c src/scripting.c src/bio.c src/rio.c src/rand.c src/memtest.c src/crc64.c src/bitops.c src/sentinel.c src/notify.c src/setproctitle.c src/blocked.c src/hyperloglog.c
        )

set(SRC_CLI
        src/anet.c src/sds.c src/adlist.c src/redis-cli.c src/zmalloc.c src/release.c src/anet.c src/ae.c src/crc64.c
        )


if (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    # better not to work with jemalloc
endif()

add_executable(redis-server ${SRC_SERVER})
add_executable(redis-cli ${SRC_CLI})

set_property(TARGET redis-server PROPERTY C_STANDARD 99)
set_property(TARGET redis-server PROPERTY CXX_STANDARD 11)
set_property(TARGET redis-server PROPERTY CXX_STANDARD_REQUIRED ON)

set_property(TARGET redis-cli PROPERTY C_STANDARD 99)
set_property(TARGET redis-cli PROPERTY CXX_STANDARD 11)
set_property(TARGET redis-cli PROPERTY CXX_STANDARD_REQUIRED ON)


target_include_directories(redis-server
        PRIVATE ${REDIS_ROOT}/deps/hiredis
        PRIVATE ${REDIS_ROOT}/deps/linenoise
        # PRIVATE ${REDIS_ROOT}/deps/jemalloc
        PRIVATE ${REDIS_ROOT}/deps/lua/src
        )

target_include_directories(redis-cli
        PRIVATE ${REDIS_ROOT}/deps/hiredis
        PRIVATE ${REDIS_ROOT}/deps/linenoise
        # PRIVATE ${REDIS_ROOT}/deps/jemalloc
        PRIVATE ${REDIS_ROOT}/deps/lua/src
        )


target_link_libraries(redis-server
        PRIVATE pthread
        PRIVATE m
        PRIVATE lua
        PRIVATE linenoise
        PRIVATE hiredis
        )

target_link_libraries(redis-cli
        PRIVATE pthread
        PRIVATE m
        PRIVATE linenoise
        PRIVATE hiredis
        )

link_directories(deps/hiredis/ deps/linenoise/ diredeps/lua/src)
```

写好所有CmakeLists.txt便可以将项目导入CLion，导入时不要覆盖已有的CMakeLists.txt。在CLion中选择`redis-server`，选择运行/调试，即可成功运行。