# 混合编程标准工作流概述
1. **纯python的原型验证**
   * 使用`pure python`快速实现算法的端到端原型。这个阶段的目标是验证算法的有效性，而非性能。
2. **性能瓶颈分析**
   * 当原型验证通过后，使用Python的性能剖析工具（如`cProfile`, `line_profiler`, `PySpy`）运行代码，识别出**消耗时间最长的函数或代码块**。通常，这些瓶颈会出现在**大规模循环、复杂的数据变换或数值计算**中。
3. **瓶颈模块C++化**
   * **只重写性能瓶颈部分**。将分析出的热点函数或类，用C++重新实现。在实现时，应用相关的性能优化技术（多线程、SIMD等）。
4. **接口绑定**
   * 使用**Pybind11**为C++模块创建Python接口。Pybind11的强大之处在于它可以非常自然地处理STL容器、NumPy数组（通过`py::array_t`实现零拷贝或低开销转换）和自定义类。这使得C++函数可以像普通的Python函数一样被调用。
5. **python端集成与测试**
   * 在Python代码中，`import`编译好的C++扩展模块，替换掉原来缓慢的Python实现。
   * 利用Python强大的测试框架（如`pytest`）来验证C++模块的正确性，并与原Python实现的结果进行对比，确保数值一致。
# 基于uv的python项目构建
## 项目初始化
```bash
mkdir MyPyProject
cd MyPyProject
uv --init -name MyPyProject Name
```
### 自定义开发环境python版本要求
修改`.python-version`**版本锁定文件**：该文件限制了该开发环境中必须满足的python版本
### 自定义项目的最低支持python版本
修改`.pyproject.toml`配置文件：其中`requires-python`字段可以声明该项目的最低支持python版本
## 创建并激活虚拟环境
```bash
uv venv --python 3.*
source .venv/bin/activate
```
### 注：退出虚拟环境
```bash
deactivate
```
### 包安装，硬链接与缓存
#### 包安装
1. 作为依赖加入`pyproject.toml`中，并进行安装
```bash
uv add package
```
2. 兼容`pip api`，仅仅为该虚拟环境python解释器安装
```bash
uv pip install package
```
#### 硬链接与缓存
> 安装包时，`uv` 尝试使用一种最高效的方式（“硬链接”）来将包文件从中央缓存区“安装”到你的项目虚拟环境（`.venv`）中，但失败了。因此，它只能退而求其次，使用一种较慢且更占磁盘空间的方式（“完整复制”）

1. 两个关键概念
- **硬链接 (Hardlink)**：这是一种文件系统的高级功能。可以把它想象成给同一个文件起了多个名字。从 `uv` 的缓存目录创建一个到项目 `.venv` 目录的硬链接，**几乎不花费时间和磁盘空间，因为它没有真正复制文件数据，只是创建了一个指向已有数据的新指针**。这是 `uv` 实现极速安装和节省磁盘空间的关键技术之一。
- **完整复制 (Full Copy)**：这就是我们通常理解的“复制粘贴”。**它会读取源文件的所有数据，并在目标位置创建一个全新的、一模一样的文件副本。这个过程需要消耗与文件大小成正比的时间和磁盘空间**。
2. 为什么硬链接会失败
	**硬链接有一个核心限制：它不能跨越不同的文件系统或磁盘分区。**`uv` 的缓存目录（存放所有下载的包）和你的项目虚拟环境目录 (`.venv`) 必须在同一个文件系统上，才能成功创建硬链接。
3. 解决方案
    确保 `uv` **缓存和你的项目在同一个文件系统上**
    * way1: 将项目移动到主文件系统
    * way2: **将 `uv` 的缓存目录移动到项目所在的文件系统**
      ```bash
		export UV_CACHE_DIR=...
		```

# 基于CMake的C++子项目构建
## 核心工具链说明与安装
### 核心：编译器
> 唯一职责就是将人类可读的源代码（`.cpp`, `.h` 文件）翻译成机器可执行的二进制文件。

