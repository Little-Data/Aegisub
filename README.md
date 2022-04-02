[![Build Status](https://github.com/wangqr/Aegisub/actions/workflows/gha-ci.yml/badge.svg)](https://github.com/wangqr/Aegisub/actions/workflows/gha-ci.yml)

# Aegisub
### 以下翻译均来自Google翻译，有任何问题请到[wangqr维护的发行版](https://github.com/wangqr/Aegisub/releases)

### 本库为文件存储库

有关二进制文件和一般信息，请参阅 [Aegisub官方网站](http://www.aegisub.org) 

错误跟踪器可以在[wangqr维护的发行版issues](https://github.com/wangqr/Aegisub/issues).

如果要测试官方上游版本，r8942 [可以从Little-Data下载](https://github.com/Little-Data/Aegisub-storaged/releases/tag/r8942)或者[到官方测试发行版](http://www.plorkyeran.com/aegisub/). 如果 r8942 和这个正式版都有一些共同的问题，请到wangqr维护的发行版或[Aegisub官方issues页面](https://github.com/Aegisub/Aegisub/issues)（官方issues页面现在**没人管**）可能会让更多人看到你的问题

IRC 上提供支持 ( irc://irc.rizon.net/aegisub ,对于上游版本，现在**没人管**)或通过issues.

## 构建Aegisub

### autoconf / make (for linux and macOS)

这是在 linux 和 macOS 上构建 Aegisub 的推荐方式。 目前 AviSynth+ 支持不包含在 autoconf 项目中。 如果您需要 AviSynth+ 支持，请参阅下面的 CMake 说明。
Aegisub has some required dependencies:
* `libass`
* `Boost`(with ICU support)
* `OpenGL`
* `libicu`
* `wxWidgets`
* `zlib`
* `fontconfig` (not needed on Windows)
* `luajit` (or `lua`)

和可选依赖项：
* `ALSA`
* `FFMS2`
* `FFTW`
* `Hunspell`
* `OpenAL`
* `uchardet`
* `AviSynth+`

可以使用发行版提供的包管理器来安装这些依赖项。 软件包名称因发行版而异。 一些有用的参考资料是：

* 对于 ArchLinux，请参阅 [AUR](https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=aegisub-git).
* 对于 Ubuntu，请参阅 [Travis](.travis.yml#L14-L32).
* 对于 macOS，请参阅 [Special notice for macOS](https://github.com/wangqr/Aegisub/wiki/Special-notice-for-macOS) 在项目wiki上.

安装依赖项后，您可以使用以下命令克隆和构建 Aegisub：
```sh
git clone https://github.com/wangqr/Aegisub.git
cd Aegisub
./autogen.sh
./configure
make
```

### CMake (for Windows, linux and macOS)

此 fork 还提供 CMake 构建。 由于对使用 CMake 构建 LuaJIT 的支持有限，目前仅支持 x86 和 x64。

您仍然需要安装上面的依赖项。 要启用 AviSynth+ 支持还需要它。 在 Windows 上安装依赖项可能很棘手，因为 Windows 没有一个好的包管理器。 参考 [the Wiki page](https://github.com/wangqr/Aegisub/wiki/Compile-guide-for-Windows-(CMake,-MSVC)) 关于如何获取 Windows 上的所有依赖项。

安装依赖项后，您可以使用以下命令克隆和构建 Aegisub：
```sh
git clone https://github.com/wangqr/Aegisub.git
cd Aegisub
./build/version.sh .  # This will generate build/git_version.h
mkdir build-dir
cd build-dir
cmake ..  # Or use cmake-gui / ccmake
make
```

可以通过切换 CMake 中的功能来打开/关闭`WITH_*` 开关。

Archlinux 用户也可以试试[PKGBUILD in project wiki](https://github.com/wangqr/Aegisub/wiki/PKGBUILD-for-Arch).

## 更新 Moonscript

从 Moonscript 存储库中，运行`bin/moon bin/splat.moon -l moonscript moonscript/ > bin/moonscript.lua`.
打开新建的 `bin/moonscript.lua`, 并在其中进行以下更改：

1. 前置文件的最后一行, `package.preload["moonscript"]()`,与 `return`, producing `return package.preload["moonscript"]()`.
2. 在函数内`package.preload['moonscript.base']`, 删除 `moon_loader`, `insert_loader`, 和 `remove_loader`. 这意味着在返回的表中删除它们的声明、定义和条目。
3. 在函数内 `package.preload['moonscript']`, 删除该行 `_with_0.insert_loader()`.

该文件现在可以使用了，可以放入 `automation/include` 在 Aegisub 存储库中。

## 许可证

此存储库中的所有文件均根据各种与 GPL 兼容的 BSD 样式许可进行许可； 有关详细信息，请参阅许可证和各个源文件。
由于包含 fftw3，官方的 Windows 版本是 GPLv2。
