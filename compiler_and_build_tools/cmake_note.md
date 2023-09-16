# cmake 使用指南

本文将介绍以下内容：

- cmake 入门示例；
- 常用命令
- 高级用法
- 常用模板

## CMake 入门示例

以下是一个简易示例项目的目录结构，以及一个对应的CMakeLists.txt文件的示例，用于展示如何配置多级目录的CMake项目。

假设您的项目目录结构如下：

```cmake
my_project/
    CMakeLists.txt
    demo/
        CMakeLists.txt
        main.cpp
    include/
        my_class.h
    src/
        CMakeLists.txt
        my_class.cpp
```

接下来，您可以为每个子目录编写对应的CMakeLists.txt文件。

1. 在my_project/目录下的CMakeLists.txt文件：

```cmake
cmake_minimum_required(VERSION 3.12)
project(my_project)

# 添加子目录
add_subdirectory(demo)
add_subdirectory(src)
```

2. 在my_project/demo/目录下的CMakeLists.txt文件：

```cmake
# 声明当前目录下的源文件
file(GLOB SOURCES "*.cpp")

# 创建可执行文件
add_executable(my_project ${SOURCES})

# 包含头文件目录
target_include_directories(my_project PRIVATE ${CMAKE_SOURCE_DIR}/include)

# 链接库
target_link_libraries(my_project my_lib)
```

3. 在my_project/src/目录下的CMakeLists.txt文件：

```cmake
# 声明当前目录下的源文件
file(GLOB SOURCES "*.cpp")

# 创建库
add_library(my_lib ${SOURCES})

# 包含头文件目录
target_include_directories(my_lib PUBLIC ${CMAKE_SOURCE_DIR}/include)

# 如果有其他依赖，可以在这里添加
# target_link_libraries(my_lib other_lib)
```

这样，您的多级目录CMake项目就配置好了。通过这个示例，CMake将会在构建时自动处理每个子目录，构建可执行文件my_project，并将my_lib链接到它。

请注意，这只是一个示例，您可以根据您的项目结构和需求来进行修改和扩展CMakeLists.txt文件。此外，还要确保您的源文件和头文件正确放置在各自的目录中。

## 常用命令

### CMake 版本

### 编译选项

### 包含头文件目录

### 生成可执行文件

### 包含库文件目录

### 链接库

## 常用预定义变量

CMake中有许多预定义变量，其中一些较为常用的包括：

1. **CMAKE_SOURCE_DIR**：项目的根目录。

2. **CMAKE_BINARY_DIR**：编译目录（生成的可执行文件和中间文件的存放目录）。

3. **CMAKE_CURRENT_SOURCE_DIR**：当前处理的CMakeLists.txt文件所在的目录。

4. **CMAKE_CURRENT_BINARY_DIR**：当前处理的CMakeLists.txt文件生成的中间文件存放目录。

5. **CMAKE_INSTALL_PREFIX**：安装目录的前缀，默认为 `/usr/local`（可通过 `-DCMAKE_INSTALL_PREFIX` 选项进行更改）。

6. **CMAKE_C_COMPILER** 和 **CMAKE_CXX_COMPILER**：C和C++编译器的路径。

7. **CMAKE_C_FLAGS** 和 **CMAKE_CXX_FLAGS**：C和C++编译器的标志。

8. **CMAKE_BUILD_TYPE**：构建类型，通常是Debug或Release。

9. **CMAKE_MODULE_PATH**：CMake模块文件的搜索路径，用于寻找额外的模块。

10. **CMAKE_SYSTEM_NAME** 和 **CMAKE_SYSTEM_PROCESSOR**：目标系统的名称和处理器。

11. **CMAKE_PREFIX_PATH**：查找依赖包的根目录。

12. **CMAKE_LIBRARY_OUTPUT_DIRECTORY** 和 **CMAKE_RUNTIME_OUTPUT_DIRECTORY**：设置库和可执行文件的输出目录。

13. **CMAKE_PROJECT_NAME**：当前项目的名称。

14. **CMAKE_PROJECT_VERSION**：当前项目的版本号。

15. **CMAKE_CURRENT_LIST_DIR**：当前CMakeLists.txt文件所在的目录。

这些是一些常用的预定义变量，CMake还提供了许多其他变量，用于控制构建过程、查找依赖项、设置编译器和生成文件等。这些变量在CMake配置和定制中非常有用，可以根据项目的需求使用它们。您可以在CMake官方文档中找到完整的预定义变量列表以及它们的详细说明。

## 高级用法

### 查找库

### 生成静态库

### 生成动态库

### .cmake文件

在 CMake 中，可以使用 `.cmake` 文件来存储 CMake 变量、函数、宏或其他 CMake 相关的配置。这些文件可以包含一系列 CMake 命令，以便在不同项目或不同 CMakeLists.txt 文件中重复使用相同的配置或函数。

以下是一些关于 `.cmake` 文件的基本信息：

1. **文件扩展名**：CMake 配置文件的扩展名通常是 `.cmake`，例如 `myconfig.cmake`。

2. **使用方式**：你可以在 CMakeLists.txt 文件中使用 `include()` 命令来导入一个 `.cmake` 文件，从而将其中定义的变量、函数、宏引入到你的 CMake 构建系统中。例如：

   ```cmake
   include(myconfig.cmake)
   ```

3. **内容**：一个 `.cmake` 文件可以包含任何合法的 CMake 命令、变量设置、函数或宏定义。通常，这些文件用于定义项目范围的全局配置，以便多个 CMakeLists.txt 文件可以共享相同的配置。示例内容如下：

   ```cmake
   # 定义一个全局变量
   set(MY_GLOBAL_VARIABLE "Hello from myconfig.cmake")

   # 定义一个函数
   function(my_function)
     message("This is my function")
   endfunction()

   # 设置编译器选项
   set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
   ```

4. **位置**：通常，`.cmake` 文件可以放置在项目根目录或一个特定的子目录中，具体取决于你的组织结构和项目需求。

5. **导入顺序**：请注意，导入 `.cmake` 文件的顺序很重要，因为后导入的文件可以覆盖先导入的文件中的配置。因此，确保按照适当的顺序导入这些文件。

使用 `.cmake` 文件可以更好地组织和管理 CMake 配置，使其更具可维护性，特别是在大型项目中。但是请注意，滥用全局配置文件可能会导致不可预测的行为，因此建议谨慎使用。
