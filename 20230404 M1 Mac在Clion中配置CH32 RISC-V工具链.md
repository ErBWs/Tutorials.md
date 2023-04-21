## 1.前言

- 感谢该[文章](https://blog.csdn.net/wu58430/article/details/126548417?spm=1001.2014.3001.5506)给我的启发！

- 准备好CLion

- Windows或Linux虚拟机并安装好MounRiverStudio，推荐Linux虚拟机

- 从[MRS](http://www.mounriver.com/download)官网下载好MacOS工具链

![MRS](https://img-blog.csdnimg.cn/img_convert/14ea61b026bdb6cb4e68e4d2348b4526.png)

## 2.环境配置

1. 解压下载好的安装包，根据电脑的芯片选择解压对应的`openocd`及`xpack-riscv-none-embed-gcc-8.2.0`![image-20230404200116832](https://img-blog.csdnimg.cn/img_convert/d1355053e553c93d79578b1ab79b44bb.png)
2. 将解压好的两个文件夹放在自己的想放的目录下
3. 在`Users`文件夹内按下`cmd+shift+.`显示隐藏文件，打开`.zshrc`或其他环境变量配置文件，如下所示添加环境变量，保存并关闭

```
export RISV_GCC=/Users/baohan/ErBW_s/Code/Toolchains/xpack-riscv-none-embed-gcc-8.2.0/bin	#替换为你的文件所在路径
export RISV_OPENOCD=/Users/baohan/ErBW_s/Code/Toolchains/openocd-arm64/bin	#替换为你的文件所在路径
export PATH=$PATH:$RISV_GCC
export PATH=$PATH:$RISV_OPENOCD
```

## 3.迁移MRS文件

1. 在虚拟机的MRS上对工程文件进行一次成功编译，会发现在工程目录下多了一个`obj`文件夹，其内包含了`makefile`和`subdir.mk`文件，这是在Clion中配置环境的关键

2. 进入CLion中打开该工程文件，随意打开一个`subdir.mk`，此时的`C_SRCS`及最下面的可执行`.o`文件的文件路径均为虚拟机文件路径，用全局替换将路径替换为Mac下的工程路径

   > 注意`/`和`\`的区别！Windows下MRS生成的文件路径可能夹杂`/`和`\`，`.o`文件则全为`\`，全局替换会比较麻烦。
   >
   > 一个可行的方法是在`obj`文件夹内全局将`\`替换为`/`，随后全局将` /`替换为` \`(注意区分这里！第二次替换时两个斜杠前面都有一个空格！！)

![image-20230404212650886](https://img-blog.csdnimg.cn/img_convert/459ca6b4479dc38e94c968be040bf6e2.png)

## 4.编译烧录

1. 在Clion中编辑配置，新建一个`Makefile Target`

  ![image-20230421163947620](https://img-blog.csdnimg.cn/img_convert/aac50a89ffae42a7729af5ef5948b6b0.png)

2. 如图选择并填写，将`Working Directory`改为`${你的工程路径}/obj`，`makefile`选择该文件夹下的`makefile`文件

   ![image-20230421164027234](https://img-blog.csdnimg.cn/img_convert/2461691f85cf09326cc4b80cdc063a36.png)

   ![image-20230421164116532](https://img-blog.csdnimg.cn/img_convert/56537df0bc13277dd26c765a8eeb5a54.png)

   > `make clean`可以用于清理所有编译的文件，运行一次`make clean`再运行`make all`可达到重新编译的效果

3. 再新建一个配置，选择`OpenOCD Download & Run`，如图进行填写

   ![image-20230421164135866](https://img-blog.csdnimg.cn/img_convert/9c7feb4954677f77c2e73af49c1e2927.png)

   > `Board config file`选择上文`openocd/bin`内的`wch-riscv.cfg`
   >
   > 如果`Debug`失败，可以将`Debugger`改上文中`xpack-riscv-none-embed-gcc-8.2.0/bin/`内的`riscv-none-embed-gdb`

4. 在`Before launch`内增加运行其他配置，选择前文添加的`make all`，这样便会在运行这个配置时自动进行一次编译。

   ![image-20230404214107397](https://img-blog.csdnimg.cn/img_convert/691f4b04bceb3bc3811272314ad895d6.png)

5. 然后就可以通过`运行`和 `调试`进行下载调试了！

   ![image-20230404214301385](https://img-blog.csdnimg.cn/img_convert/7aab0f85860a246f6399a12130f942f5.png)

## 5.注意事项

1. 在Mac上想要新增文件会比较麻烦，因为需要手动向`subdir.mk`文件增加内容，不过只需要仿照文件内原有的内容扩写即可。
2. 配置完成后就尽量不要再去用mrs进行编译了，因为会覆盖`subdir.mk`文件导致需要重新全局替换文件路径。
