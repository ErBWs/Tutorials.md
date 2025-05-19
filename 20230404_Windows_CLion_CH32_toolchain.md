## 1.准备工作

- 准备好CLion

- 从[MRS](http://www.mounriver.com/download)官网下载好MounRiverStudio

  ![image-20230404215125940](https://img-blog.csdnimg.cn/img_convert/e6cf0762f780c1cbf67136180b56afa6.png)

## 2.环境配置

**（该教程基于Windows11，Windows10也可以类似进行配置）**

1. 打开设置，寻找到`系统/系统信息/高级系统设置`，打开这个界面

   ![image-20230421165620682](https://img-blog.csdnimg.cn/img_convert/b732128f08370e7e5f810338ab4df8e3.png)

2. 打开`环境变量`，找到`系统变量`的`Path`，双击打开进行编辑

   ![image-20230421165814941](https://img-blog.csdnimg.cn/img_convert/c20c4884055e1c6f2c481dc874cc0068.png)

3. 找到MounRiver Studio软件路径下的这两个文件夹，将其路径加入环境变量中，保存并重启电脑

```
C:\MounRiver\MounRiver_Studio\toolchain\OpenOCD\bin
C:\MounRiver\MounRiver_Studio\toolchain\RISC-V Embedded GCC\bin
```

## 3.CLion设置

**（界面是macOS下的CLion，Windows下没有区别）**

1. 如图找到Clion设置中的`Make`

   ![image-20230421181935689](https://img-blog.csdnimg.cn/img_convert/d14ec04c0bf765e7b943c4ed883aee1f.png)

2. 将`Make executable`路径改为MounRiver Studio自带的`make.exe`，其路径地址为：

   `C:\MounRiver\MounRiver_Studio\toolchain\Build Tools\bin\make.exe`

3. 保存并退出设置

## 4.编译烧录

1. 使用MounRiver Studio对工程进行一次成功编译，会发现在工程目录下多了一个`obj`文件夹，其内包含了`makefile`和`subdir.mk`文件，这是在Clion中配置的关键

2. 进入CLion中打开该工程文件

3. 在Clion中编辑配置，新建一个`Makefile Target`

   ![image-20230421163947620](https://img-blog.csdnimg.cn/img_convert/cd5687f034378d34d454469594a6977c.png)

4. 如图选择并填写，将`Working Directory`改为`${你的工程路径}/obj`，`makefile`选择该文件夹下的`makefile`文件

   ![image-20230421164027234](https://img-blog.csdnimg.cn/img_convert/3131faff42b9dacf3c168f5bc3bf91eb.png)

   ![image-20230421164116532](https://img-blog.csdnimg.cn/img_convert/422ea08df1b395ce941fd54e06efcf0d.png)

   > `make clean`可以用于清理所有编译的文件，运行一次`make clean`再运行`make all`可达到重新编译的效果

5. 再新建一个配置，选择`OpenOCD Download & Run`，如图进行填写

   ![image-20230421164135866](https://img-blog.csdnimg.cn/img_convert/718058cb317bc2353fedf41cdae99904.png)

   > `Board config file`选择MounRiver Studio安装路径内如下所示的`.cfg`文件：
   >
   > `C:\MounRiver\MounRiver_Studio\toolchain\OpenOCD\bin\wch-riscv.cfg`
   >
   > 将`Debugger`改为MounRiver Studio安装路径内如下所示的`gdb.exe`文件：
   >
   > `C:\MounRiver\MounRiver_Studio\toolchain\RISC-V Embedded GCC\bin\riscv-none-embed-gdb.exe`

6. 在`Before launch`内增加运行其他配置，选择前文添加的`make all`，这样便会在运行这个配置时自动进行一次编译。

   ![image-20230404214107397](https://img-blog.csdnimg.cn/img_convert/050977f62dffd80d41c246230ac93f5e.png)

7. 然后就可以通过`运行`和 `调试`进行下载调试了！

   ![image-20230404214301385](https://img-blog.csdnimg.cn/img_convert/45ab621af2122e5f56eee622523d7048.png)

## 5.注意事项

1. 在Windows上想要新增文件只需要再加入新文件后，用MounRiver Studio打开这个工程重新进行一次成功的编译即可获得更新后的`makefile`和`subdir.mk`文件，便可以继续用Clion进行代码编写烧录了。