1. GCC和G++
	- **GCC (GNU Compiler Collection)**: 一个由 GNU 项目开发的编译器**集合**。它不仅包含 C++ 编译器，还包含 C (`gcc`), Fortran, Ada 等编译器。
	- **g++**: GCC 集合中专门用于编译 C++ 程序的**命令**。它是一个驱动程序（driver program），在后台会调用真正的编译器、汇编器和链接器。注意：在编译`C++`程序时，应该始终使用`g++`，因为其会自动链接`lstdc++`标准库和预定义`__cplusplus`宏。
2. **经典流程**
	预处理->编译->汇编->**链接**

### clang
> 由 LLVM 项目开发的 C/C++/Objective-C 编译器。它的设计目标是提供更快的编译速度、更好的错误信息和更现代的代码生成。
1. **clang**: 这是 LLVM 项目中的 C/C++ 编译器。
2. **clang++**: 这是 clang 的 C++ 编译器驱动程序
3. **clang-tidy**: 一个 C++ 代码分析工具，提供了许多现代 C++ 编程的最佳实践检查和自动修复功能。
4. **clang-format**: 一个代码格式化工具，可以自动将 C++ 代码格式化为一致的风格。它支持多种风格配置，可以通过 `.clang-format` 文件进行自定义

#### clang安装及设置
> 官方源支持的最高版本和默认版本可能不同，可以手动选择安装较高版本

```bash
sudo apt update && sudo apt upgrade
sudo apt install -y clang lldb
```
设置为默认版本
```bash
sudo update-alternatives --install /usr/bin/clang++ clang++ /usr/bin/clang++-{} 100
sudo update-alternatives --install /usr/bin/clang clang /usr/bin/clang-{} 100
```

### 自动化：构建系统
> **手动编译、链接的弊端**：
> 1. 哪些文件需要重新编译？（只重新编译修改过的文件及其依赖项）
> 2. 编译和链接的顺序是什么？
> 3. 编译和链接的选项是什么？（-O3, -Wall, -I/path/to/include, -L/path/to/lib, -l-some-lib）
> ==构建系统就是为自动化解决这些问题而生==, 它根据预先定义的依赖关系，智能地、增量地调用编译器（`g++`）来构建项目

**Ninja**: 一个现代、更快的构建系统。它被设计为“小而快”。Ninja 本身不做复杂的逻辑判断，它读取一种非常简单的 `.ninja` 文件格式。它的核心优势在于，当依赖关系图极其庞大时，其启动速度和增量构建的决策速度远快于**Make**。
你**几乎永远不会手写 `.ninja` 文件**。它们`是由更高层次的工具（如 CMake）`生成的
#### Ninja安装
```bash
sudo apt updata && sudo apt upgrade
sudo apt install ninja-build
```

### 元构建：构建系统生成器
> 单独存在的构建系统具有致命缺陷：
> 1. **平台不兼容**: 在 Windows 上，你可能需要用 MSVC 编译器，命令和库的名称都不同。你需要写一个 `NMake` 或 `MSBuild` 的文件。
> 2. **难以维护**: 手动维护大型项目中成百上千个文件的依赖关系是一场噩梦。找一个头文件的路径、一个库的位置，都需要手动修改 `Makefile`
> 3. **功能有限**: 寻找依赖库、配置编译选项等高级功能需要编写复杂的 Shell 脚本逻辑。
> ==CMake就是为了解决这些问题而生的“元构建系统”，==只需维护一份 `CMakeLists.txt`，就能在 Linux, macOS, Windows 等不同平台上，**使用不同的编译器和构建系统来构建你的项目**。这是**现代 C++ 项目跨平台和管理复杂性的基石**。

#### CMake核心概念
1. **CMake**: 它**不直接构建项目**。它的工作是 **生成** 特定平台下的构建系统文件（如 Linux 下的 `Makefile` 或 `build.ninja`，Windows 下的 Visual Studio `.sln` 文件）。
2. **CMakeLists.txt**: 你只需要编写一个高层、抽象、跨平台的 `CMakeLists.txt` 文件来**描述你的项目**：它叫什么名字、包含哪些源文件、需要哪些头文件目录、依赖哪些库。
#### CMake工作流程
1. **配置 (Configure)**:
    - **命令**: `$ cmake -S . -B build -G "Unix Makefiles"`
    - **过程**: `CMake` 解析根目录 (`-S .`) 下的 `CMakeLists.txt`。它会探测你的系统环境（是什么操作系统？`g++` 在哪？），执行 `find_package` 等命令寻找依赖库，然后根据你选择的生成器（`-G "Unix Makefiles"`），在 `build` 目录（`-B build`）中生成相应的构建文件（这里是 `Makefile`）。
    - **输出**: `build/Makefile`, `build/cmake_install.cmake` 等。
