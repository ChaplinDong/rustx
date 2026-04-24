# 04 新手常见问题排查（入门避坑，详细版）
整理 Rust 入门阶段（环境搭建、Cargo 使用、代码编写、程序运行）最常遇到的问题，涵盖所有新手高频报错，每个问题都详细说明「报错现象、报错原因、解决方案、注意事项」，新手遇到报错时，可按“报错关键词”快速查找对应解决方案，无需浪费时间搜索。

## 一、环境搭建相关问题（01-install-rust.md 对应）
### 问题1：安装失败，提示“linker 'cc' not found”（Linux/macOS 专属）
#### 报错现象
执行 Rust 安装命令（curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh）后，终端提示“linker 'cc' not found”，安装中断。
#### 报错原因
系统缺少编译依赖（cc 是 C 语言编译器，Rust 编译过程中需要用到），Linux/macOS 系统默认可能未安装该依赖。
#### 解决方案（分系统操作）
1.  Ubuntu/Debian 系列（如 Ubuntu 20.04、Ubuntu 22.04）：
    打开终端，输入以下命令，安装依赖：
    ```bash
    sudo apt update && sudo apt install build-essential
    ```
    安装完成后，重新执行 Rust 安装命令即可。
2.  CentOS/Fedora 系列（如 CentOS 8、Fedora 38）：
    打开终端，输入以下命令，安装依赖：
    ```bash
    sudo dnf install gcc
    ```
    安装完成后，重新执行 Rust 安装命令即可。
3.  macOS 系统：
    终端会提示“缺少 Xcode 命令行工具”，直接点击“安装”，等待 Xcode 命令行工具安装完成后，重新执行 Rust 安装命令即可；若未提示，可手动输入以下命令安装：
    ```bash
    xcode-select --install
    ```
#### 注意事项
- 安装依赖时，需要输入管理员密码（Linux 输入 sudo 密码，macOS 输入系统登录密码），输入密码时终端不会显示，直接输入即可，输完按回车。
- 若安装依赖后仍报错，可重启终端，再重新执行安装命令。

### 问题2：安装成功后，输入 `rustc --version` 提示“不是内部或外部命令”（Windows 专属）
#### 报错现象
Rust 安装完成后，打开命令行，输入 `rustc --version`，提示“'rustc' 不是内部或外部命令，也不是可运行的程序或批处理文件”。
#### 报错原因
环境变量未生效，Rust 安装程序会自动配置环境变量，但需要重新打开终端才能生效，新手常忽略这一步。
#### 解决方案
1.  关闭当前所有打开的命令行窗口（包括安装时弹出的黑色命令行窗口）。
2.  重新打开一个新的命令行窗口（Win + R 输入 cmd，点击确定）。
3.  再次输入 `rustc --version`，即可正常显示版本号。
#### 补充方案
若重新打开终端仍报错，可手动配置环境变量：
1.  右键点击“此电脑”，选择“属性” → “高级系统设置” → “环境变量”。
2.  在“系统变量”中，找到“Path”，双击打开。
3.  点击“新建”，添加路径：`C:\Users\你的用户名\.cargo\bin`（将“你的用户名”替换为你电脑的用户名，如 `C:\Users\ZhangSan\.cargo\bin`）。
4.  点击“确定”保存，关闭所有窗口，重新打开命令行，输入 `rustc --version` 验证。

### 问题3：安装失败，提示“curl: command not found”（macOS/Linux 专属）
#### 报错现象
执行 Rust 安装命令时，终端提示“curl: command not found”，无法下载安装脚本。
#### 报错原因
系统未安装 curl 工具（curl 是用于下载文件的工具，安装脚本需要用 curl 下载）。
#### 解决方案
1.  macOS 系统：
    - 若已安装 Homebrew：打开终端，输入 `brew install curl`，安装完成后重新执行 Rust 安装命令。
    - 若未安装 Homebrew：打开浏览器访问 https://brew.sh/，按照页面提示安装 Homebrew（复制页面中的命令，在终端执行），安装完成后再安装 curl。
2.  Linux 系统：
    - Ubuntu/Debian 系列：`sudo apt update && sudo apt install curl`
    - CentOS/Fedora 系列：`sudo dnf install curl`
    安装完成后，重新执行 Rust 安装命令。

### 问题4：Windows 安装时，弹出“Windows 已保护你的电脑”提示
#### 报错现象
双击 `rustup-init.exe` 后，系统弹出安全提示，提示“Windows 已保护你的电脑，阻止了一个未识别的应用”，无法继续安装。
#### 报错原因
Windows 系统的安全机制，对未知来源的可执行文件进行拦截，rustup 是官方安全工具，可放心运行。
#### 解决方案
1.  点击提示窗口中的“更多信息”。
2.  点击“仍要运行”，即可继续安装程序。

### 问题5：安装脚本执行失败，提示“installer for platform 'xxx' not found”
#### 报错现象
执行安装命令后，终端提示“installer for platform 'xxx' not found, this may be unsupported”（xxx 为你的系统架构），安装中断。
#### 报错原因
你的系统架构不被当前版本的 rustup 安装脚本支持，或安装脚本下载过程中出现损坏。
#### 解决方案
1.  检查系统架构是否支持：Rust 支持主流架构（x86_64、aarch64 等），若为特殊架构（如 32位 x86），可访问 Rust 官方网站，查找对应版本的安装包。
2.  重新执行安装命令，确保网络稳定，避免脚本下载损坏。
3.  若仍失败，可手动下载安装包：访问 https://static.rust-lang.org/rustup/dist，找到对应系统架构的安装包，下载后手动运行安装。

