## 前言

这是一个单纯的记录性文档，用来记录我如何从头开始配置 macOS 上我需要的开发环境

> **Q: 为什么不用 nix-darwin?**

A: 这东西太复杂了，为了安装某个工具链还需要搜索配置一大堆内容，make life simple

下面正式开始

## 工具链文件夹

首先创建一个工具链文件夹，后续工具链基本都配置在该文件夹内，方便管理

```shell
mkdir ~/dev/toolchains/
cd ~/dev/toolchains
```

## oh-my-zsh

> 这里需要配置一个临时 proxy 用来访问 raw.githubusercontent.com

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

修改生成的 `～/.zshrc` 

1. 终端主题 `ZSH_THEME="af-magic"`
2. 添加 proxy

## Homebrew

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# ~/.zshrc
export PATH=/opt/homebrew/bin:$PATH
```

## Flutter

Flutter 分为原始仓库 [flutter / flutter](https://github.com/flutter/flutter) 和 [OpenHarmony-SIG / flutter_flutter](https://gitcode.com/openharmony-sig/flutter_flutter)，其中后者提供了 ohos 支持

1. flutter 安装

可以考虑直接从 [Archive](https://docs.flutter.dev/release/archive) 下载 zip 或者 git clone。我的本地环境从 GitHub clone 下载速度更快

```shell
git clone -b stable https://github.com/flutter/flutter.git

# ~/.zshrc
export PATH=~/dev/toolchains/flutter/bin:$PATH
```

2. flutter_ohos 安装

```shell
git clone -b 3.22.0-ohos https://gitcode.com/openharmony-sig/flutter_flutter.git flutter_ohos

# ~/.zshrc
alias flutter_ohos="~/dev/toolchains/flutter_ohos/bin/flutter"
```

> **Q: 为什么不用 fvm?**

A: 我找不到用 fvm 的理由，IDEA 可以自己管理 flutter 依赖

## macOS/iOS

从 App Store 下载 Xcode

```shell
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
sudo xcodebuild -runFirstLaunch
sudo xcodebuild -license
```

## CocoaPods

```shell
brew install cocoapods
```

一定不要用[官网](https://cocoapods.org/)提供的 sudo gem install cocoapods

## HarmonyOS SDK

从[官网](https://developer.huawei.com/consumer/cn/deveco-studio/)下载 DecEco Studio

```shell
# ~/.zshrc
export TOOL_HOME=/Applications/DevEco-Studio.app/Contents
export DEVECO_SDK_HOME=$TOOL_HOME/sdk
export PATH=$TOOL_HOME/tools/ohpm/bin:$PATH
export PATH=$TOOL_HOME/tools/hvigor/bin:$PATH
export PATH=$TOOL_HOME/tools/node/bin:$PATH
```