2. **构建 (Build)**:
    - **命令**: `$ cmake --build build` (这是一个跨平台的命令，它会自动调用底层的 `make` 或 `ninja`)
    - **过程**: 进入 `build` 目录，执行**原生构建命令**（如 `make`）。`make` 读取 `CMake` 生成的 `Makefile`，然后调用 `g++` 进行编译和链接。
#### CMake安装
```bash
sudo apt install cmake
```

### 开发态智能：语言服务器
#### LSP
> 在过去，每个编辑器（VSCode, Vim, Emacs）都需要自己写一个 C++ 解析器来实现诸如代码自动补全、跳转、查找应用、错误/警告提示、重构等功能，这非常困难且效果不一。

**LSP (Language Server Protocol)**: 由微软开创的一套开放协议。它将“编辑器功能”和“语言智能”解耦。
- **语言服务器 (Language Server)**: 一个独立的后台进程，它负责解析代码、提供补全、诊断等“智能”服务。`clangd` 就是一个针对 C/C++/Objective-C 的语言服务器。
- **编辑器客户端**: 编辑器（如 VSCode）**只需要实现 LSP 客户端**，就能通过标准化的 JSON-RPC 消息与任何语言服务器通信。
#### clangd
> **clangd**是基于 Clang (LLVM 项目的 C++ 前端) 构建的语言服务器。因为它与编译器共享同一套代码解析库 (`libclang`)，所以它对 C++ 语法的理解极其精确，诊断信息和补全建议的质量非常高。

##### clangd安装
```bash
sudo apt update
sudo apt install clangd
```
##### compile_commands.json
> 列出了项目中每个源文件所对应的**完整编译命令**。clangd通过该文件获取信息，才知道如何“编译”你当前正在编辑的文件，才能提供准确的上下文信息

如何生成？
在 `CMakeLists.txt` 中加入一行 `set(CMAKE_EXPORT_COMPILE_COMMANDS ON)`。当你运行 `cmake` 配置项目时，它就会在构建目录中自动生成 `compile_commands.json`。`clangd` 会自动在项目根目录或上层目录中寻找这个文件，从而获得构建项目所需的所有信息。

### 总结
**工具链 (Toolchain)** 是指用于构建软件的一整套工具的集合。对于 C++ 而言，一个典型的工具链（如 GCC Toolchain）通常包括：
- **编译器 (Compiler)**: `g++`
- **标准库 (Standard Library)**: `libstdc++` (头文件和链接库)
- **链接器 (Linker)**: `ld`
- **调试器 (Debugger)**: `gdb`
- **二进制工具集 (Binutils)**: `ar` (创建静态库), `nm` (列出符号), `objdump` (显示目标文件信息), `as` (汇编器) 等。
- 
不同的工具链提供商有：
- **GNU Toolchain**: `gcc`, `g++`, `gdb`, `libstdc++`, `GNU ld`
- **LLVM/Clang Toolchain**: `clang`, `clang++`, `lldb`, `libc++`, `lld` (链接器)
- **MSVC Toolchain**: `cl.exe`, Visual C++ Runtime, `link.exe`, Visual Studio Debugger

`CMake` 在配置项目时，一个重要任务就是探测和选择使用哪个工具链
## VSCode插件及配置
### 核心插件安装
1. **`CMake Tools`**: 项目构建和管理的核心
2. **`clangd`**: C++ 语言服务器
3. **`CodeLLDB`**: LLDB 调试器前端（可能需要**离线安装**，按照网络教程操作）
4. **`C/C++ Extension Pack`**: 微软官方的插件包，它也包含`CMake Tools`
### 配置`clangd`为唯一语言智能引擎
微软的 `C/C++` 插件自带了一个 IntelliSense 引擎，它会与 `clangd` 产生冲突，导致功能重复或行为异常。我们必须禁用它，让 `clangd` 全权接管。
- 按下 `Ctrl+Shift+P` 打开命令面板，输入 `Preferences: Open User Settings (JSON)`。
- 在打开的 `settings.json` 文件中，添加以下配置
	```json
	{
		// 完全禁用微软C/C++插件的IntelliSense引擎 
		"C_Cpp.intelliSenseEngine": "disabled", 
		// (可选但推荐) 指定 clangd 的路径，通常apt安装后会自动找到 
		// "clangd.path": "/usr/bin/clangd", 
		// 让 clangd 在后台分析时使用更多的线程，加快分析速度 
		"clangd.arguments": ["--compile-threads=4"] 
	}
	```

