# 00 aseprite

2d像素画的神器，自编译版本亦可商用。

## 下载源码
- 到其官网clone源码, 或者直接下载最新的release包
- 查看其install.md文件需要一些依赖

```
# Dependencies

To compile Aseprite you will need:

* The latest version of [CMake](https://cmake.org) (3.16 or greater)
* [Ninja](https://ninja-build.org) build system
* And a compiled version of the `aseprite-m102` branch of
  the [Skia library](https://github.com/aseprite/skia#readme).
  There are [pre-built packages available](https://github.com/aseprite/skia/releases).
  You can get some extra information in
  the [*laf* dependencies](https://github.com/aseprite/laf#dependencies) page.

## Windows dependencies

* Windows 10 (we don't support cross-compiling)
* [Visual Studio Community 2022](https://visualstudio.microsoft.com/downloads/) (we don't support [MinGW](#mingw))
* The [Desktop development with C++ item + Windows 10.0.18362.0 SDK](https://imgur.com/a/7zs51IT)
  from the Visual Studio installer

```
- 根据文档安装依赖
- 安装完毕之后，直接使用网友写的脚本填入具体的依赖信息
```bat
:: ！！请用管理员权限运行！！
@echo off
:: ============配置部分自行修改============
:: VsDevCmd.bat文件位置
set VSDEVC="D:\vs2022\IDE\Common7\Tools\VsDevCmd.bat"
:: (Aseprite)[https://github.com/aseprite/aseprite/releases]
set ASEPRITE="E:\Code\unity\aseprite\Aseprite-v1.3.9.1-Source"
:: (Skia)[https://github.com/aseprite/skia/releases]
set DSKIA="E:\Code\unity\aseprite\skia"
:: (Ninja)[https://github.com/ninja-build/ninja/releases]
set NINJA="E:\Code\unity\aseprite\ninja\"
set CMAKE=""D:\vs2022\IDE\Common7\IDE\CommonExtensions\Microsoft\CMake\CMake\bin\cmake.exe""
:: ============分割线============


:: ============以下谨慎修改============
set PATH=%NINJA%;%PATH%
call %VSDEVC% -arch=x64
cd /d %ASEPRITE%
if exist build rd /s /q build
mkdir build
cd build
CMAKE -DCMAKE_BUILD_TYPE=RelWithDebInfo -DLAF_BACKEND=skia -DSKIA=%DSKIA% -DSKIA_LIBRARY_DIR=%DSKIA%\out\Release-x64 -DSKIA_LIBRARY=%DSKIA%\out\Release-x64\skia.lib -G Ninja ..
NINJA aseprite
echo Finish
if exist bin explorer bin
pause 
```
emmmm折腾了一阵 失败了

## github action
直接fork这个[仓库](https://github.com/Algieba-dean/aseprite_builder/actions)
按照描述
提前打开Actions，和gitlab的pipeline一样，手动触发，就能等着了.
- 跑完就能在release中找到并下载了。 从[链接](https://github.com/Algieba-dean/aseprite_builder/releases/download/untagged-517f3d3c7d33df3531ec/Aseprite-v1.3.9.1-Windows.zip)可直接下载