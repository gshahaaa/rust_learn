# Cargo
Cargo为项目的建立，构建以及测试，运行，部署提供了一系列工具。同时也为Rust项目的管理提供完善的包管理工具,其允许rust项目声明各种依赖项，确保获得可重复的构建。
## Cargo命令
* cargo build: 构建工程
* cargo run：运行工程
* cargo tree：查看第三方库的版本和依赖关系
* cargo bench：运行benchmark
* cargo fmt：类似于go fmt，代码格式化
* cargo check: 可以在不生成可执行文件的情况下，对代码进行检查能不能通过编译

cargo build/run --release编译要比默认的debug编译性能提升10倍以上，但是release的编译速度较慢。
## 作用
* 引入两个包含各种项目信息的元数据文件
* 获取并构建项目的依赖项
* 调用rustc或者其余工具构建项目
* 引入约定从而更容易构建rust包

## 词汇表
**Artifact** 
为一系列编译过程创建的文件，通常包括链接库，可执行二进制文件以及生成的文档
**Cargo.lock** 
用于在workspace或者package下准确保存每一个依赖的版本，由cargo自动生成。其确保构建是可以重现的，在第一次构建项目时，cargo会计算出所有符合要求的依赖版本并存入该文件，将来构建项目时，cargo会使用其中指定的版本。当再次升级Crate时，需要使用`cargo update`。其会忽略cargo.lock文件，并重新计算所有符合cargo.toml的版本。
**Cargo.toml** 
属于workspace或者package的描述,同时再次保存了项目依赖的Crate版本
**Crate** 
为一个library或者可执行程序，每一个为cargo package定义的target属于一个Crate。