## C++项目的配置、构建与安装
### 编写C++源文件
> 在C++源文件夹下编写C++代码
### CMakePresets.json
> `CMakePresets.json`是 `CMake 3.19` 引入的一个新特性，用于简化 CMake 项目的配置和构建过程。它通过一个 JSON 文件，以声明式的方式定义了项目配置（Configure）和构建（Build）的预设集合（Presets），使得团队成员、CI/CD 系统和 IDE（如 Visual Studio Code）能够以统一、可预测的方式来操作 CMake 项目。

```json
{
    "version": 4,
    "cmakeMinimumRequired": {
        "major": 3,
        "minor": 18,
        "patch": 0},
    "configurePresets": [
        {
            "name": "default",
            "displayName": "Default Config",
            "description": "Default build configuration (requires the project's .venv to exist before configuring)",
            "generator": "Ninja",
            "binaryDir": "${sourceDir}/build",
            "environment": {
                "PATH": "${sourceDir}/.venv/bin:$penv{PATH}"
            },
            "cacheVariables": {
                "CMAKE_BUILD_TYPE": "Debug",
                "CMAKE_EXPORT_COMPILE_COMMANDS": "ON",
                "Python3_EXECUTABLE": "${sourceDir}/.venv/bin/python",
                "CMAKE_INSTALL_PREFIX": "${sourceDir}/install"
            }
        },
        {
            "name": "release",
            "displayName": "Release Config",
            "description": "Release build configuration",
            "inherits": "default",
            "cacheVariables": {
                "CMAKE_BUILD_TYPE": "Release"
            }
        },
        {
            "name": "rel-with-deb-info",
            "displayName": "Release with Symbols",
            "description": "Optimized build that keeps symbols (ideal for Python modules)",
            "inherits": "default",
            "cacheVariables": {
                "CMAKE_BUILD_TYPE": "RelWithDebInfo"
            }
        }
    ],
    "buildPresets": [
        {
            "name": "default",
            "configurePreset": "default"
        },
        {
            "name": "release",
            "configurePreset": "release"
        },
        {
            "name": "rel-with-deb-info",
            "configurePreset": "rel-with-deb-info"
        }
    ]
}
```
#### 顶层结构与元数据
1. **"version"**: 这是 `CMakePresets.json` 文件格式的版本号，版本 4 是一个相当现代且广泛支持的版本。不同的版本会引入新的字段和功能。
2. **"cmakeMinimumRequired"**: 这部分**并不直接强制**本地安装的 CMake 版本，而是作为一种元数据，**建议**使用者需要至少的 CMake 版本才能正确解析和使用此预设文件中的所有功能。
#### 配置预设
> **"configurePresets" 数组**定义了一系列**用于“配置”阶段**的参数组合。配置阶段是 CMake 工作流的第一步，它会解析 `CMakeLists.txt` 文件，侦测编译器、库和系统环境，并生成构建系统文件（例如，Ninja 的 `.ninja` 文件或 Makefile）。

##### 基础预设 **"default"**
1. **"name"**: 预设的机器可读**唯一标识符**。在命令行或通过其他预设的 `inherits` 字段引用时使用。
2. "displayName": 在 IDE（如 VSCode）中展示给用户的友好名称。
3. **"generator"**: 项目使用的构建系统
4. **"binaryDir"**: CMake 强烈推荐 **“源外构建”（Out-of-Source Build** 。这意味着所有由 CMake 生成的中间文件、缓存、目标文件（`.o`, `.obj`）和最终产物（可执行文件、库）都应存放在一个与源代码目录 (`sourceDir`) 分离的目录 (`binaryDir`) 中。这可以保持源代码树的干净。
	* 注：`${sourceDir}`是 CMake Presets 提供的一个宏，它会自动展开为包含 `CMakeLists.txt` 的根目录路径。
