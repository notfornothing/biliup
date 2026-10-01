# 本机启动

当前分支 `dev` 停在稳定版 **v1.2.11**，上面只有说明文档。录和传都在一个程序里：`biliup server`。它同时提供网页、开播检测、录制和投稿。没有单独的数据库服务，也没有注册中心。

数据库是文件 `data/data.sqlite3`。第一次启动时程序自己创建。

## 要装什么

只在编译时需要：

- **Node.js ≥ 20.9**。用来执行 `npm run build`，把网页打成静态文件 `out/`。
- **Rust stable ≥ 1.95**。`cargo` 是它的构建命令，随 Rust 一起安装。本机若提示找不到 `cargo`，先装再重开终端：`winget install --source winget Rustlang.Rustup`。
- **Visual Studio 2022 的 C++ 生成工具**。Windows 上编 Rust 要用它的链接器 `link.exe`。本机已有 `C:\Program Files (x86)\Microsoft Visual Studio\2022\BuildTools`。

运行时需要：

- **FFmpeg**，并且在 `PATH` 里。录制、转封装和精确切片会调用它。

Python 那条入口这次不用。

## 启动

在 `D:\Project_AI\biliup`：

```powershell
$env:Path = "$env:USERPROFILE\.cargo\bin;" + $env:Path
npm i
npm run build
cmd /c "call `"C:\Program Files (x86)\Microsoft Visual Studio\2022\BuildTools\VC\Auxiliary\Build\vcvars64.bat`" >nul && cargo run --bin biliup -- server"
```

`npm run build` 必须在 `cargo` 之前。网页会被编进程序，没有 `out/` 目录编译会失败。

浏览器打开 `http://127.0.0.1:19159`。默认只听本机，不用加 `--auth`，也不用设管理员密码。

改了网页再执行 `npm run build`，然后重新 `cargo run`。只改 Rust 时直接重新 `cargo run`。

## 已经写进本机配置的

- 文件名：`{streamer}｜%Y-%m-%d %H-%M-%S｜{title}`
- 投稿通道：`web`
- 模板「自动投稿（仅自己可见）」：同一套标题，简介两行 `主播:{streamer}`、`地址:{url}`，自制、仅自己可见

## 编译时注意

迁移目录 `crates/biliup-cli/migrations` 里的 `.sql` 必须是 LF。Windows 检出成 CRLF 时，编译会直接失败。把这些文件的换行改成 LF 后再编。

拉 Rust 依赖慢时，先开本机代理（现在是 `127.0.0.1:7890`），再执行：

```powershell
$env:HTTPS_PROXY = "http://127.0.0.1:7890"
$env:HTTP_PROXY = "http://127.0.0.1:7890"
```
