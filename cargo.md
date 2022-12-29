# Cargo
Cargo为项目的建立，构建以及测试，运行，部署提供了一系列工具。同时也为Rust项目的管理提供完善的包管理工具,其允许rust项目声明各种依赖项，确保获得可重复的构建。
## Cargo命令
* cargo build: 构建工程
* cargo run：运行工程
* cargo tree：查看第三方库的版本和依赖关系
* cargo bench：运行benchmark
* cargo fmt：类似于go fmt，代码格式化

cargo build/run --release编译要比默认的debug编译性能提升10倍以上，但是release的编译速度较慢。