5. "environment": 此字段允许为 CMake 配置进程**设置临时的环境变量**。这比手动在 shell 中 `export` 变量更加可靠和可移植。
	* **"PATH"**: `"${sourceDir}/../.venv/bin:$penv{PATH}"` 修改了 `PATH` 环境变量，将 **Python 虚拟环境**的 `bin` 目录**添加到了`PATH`的最前面**,确保了当 CMake 及其子进程（如编译器、`find_package` 模块）查找可执行文件时，会**优先**在项目的虚拟环境中查找，从而找到正确的 `python` 解释器和其他通过 `pip` 安装的工具。
6. "cacheVariables": 是一个存储配置选项的文件, 一旦一个变量被设置并存入缓存，它在后续的配置运行中会保持其值，除非被显式覆盖。
	* **"CMAKE_BUILD_TYPE"**: 一个标准的 CMake 变量，**用于控制构建模式**，主要影响编译器的优化级别和调试信息的生成
	* **"CMAKE_EXPORT_COMPILE_COMMANDS"**: 当此选项开启时，CMake 会在构建目录中生成一个名为 `compile_commands.json` 的文件。该文件包含了项目中每个源文件所使用的确切编译命令。它对于现代 C++ 开发工具链至关重要。
	* **"Python3_EXECUTABLE"**: 在 `CMakeLists.txt` 中使用 `find_package` 来查找 Python 时，CMake 的 `FindPython3` 模块会去寻找 Python 解释器。通过此缓存变量，你**直接、显式地**告诉 CMake Python 解释器的确切位置。这避免了 CMake 在系统路径中进行猜测，从而保证了 100% 的准确性
	* **"CMAKE_INSTALL_PREFIX"**: "${sourceDir}/install": **显式指定**了手动安装时的目标目录，使其位于项目顶层的`install`文件夹，清晰明了
##### 派生预设 **"release" 和 "rel-with-deb-info"**
1. **"inherits"**: 继承是 `CMakePresets.json` 中实现配置复用的强大机制。一个预设可以通过 `inherits` 字段继承一个或多个（以数组形式）基础预设的所有配置。
2. **"cacheVariables"**: 当派生预设中定义了与基础预设相同的字段时，派生预设的值会**覆盖**基础预设的值。
	* **"release"**: 通常开启高级别优化 (`-O3`) 并且不包含调试信息，用于**最终的发布版**本。
	* **"rel-with-deb-info"**: 一个非常有用的折中方案：它会开启优化 (`-O2`)，同时也会生成调试信息 (`-g`)。`注意：生成动态库时，可执行文件需要被其他程序（如这里的python解释器）进行动态链接（符号解析与重定位），因此不能选择使用"release"模式。因此，我们通常选择"rel-with-deb-info"模式。
#### 构建预设
> `buildPresets` 数组定义了**用于“构建”阶段的参数组合**。构建阶段是 CMake 工作流的第二步，它会实际调用底层的构建工具（如 Ninja）来编译源代码并生成最终的目标文件。

1. **"name"**: 构建预设的唯一标识符。
2. **"configurePreset"**: 这个字段将一个构建预设**链接**到一个配置预设。这意味着当你选择执行某个**构建预设**时，它会自动使用**对应的配置预设**所定义的 `binaryDir` 和其他构建相关的设置。
### 顶层CMakeLists.txt
> 这个文件是整个项目的入口，它告诉CMake去处理子目录中的C++项目

```cmake
cmake_minimum_required(VERSION 3.19)
project(cs336_basics)
add_subdirectory(cpp_src)
```

**"add_subdirectory(cpp_src)"**: 将C++源码目录添加为一个子项目
### 子项目CMakeLists.txt
> 这个CMake文件负责构建我们的C++动态库

```cmake
# ----------------------------------
cmake_minimum_required(VERSION 3.19)
project(cpplib VERSION 0.1.0 LANGUAGES C CXX)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(MODULE_NAME "prime")

# ----------------------------------
include(FetchContent)
FetchContent_Declare(
    pybind11
    GIT_REPOSITORY https://github.com/pybind/pybind11.git
    GIT_TAG v2.12.1
)
FetchContent_MakeAvailable(pybind11)

