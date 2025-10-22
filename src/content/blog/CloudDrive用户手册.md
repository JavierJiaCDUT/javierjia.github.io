---
title: 'CloudDrive 用户手册 - macOS 安装指南'
description: 'CloudDrive 在 macOS 系统上的完整安装指南，包括 Homebrew 和手动安装两种方式'
pubDate: 'Jan 22 2025'
---

## macOS 安装指南

### 方式一：Homebrew 安装（推荐）

#### 前置要求

确保你的 macOS 已安装 Homebrew。

运行前请确保系统已安装 macFUSE，并已按照提示正确设置权限。或者通过 brew 命令安装 macFUSE：

```bash
brew install --cask macfuse
```

#### 安装 CloudDrive

运行以下命令安装 CloudDrive：

```bash
brew tap cloud-fs/clouddrive2
brew install clouddrive2
```

#### 启动 CloudDrive

运行以下命令启动 CloudDrive：

```bash
brew services start clouddrive2
```

运行成功后将会自动创建后台服务并且下次登录会自动运行。

启动成功后用浏览器打开 http://localhost:19798 进入管理配置界面。

#### 停止 CloudDrive

运行以下命令停止 CloudDrive：

```bash
brew services stop clouddrive2
```

#### 卸载 CloudDrive

运行以下命令卸载 CloudDrive：

```bash
brew services stop clouddrive2
brew uninstall clouddrive2
```

#### 更新 CloudDrive

运行以下命令更新 CloudDrive：

```bash
brew services stop clouddrive2
brew update
brew upgrade clouddrive2
brew services start clouddrive2
```

### 方式二：手动安装

#### 前置要求

运行前请确保系统已安装 macFUSE，并已按照提示正确设置权限。

#### 安装步骤

1. 访问我们的官方网站下载对应架构的 CloudDrive。

2. 打开终端并导航到下载的安装包所在的目录。

3. 运行以下命令以展开 CloudDrive：

```bash
tar zxvf clouddrive-2-macos-$ARCH-$VERSION.tgz
```

4. 进入展开后的目录，运行：

```bash
./clouddrive
```

#### 自启动配置

可以将 clouddrive 运行加入系统自启动，或者设置为系统服务，具体操作请参考操作系统帮助。
w
