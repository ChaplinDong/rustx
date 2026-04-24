# 02 Cargo 基础（Rust 核心工具，详细实操版）
Cargo 是 Rust 官方提供的「项目管理+构建+测试+依赖管理」一体化工具，是 Rust 开发的必备工具，新手入门阶段，无需深入理解其底层实现，重点掌握以下核心命令和用法，即可应对大部分入门场景，后续学习中再逐步深入。

简单来说：Cargo 帮你搞定“项目创建、代码编译、程序运行、依赖下载”所有繁琐操作，不用手动配置编译参数、不用手动管理依赖，极大提升开发效率。

## 一、先确认 Cargo 已安装成功
在开始学习之前，先确认 Cargo 已安装成功（承接上一节的环境搭建）：
1.  打开终端（Windows：cmd；macOS/Linux：终端）
2.  输入命令：`cargo --version`
3.  若显示版本号（如 cargo 1.95.0 (abc12345 2026-03-04)），则说明安装成功；若提示“命令未找到”，请回到 01-install-rust.md，重新检查环境搭建步骤。

## 二、常用核心命令（必记、必练，详细解读）
### 1. 创建新项目（最常用，基础中的基础）
#### 命令格式
```bash
cargo new 项目名称
```
#### 示例（新手建议跟着示例操作）
```bash
cargo new hello-rust
```
#### 操作步骤
1.  打开终端，确保当前路径是你想创建项目的目录（如 Windows 可先输入 `cd 桌面`，将项目创建在桌面上；macOS/Linux 可输入 `cd ~/Desktop`）
2.  输入上述示例命令，按回车执行
3.  执行完成后，终端会显示“Created binary (application) `hello-rust` package”，表示项目创建成功

#### 命令详解
- `cargo new`：固定命令，用于创建一个新的 Rust 项目
- `hello-rust`：项目名称，可自定义（建议英文、小写，用连字符分隔，如 `hello-rust`、`rust-demo`，不要用中文或特殊符号）
- 项目创建后，会自动生成一个标准的 Rust 项目结构，无需手动创建文件夹和文件

#### 生成的项目结构详解（重点了解）
创建成功后，会在当前目录下生成一个名为 `hello-rust` 的文件夹，打开该文件夹，结构如下：
```
hello-rust/
├── Cargo.toml       # 项目配置文件（核心，管理依赖、项目信息）
└── src/             # 源代码目录（所有 Rust 代码都放在这里）
    └── main.rs      # 主程序文件（程序入口，所有代码写在这里）
```
- `Cargo.toml`：相当于项目的“身份证+配置清单”，记录项目名称、版本、作者、依赖等信息，后续添加依赖、配置项目都需要修改这个文件
- `src/main.rs`：Rust 程序的入口文件，所有可执行程序的代码都从这个文件开始执行，默认已经生成了简单的 Hello World 代码

#### 注意事项
1.  项目名称不能重复：若当前目录下已存在 `hello-rust` 文件夹，执行命令会报错，需修改项目名称（如 `hello-rust-01`）
2.  不要手动修改项目结构：新手阶段，不要随意删除或修改 `Cargo.toml` 和 `src` 文件夹，否则会导致项目无法编译运行
3.  进入项目目录：项目创建成功后，需要先进入项目目录，才能执行后续的编译、运行命令，进入命令为 `cd hello-rust`（将 `hello-rust` 替换为你的项目名称）

### 2. 编译并运行项目（最常用，核心操作）
#### 命令格式
```bash
cargo run
```
#### 操作步骤
1.  确保终端当前路径是项目目录（如 `hello-rust` 目录，可通过 `cd hello-rust` 进入）
2.  输入 `cargo run` 命令，按回车执行
3.  执行过程：
    1.  首先，Cargo 会自动检查项目是否有依赖（新手项目默认无依赖）
    2.  然后，Cargo 会调用 rustc 编译器，编译 `src/main.rs` 中的代码
    3.  编译成功后，自动运行编译后的可执行文件
4.  运行成功后，终端会显示 `Hello, world!`（默认代码的输出结果）

#### 命令详解
- `cargo run`：一站式命令，相当于“编译+运行”两步操作，新手首选
- 若代码有错误，编译会失败，终端会显示错误信息（如语法错误、拼写错误），需根据错误提示修改代码后，重新执行 `cargo run`

#### 常见问题
1.  执行 `cargo run` 报错“No such file or directory”：原因是当前终端路径不在项目目录下，需先执行 `cd 项目名称` 进入项目目录
2.  编译失败，提示“expected ';' found '}'”：原因是代码中语句结尾忘记加 `;`，修改代码后重新执行即可
3.  运行速度慢：第一次运行时，Cargo 会初始化相关配置，速度较慢，后续运行会加快