# ----------------------------------
# * lookup the Python interpreter
find_package(Python3 COMPONENTS Interpreter Development REQUIRED)
message(STATUS "Using Python interpreter: ${Python3_EXECUTABLE}")
message(STATUS "Using Python include dirs: ${Python3_INCLUDE_DIRS}")
message(STATUS "Using Python libraries: ${Python3_LIBRARIES}")
# ----------------------------------
pybind11_add_module(${MODULE_NAME}
    SHARED prime.cpp
)
# ----------------------------------
install(TARGETS ${MODULE_NAME}
    LIBRARY
    DESTINATION ${MODULE_NAME}
)
# ----------------------------------
file(WRITE "${CMAKE_BINARY_DIR}/__init__.py" "from .${MODULE_NAME} import *\n")
install(FILES "${CMAKE_BINARY_DIR}/__init__.py"
        DESTINATION ${MODULE_NAME}
)
# ----------------------------------
find_program(STUBGEN_EXECUTABLE pybind11-stubgen)
message(STATUS "Found pybind11-stubgen: ${STUBGEN_EXECUTABLE}, configuring .pyi generation.")
install(CODE "
    message(STATUS \"Generating .pyi stub at install time...\")
    execute_process(
        COMMAND \"${Python3_EXECUTABLE}\" -m pybind11_stubgen -o \"\${CMAKE_INSTALL_PREFIX}\" ${MODULE_NAME}
        WORKING_DIRECTORY \"\${CMAKE_INSTALL_PREFIX}\"
        RESULT_VARIABLE return_code
    )
    if(NOT return_code EQUAL 0)
        message(FATAL_ERROR \"pybind11-stubgen failed during install with exit code: \${return_code}\")
    endif()
" VERBATIM)
```

#### 项目初始化与C++标准设置
1. **"set(CMAKE_CXX_STANDARD_REQUIRED ON)"**: 这是一个重要的强制性设置。如果编译器不支持所设定的版本，配置过程将直接失败并报错，而不是降级到旧标准。这保证了语言特性的一致性。
2. **"set(CMAKE_CXX_EXTENSIONS OFF)"**: 禁用编译器特定的扩展（例如，GCC 的 `GNU++17` 而非标准的 `C++17`）。这是一个良好的可移植性实践，确保代码在不同编译器（Clang, MSVC, GCC）上的行为一致。
#### 依赖管理
> `FetchContent` 是 CMake 3.11+ 提供的内置模块，用于在**配置时**（Configure Time）下载并集成外部项目
1. **"FetchContent_Declare(...)"**: 此命令仅**声明**了依赖项 `pybind11` 的元数据：从哪里获取（Git 仓库）以及获取哪个版本（`v2.12.1` 标签）。此时并不会发生下载。
2. **"FetchContent_MakeAvailable(pybind11)"**: 这是实际触发动作的命令。它会检查 `pybind11` 是否已被添加到项目中。如果没有，它将：
	* 在构建目录（`binaryDir`）下的 `_deps` 子目录中克隆指定的 Git 仓库
	* 调用 `add_subdirectory()` 将 `pybind11` 的 `CMakeLists.txt` 包含到您的主构建流程中。 这使得 `pybind11` 提供的 CMake 函数（如 `pybind11_add_module`）和目标（targets）在后续的脚本中立即可用。这种方式远优于 Git Submodules 或要求用户手动安装库的传统方法。
#### Python环境探测
> **find_package**: 这是 CMake 用来**查找已安装库和程序的标准机制**。它会搜索 `Find<PackageName>.cmake` 模块文件

1. **"find_package(Python3 ...)"**: 
    * COMPONENTS Interpreter Development: 这告诉 `find_package` 需要找到 Python 的两个部分：`Interpreter` (解释器本身) 和 `Development` (即头文件 `Python.h` 和链接库)。两者对于编译 `pybind11` 模块都是必需的
    * REQUIRED: 如果找不到满足条件的 Python 环境，配置将立即失败
#### 定义Python模块目标
1. **"pybind11_add_module(...)"**: 这是 `pybind11` 提供的核心 CMake 函数。它极大地简化了创建 Python 扩展模块的过程。
	* 它会创建一个名为 `${MODULE_NAME}` 的库目标。
	* `SHARED` 关键字指定了要构建一个共享库（在 Linux/macOS 上是 `.so`，Windows 上是 `.pyd`）。
	* 在幕后，它会自动处理许多复杂的细节，包括：链接 `Python3::Python` 目标、设置正确的编译器标志、处理不同平台下的文件扩展名等。
#### 安装规则
1. `install(TARGETS ${MODULE_NAME} ...)`: 定义了最核心的安装规则，即如何安装编译好的`.so`文件
2. `file(WRITE ...)` 与 `install(FILES ...)`: 这两步配合，创建了一个包含`from .${MODULE_NAME} import *`内容的`__init__.py`文件并安装它。这使得Python包的API对用户更友好
3. **`install(CODE "...")`**: 这是整个配置的点睛之笔。它采用“生成即安装”的模式，在**安装阶段**才执行`pybind11-stubgen`来生成`.pyi`文件
	* **`execute_process(...)`**: 在安装时安全地调用`python -m pybind11_stubgen`
	* **`WORKING_DIRECTORY "\${CMAKE_INSTALL_PREFIX}"`**: 通过设置正确的工作目录，巧妙地解决了Python模块的导入路径问题
	* **`-o "\${CMAKE_INSTALL_PREFIX}"`**: 指定了正确的输出根目录，让`pybind11-stubgen`能够生成结构正确的包
	* **错误检查**: 通过`RESULT_VARIABLE`和`if()`语句，确保了如果存根生成失败，整个安装过程会以明确的错误停止
# 工作流
## 两种并行的工作流
### Python中心的打包/集成工作流(pip)
1. 触发方式：在终端运行`pip install -e .`
2. 执行者：`pip` + `scikit-build-core`
3. 目的：
	* 作为 Python 开发者，快速将 C++ 扩展集成到当前的 Python 环境中
	* 在 CI/CD 流水线中，进行自动化的构建、测试和打包
	* 验证 `pyproject.toml` 配置是否正确，项目是否能被标准 Python 工具链消费
4. 产物位置：所有中间产物都严格限制在由`scikit-build-core`管理的临时目录如`_skbuild/`中
### C++中心的开发/调试工作流(CMake Tools)
1. 触发方式：在VSCode中执行构建
2. 执行者：VSCode的CMake Tools插件
3. 目的：
	* 作为 C++ 开发者，专注于C++代码的编写
	* 利用 IDE 的强大功能，如语法高亮、代码补全（得益于 `compile_commands.json`）、静态分析
	* 对C++代码进行单步调试（重要场景）
4. 产物位置：所有产物都位于由`CMakePresets.json`中`binaryDir`字段指定的目录（例如`build/`
## 配置
### .VSCode
#### setting.json
```json
{
    "cmake.sourceDirectory": "",
    "cmake.loggingLevel": "debug",
    "cmake.useCMakePresets": "always",
    "python.defaultInterpreterPath": "${workspaceFolder}/.venv/bin/python"
}
```

1. **"cmake.sourceDirectory"**: 告诉插件在哪里找到顶层`CMakeLists.txt`文件，而不是定义变量本身。`${CMAKE_SOURCE_DIR}` 是 CMake 的一个**内置变量**，它始终指向包含顶层 `CMakeLists.txt` 的目录。
2. "cmake.loggingLevel": 设置 CMake Tools 插件在 VSCode“输出(Output)”面板中打印日志的详细程度，`"debug"` 级别会输出所有详细的执行信息，这在初次配置项目或排查构建问题时非常有用。当项目稳定后，可以考虑将其改为 `"info"` 以减少干扰。
3. **"cmake.useCMakePresets": "always"**: 这是一个至关重要的设置。它强制 CMake Tools 插件**完全忽略** `settings.json` 中的其他 CMake 相关配置（如 `cmake.configureSettings`, `cmake.generator` 等），并**只使用** `CMakePresets.json` 和 `CMakeUserPresets.json` 作为唯一的配置来源。
4. **"python.defaultInterpreterPath"**: 它确保了在 VSCode 中执行任何 Python 相关操作时（如打开终端、运行/调试 Python 文件、使用 linter/formatter），VSCode 会自动使用项目专属的 `.venv` 虚拟环境
#### launch.json
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Debug C++ Module",
            "type": "python",
            "request": "launch",
            "program": "${workspaceFolder}/main.py",
            "console": "integratedTerminal",
            "justMyCode": false,
            "env": {
                "PYTHONPATH": "${workspaceFolder}/install" 
            }
        }
    ]
}
```

1. **"program"**: 将其填充为用于测试 C++ 模块的入口 Python 脚本，例如："${workspaceFolder}/my_app.py"
2. **"justMyCode": false"**: 制 Python 调试器是否只停在用户自己编写的代码中。默认值为 `true`。必须设置为 `false`，才能让调试器在从 Python 代码调用 C++ 函数时，顺利停在 `.cpp` 文件的断点上
3. **`"env": { "PYTHONPATH": "${workspaceFolder}/install" }`**: 为调试器启动的 Python 进程设置环境变量。这是支撑`C++中心工作流`的核心，它告诉Python解释器，除了标准的`site-packages`，还需要到`${workspaceFolder}/install`这个目录模块中寻找模块。这个路径正是我们通过 IDE 手动运行 `CMake: Install` 命令后，C++ 模块的安装位置。==这个设置意味着，我们可以在 IDE 中修改、编译、安装 C++ 代码，然后直接按 F5 调试，完全无需运行 `pip install -e .`。它完美地将 C++ 的交互式开发调试周期从 Python 的打包流程中解放出来==
### pyproject.toml
```toml
[project]
name = ""
version = "0.1.0"
dependencies = [
    "pybind11-stubgen>=2.5.5",
]

# -------------------- 核心配置 --------------------
[build-system]
# 告诉 pip，构建本项目需要 scikit-build-core 这个包
# * pybind11-stubgen是一个构建时依赖，它确保了pip/uv在**创建隔离的构建环境**时，
# * 会自动将pybind11-stubgen安装进去，使得后续的CMake安装脚本能成功调用它。
requires = [
    "scikit-build-core >= 0.7.0",
    "pybind11-stubgen",
]
# 告诉 pip，请调用 scikit_build_core.build 来执行构建
build-backend = "scikit_build_core.build"

[tool.scikit-build]
# 这是给 scikit-build-core 的配置
# 告诉它 CMake 的源码根目录在 'cpp' 文件夹下
cmake.source-dir = "cpp"
```

## 两种工作流如何无缝协作
> 考虑如下一个场景：使用C++编写一个动态链接库，并希望在Python中调用，期间需要对C++代码进行调试
### 启动C++工作流
1. 在VSCode中，CMake Tools会自动识别出顶层的 `CMakeLists.txt` 和 `CMakePresets.json`。
2. 选择`Configure`，如`[default]`,插件会运行`cmake --preset default`。此时，一个**名为 `build/` 的目录**在项目根目录被创建。**注意，这与 `pip` 使用的 `_skbuild/` 是完全不同的目录，两者互不干扰。**
3. 开始在cpp源文件中编写（新）代码
### 构建与调试
1. 完成代码编写后，点击`Build`，CMake Tools 插件会运行 `cmake --build build`。`.so` 文件现在生成在了 `build`目录下（根据我们的 `install` 规则，这只是构建时的位置）。
2. 当需要调试时，在`.vscode/`下创建一个 `launch.json` 文件来启动一个 Python 脚本，并让它调用 C++ 模块
3. 在`launch.json`中，需要告诉调试器去哪里找到刚刚编译好的`.so`文件，因为目前还没有`install`，它还在`build`目录中。在VSCode命令面板中选择`CMake Install`，这会执行 `cmake --install build`，将 `build/` 目录中的产物复制到 `install/` 中，形成一个可被 `PYTHONPATH` 使用的包。
4. 现在，可以**在cpp文件中打上断点**，启动调试。
### 切换到Python工作流
1. 在完成调试后，需要确认其在标准的Python打包流程下是否也能正常工作，并可以进行正常发布
2. 在终端中，执行`pip install -e .`
3. 此时，`scikit-build-core` 会在**隔离的 `_skbuild` 目录**中，重新完整地走一遍 `configure-build-install` 流程，然后将最终产物链接到 `.venv` 中。









