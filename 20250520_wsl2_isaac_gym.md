# 使用 WSL2 配置 Isaac Gym 仿真环境

## 前言

微软开发并开源了 WSL (Windows Subsystem for Linux)，相比安装双系统，WSL2 的使用更加灵活。
因此在需要配置 Isaac Gym 环境时我优先考虑了是否可以使用 WSL 进行仿真运行。
幸运的是在谷歌了一番后成功找到这样一篇[成功案例](https://ljoson.github.io/views/AI/RL/env.html)，于是便开始了我的折腾之路。

## 准备工作

- (可选) 安装 [PowerShell 7](https://github.com/PowerShell/PowerShell)
- 安装 [WindTerm](https://github.com/kingToolbox/WindTerm) / [MobaXterm](https://mobaxterm.mobatek.net/download.html)，我个人更喜欢 WindTerm。

这里安装 WindTerm / MobaXterm 一是因为 Windows 的终端真的太难用了；二是为了后续支持 X11 forward 来打开图形化 Isaac Gym 界面。

## WSL2 安装

首先当然是安装 WSL2。目前 WSL2 是 Windows 的默认选项，因此直接按照[官网指南](https://learn.microsoft.com/zh-cn/windows/wsl/install)进行安装即可。

右键终端，打开 PowerShell

1. 安装 WSL2 并重启电脑

```shell
wsl --install
```

2. 安装 Ubuntu
```shell
# 列出所有可安装的发行版
wsl -l -o

# 选择 20.04 及以上的 Ubuntu 发行版进行安装
wsl --install -d Ubuntu-24.04
```

3. 启动 Ubuntu

```shell
wsl
```

4. Troubleshooting

此时大概率会报错：
```shell
wsl: 检测到 localhost 代理配置，但未镜像到 WSL。NAT 模式下的 WSL 不支持 localhost 代理。
```

解决方案[^1]：

%USERPROFILE%

https://github.com/microsoft/WSL/issues/10753#issuecomment-1814839310

## CUDA

https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=deb_network

zshrc
export PATH=/usr/local/cuda-12.9/bin:$PATH
export LD_LIBRARY_PATH=/usr/lib/wsl/lib/:$LD_LIBRARY_PATH

## ssh

sudo apt install openssh-server

sudo vim /etc/ssh/sshd_config 修改port

https://askubuntu.com/questions/1512180/installing-openssh-server-in-ubuntu-24-04-on-wsl2

## isaacgym

mkdir -p dev/toolchains

tar -xzvf IsaacGym_Preview_4_Package.tar.gz -C dev/toolchains/

## miniconda

mkdir -p dev/toolchains/miniconda3

wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O dev/toolchains/miniconda3/miniconda.sh

cd dev/toolchains/miniconda3
chmod 777 miniconda.sh
./miniconda.sh -b -u -p ~/dev/toolchains/miniconda3
rm miniconda.sh

~dev/toolchains/miniconda3/bin/conda init zsh

cd examples
python 1080_balls_of_solitude.py
find / -name libpython3.8.so.1.0
sudo cp /home/erbws/dev/toolchains/miniconda3/envs/unitree-rl/lib/libpython3.8.so.1.0 /usr/lib/x86_64-linux-gnu

https://blog.csdn.net/w5688414/article/details/128070172

## vulkan

https://zhuanlan.zhihu.com/p/576320322
https://vulkan.lunarg.com/sdk/home

---

[^1]: [microsoft/WSL#10753 (comment)](https://github.com/microsoft/WSL/issues/10753#issuecomment-1814839310)