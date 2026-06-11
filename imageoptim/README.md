# ImageOptim

[![License: GPL v2](https://img.shields.io/badge/License-GPL%20v2-blue.svg)](https://www.gnu.org/licenses/old-licenses/gpl-2.0.html)
[![Platform: macOS](https://img.shields.io/badge/Platform-macOS-lightgrey.svg)](https://imageoptim.com)

[ImageOptim](https://imageoptim.com) 是一款适用于 macOS 的免费开源图像优化工具，通过无损或有损压缩方式减小图像文件体积，同时保持视觉质量。它为多个业界领先的命令行优化工具提供了简洁易用的图形界面。

## 功能特性

- **无损压缩**：在不损失任何图像质量的前提下减小文件体积
- **有损压缩**：通过 pngquant 等工具实现更高压缩率（可选）
- **多格式支持**：支持 PNG、JPEG、GIF、SVG 格式及文件夹批量处理
- **拖放操作**：直接将图像文件或文件夹拖入窗口即可开始优化
- **Finder 集成**：通过 macOS 服务菜单在 Finder 中直接优化图像
- **AppleScript 支持**：可通过脚本自动化批量处理工作流
- **多语言界面**：支持中文、英文、日文、韩文等 20+ 种语言

## 集成的优化工具

ImageOptim 整合了以下第三方优化工具：

| 工具 | 格式 | 说明 |
|------|------|------|
| [OxiPNG](https://lib.rs/crates/oxipng) | PNG | Josh Holmer 开发的高性能 PNG 优化器 |
| [pngquant](https://pngquant.org) | PNG | 有损 PNG 压缩，支持质量控制 |
| [PNGCrush](http://pmt.sourceforge.net/pngcrush/) | PNG | 经典 PNG 无损压缩工具 |
| [PNGOUT](http://www.advsys.net/ken/utils.htm) | PNG | Ken Silverman 开发的 PNG 优化器 |
| [AdvPNG](http://advancemame.sourceforge.net/doc-advpng.html) | PNG | 基于 zlib 的 PNG 重压缩工具 |
| [MozJPEG](https://github.com/mozilla/mozjpeg) | JPEG | Mozilla 开发的高质量 JPEG 编码器 |
| [Jpegoptim](http://www.kokkonen.net/tjko/projects.html) | JPEG | JPEG 文件优化与质量控制 |
| [Guetzli](https://github.com/google/guetzli) | JPEG | Google 开发的感知质量 JPEG 编码器 |
| [Zopfli](https://github.com/google/zopfli) | PNG | Google 开发的高压缩率 zlib 实现 |
| [Gifsicle](http://www.lcdf.org/gifsicle/) | GIF | GIF 图像优化与处理 |
| [SVGO](https://github.com/svg/svgo) | SVG | 基于 Node.js 的 SVG 优化器 |
| [svgcleaner](https://github.com/RazrFalcon/svgcleaner) | SVG | Rust 编写的 SVG 清理工具 |

## 系统要求

- macOS（版本要求见 `Info.plist` 中的 `LSMinimumSystemVersion`）
- Xcode（用于从源码构建）

## 从源码构建

### 1. 克隆仓库

```bash
git clone https://github.com/ImageOptim/ImageOptim.git
cd ImageOptim
```

### 2. 初始化子模块

```bash
git submodule update --init --recursive
```

### 3. 下载依赖工具

```bash
make
```

此命令会自动下载 `pngout` 二进制文件并构建帮助索引。

### 4. 用 Xcode 打开并构建

```bash
open ImageOptim.xcodeproj
```

在 Xcode 中选择目标 scheme 并点击 **Build**（⌘B）或 **Run**（⌘R）。

## 项目结构

```
imageoptim/
├── Backend/                  # 核心后端逻辑
│   ├── Workers/              # 各优化工具的 Worker 封装
│   │   ├── OxiPngWorker.m    # OxiPNG 集成
│   │   ├── PngquantWorker.*  # pngquant 集成
│   │   ├── JpegtranWorker.*  # jpegtran/MozJPEG 集成
│   │   ├── GifsicleWorker.m  # Gifsicle 集成
│   │   ├── SvgoWorker.*      # SVGO 集成
│   │   └── ...               # 其他工具集成
│   ├── File.*                # 文件模型
│   ├── Job.*                 # 优化任务模型
│   ├── JobQueue.*            # 任务队列管理
│   └── DirScanner.*          # 目录扫描
├── ImageOptimize/            # Finder 扩展
├── *.lproj/                  # 本地化资源（20+ 种语言）
├── Base.lproj/               # 主界面 XIB 文件
├── ImageOptim.xcodeproj/     # Xcode 项目文件
├── Makefile                  # 构建辅助脚本
└── Info.plist                # 应用配置
```

## 使用方法

1. **拖放文件**：将图像文件或包含图像的文件夹直接拖入 ImageOptim 窗口
2. **Finder 服务**：在 Finder 中右键选择文件，通过「服务」菜单选择「ImageOptimize」
3. **AppleScript 自动化**：使用 AppleScript 将 ImageOptim 集成到自动化工作流中

```applescript
tell application "ImageOptim"
    optimize {"/path/to/image.png"}
end tell
```

## 偏好设置

ImageOptim 提供丰富的偏好设置，可在「偏好设置」面板中调整：

- 启用/禁用各个优化工具
- 调整 PNG 有损压缩质量范围
- 设置 JPEG 压缩质量
- 配置是否保留文件元数据（EXIF、颜色配置文件等）

## 许可证

ImageOptim 遵循 [GNU 通用公共许可证第 2 版或更高版本](http://www.gnu.org/licenses/old-licenses/gpl-2.0.html)发布。

> **注意**：捆绑的 PNGOUT 工具不受 GPL 约束，已获得 Ardfry Imaging, LLC 的使用许可。

## 贡献

欢迎提交 Pull Request 和 Issue！

- **Bug 报告**：请在 [GitHub Issues](https://github.com/ImageOptim/ImageOptim/issues) 中提交
- **功能建议**：同样通过 GitHub Issues 提交
- **代码贡献**：Fork 本仓库，创建功能分支，提交 PR

## 相关链接

- 🌐 官方网站：[https://imageoptim.com](https://imageoptim.com)
- 📦 Mac App Store：[ImageOptim on App Store](https://apps.apple.com/app/imageoptim/id439697913)
- 🐛 问题追踪：[GitHub Issues](https://github.com/ImageOptim/ImageOptim/issues)
- 📡 更新源：[appcast.xml](https://imageoptim.com/appcast.xml)

---

由 [Kornel Lesiński](https://kornel.ski) 及贡献者开发维护。