## 二、Cargo 使用相关问题（02-cargo-basics.md 对应）
### 问题1：执行 `cargo run` 报错“No such file or directory”
#### 报错现象
输入 `cargo run` 后，终端提示“error: could not find `Cargo.toml` in `xxx` or any parent directory”（无法在当前目录或父目录找到 Cargo.toml）。
#### 报错原因
当前终端路径不在 Cargo 项目目录下，Cargo 需要在项目目录下（包含 Cargo.toml 文件）才能执行命令。
#### 解决方案
1.  确认你创建的项目名称（如 `hello-rust`）和保存路径（如桌面）。
2.  在终端中输入 `cd 项目路径/项目名称`，进入项目目录，示例：
    - Windows：`cd 桌面/hello-rust`
    - macOS/Linux：`cd ~/Desktop/hello-rust`
3.  进入项目目录后，再次执行 `cargo run` 即可。

### 问题2：执行 `cargo new` 报错“error: a directory named 'xxx' already exists”
#### 报错现象
输入 `cargo new hello-rust` 后，终端提示“error: a directory named 'hello-rust' already exists”（名为 hello-rust 的目录已存在）。
#### 报错原因
当前目录下已存在同名的项目文件夹，Cargo 不能创建重复名称的项目。
#### 解决方案
1.  修改项目名称，如 `cargo new hello-rust-01`（添加后缀，避免重复）。
2.  若需要在原有项目基础上修改，可直接进入原有项目目录，无需重新创建。
3.  若不需要原有项目，可删除原有项目文件夹，再重新执行 `cargo new` 命令。

### 问题3：执行 `cargo build` 或 `cargo run` 报错“error: cannot find crate for `std`”
#### 报错现象
编译或运行项目时，终端提示“error: cannot find crate for `std`”（无法找到 std 库）。
#### 报错原因
Rust 工具链安装不完整，std 库是 Rust 的标准库，缺少该库会导致无法编译项目。
#### 解决方案
1.  重新执行 Rust 安装命令，确保安装过程中没有中断，等待工具链完整安装。
2.  若安装完成后仍报错，执行 `rustup update` 命令，更新工具链，修复缺失的标准库。
3.  若仍失败，执行 `rustup self uninstall` 卸载 Rust，重新安装。

### 问题4：执行 `cargo run` 速度极慢，一直卡在“Compiling xxx”
#### 报错现象
执行 `cargo run` 后，终端一直显示“Compiling xxx v0.1.0”，长时间没有反应，或编译速度极慢。
#### 报错原因
1.  第一次编译项目时，Cargo 会初始化相关配置，下载必要的依赖（即使项目无依赖，也会初始化编译环境），速度较慢，属于正常现象。
2.  网络不稳定，导致依赖下载缓慢（若项目有依赖）。
3.  电脑配置较低，编译过程占用较多资源，速度较慢。
#### 解决方案
1.  耐心等待，第一次编译通常需要1-5分钟，后续编译会加快（Cargo 会缓存编译结果）。
2.  检查网络连接，确保网络稳定，避免下载中断。
3.  关闭电脑中其他占用资源较多的程序（如游戏、视频软件），释放内存和 CPU 资源。

## 三、代码编写相关问题（03-first-program.md 对应）
### 问题1：代码报错“expected ';' found '}'”
#### 报错现象
编译代码时，终端提示“error: expected ';' found '}'”（期望分号，却找到了右花括号）。
#### 报错原因
某条可执行语句的结尾遗漏了分号 `;`，Rust 语法要求，每条可执行语句的结尾必须加 `;`，否则会认为语句未结束。
#### 解决方案
1.  找到报错提示中指定的行（终端会显示报错行号，如 `src/main.rs:2:25`，表示第2行第25列）。
2.  在该语句的结尾添加分号 `;`，示例：
    错误代码：`println!("Hello, RustX!")`
    正确代码：`println!("Hello, RustX!");`
3.  保存代码，重新执行 `cargo run` 即可。

### 问题2：代码报错“unclosed delimiter: {”
#### 报错现象
编译代码时，终端提示“error: unclosed delimiter: {”（未闭合的分隔符：{）。
#### 报错原因
左花括号 `{` 和右花括号 `}` 没有成对出现，遗漏了右花括号，导致函数体未闭合。
#### 解决方案
1.  检查代码，找到左花括号 `{`（通常在 `fn main()` 后面）。
2.  在代码的最后，添加对应的右花括号 `}`，确保成对出现，示例：
    错误代码：
    ```rust
    fn main() {
        println!("Hello, RustX!");
    // 遗漏右花括号
    ```
    正确代码：
    ```rust
    fn main() {
        println!("Hello, RustX!");
    }
    ```
3.  保存代码，重新执行 `cargo run` 即可。

### 问题3：代码报错“expected indentation, found literal”
#### 报错现象
编译代码时，终端提示“error: expected indentation, found literal”（期望缩进，却找到了文本）。
#### 报错原因
函数体内部的代码没有缩进，Rust