### 3. 仅编译项目（不运行，进阶用法）
#### 命令格式
```bash
cargo build
```
#### 操作步骤
1.  进入项目目录（如 `hello-rust`）
2.  输入 `cargo build` 命令，按回车执行
3.  编译成功后，终端会显示“Finished dev [unoptimized + debuginfo] target(s) in 0.1s”
4.  编译后的可执行文件会生成在 `target/debug/` 目录下（如 Windows 为 `target/debug/hello-rust.exe`，macOS/Linux 为 `target/debug/hello-rust`）

#### 命令详解
- `cargo build`：仅编译代码，不运行程序，适合需要单独获取可执行文件的场景
- 编译后的可执行文件可以直接双击运行（Windows），或在终端中输入 `./target/debug/hello-rust`（macOS/Linux）运行
- 每次修改代码后，重新执行 `cargo build`，会重新编译修改后的代码，未修改的代码不会重新编译，提升效率

#### 发布版本编译（优化编译，重点）
#### 命令格式
```bash
cargo build --release
```
#### 详解
- 该命令会以“发布模式”编译项目，编译后的可执行文件会进行优化，运行速度更快、占用内存更小，适合项目完成后发布使用
- 编译后的可执行文件会生成在 `target/release/` 目录下，而非 `target/debug/`
- 注意：发布模式编译速度较慢（因为要进行优化），新手开发阶段，用 `cargo build` 即可，发布时再用 `cargo build --release`

### 4. 查看 Cargo 版本（辅助命令）
#### 命令格式
```bash
cargo --version
```
#### 详解
- 用于验证 Cargo 是否安装成功，以及查看当前 Cargo 版本
- 若需要更新 Cargo，可执行 `rustup update` 命令（Cargo 是 Rust 工具链的一部分，更新工具链即可更新 Cargo）

### 5. 查看帮助信息（辅助命令）
#### 命令格式
```bash
cargo --help
```
#### 详解
- 若忘记某个命令的用法，可执行该命令，查看 Cargo 所有命令的说明
- 也可查看具体命令的帮助，如 `cargo new --help`，查看 `cargo new` 命令的详细用法

## 三、Cargo.toml 配置文件详解（新手必懂基础）
`Cargo.toml` 是项目的核心配置文件，新手阶段无需深入修改，只需了解其基础结构，后续添加依赖时会用到。

默认生成的 `Cargo.toml` 内容如下（以 `hello-rust` 项目为例）：
```toml
[package]
name = "hello-rust"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
```
#### 各部分详解
1.  `[package]`：包配置区域，用于配置项目的基本信息
    - `name`：项目名称，和创建项目时的名称一致，可手动修改
    - `version`：项目版本，遵循语义化版本（如 0.1.0，依次为 major、minor、patch），新手可默认不变
    - `edition`：Rust 版本edition，默认是 2021（当前稳定版本），无需修改
2.  `[dependencies]`：依赖配置区域，用于添加项目所需的第三方依赖（如网络库、数据库库等）
    - 新手阶段，该区域为空，后续学习中，若需要添加依赖，可在该区域添加，如 `rand = "0.8.5"`（添加随机数库）
    - 添加依赖后，执行 `cargo run` 或 `cargo build`，Cargo 会自动下载并安装依赖

## 四、新手常见操作场景（贴合实际开发）
### 场景1：创建项目 → 编写代码 → 运行项目（完整流程）
1.  打开终端，进入桌面：`cd 桌面`（Windows）/ `cd ~/Desktop`（macOS/Linux）
2.  创建项目：`cargo new hello-rust-demo`
3.  进入项目：`cd hello-rust-demo`
4.  打开 `src/main.rs` 文件，修改代码（如将 `Hello, world!` 改为 `Hello, RustX!`）
5.  运行项目：`cargo run`
6.  查看运行结果：终端显示 `Hello, RustX!`，操作成功

### 场景2：修改代码后重新运行
1.  打开 `src/main.rs` 文件，修改代码
2.  无需重新编译，直接执行 `cargo run`，Cargo 会自动重新编译并运行
3.  若修改后报错，根据终端提示修改代码，再次执行 `cargo run`

### 场景3：生成可执行文件，单独运行
1.  进入项目目录，执行 `cargo build`
2.  找到 `target/debug/` 目录下的可执行文件（如 `hello-rust-demo.exe`）
3.  双击该文件（Windows），或在终端中输入 `./target/debug/hello-rust-demo`（macOS/Linux），即可运行

## 五、注意事项
1.  命令区分大小写：Cargo 命令全部为小写，如 `cargo run` 不能写成 `Cargo Run`，否则会报错
2.  项目路径不要包含中文：若项目路径包含中文，可能导致编译失败，建议将项目创建在纯英文路径下（如桌面、文档文件夹）
3.  不要手动删除 `target` 文件夹：`target` 文件夹是编译生成的目录，删除后，Cargo 会重新编译，不影响项目，但会浪费时间
4.  依赖管理：添加依赖时，要确保依赖版本正确，可在 https://crates.io/（Rust 官方依赖库）查询依赖的最新版本
5.  编译缓存：Cargo 会缓存编译结果，第二次编译时会加快速度，若需要清理缓存，可执行 `cargo clean` 命令（清理 `target` 文件夹）
