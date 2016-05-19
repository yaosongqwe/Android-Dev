# 环境准备

## 安装 Ubuntu
我使用的是ubuntu 麒麟 64位
对应的下载地址是
http://cdimage.ubuntu.com/ubuntukylin/releases/14.04/release/

一开始安装了32位15.10 Ubuntu . 编译时报错:
>build/core/envsetup.mk:89: *** Unable to determine HOST_ARCH from uname -sm: Linux i686!。

发现是系统版本问题.乖乖换成64位

## 安装 jdk8
### JDK 版本选择
关于 jdk 版本需要注意一下. 编译不同版本的源码需要安装对应的 JDK(https://source.android.com/source/requirements.html#jdk),
参照下表:

源码版本| JDK 版本
---|---
The master branch of Android in AOSP | OpenJDK 8
Android 5.x (Lollipop) - Android 6.0 (Marshmallow) | OpenJDK 7
Android 2.3.x (Gingerbread) - Android 4.4.x (KitKat) | Java JDK 6
Android 1.5 (Cupcake) - Android 2.2.x (Froyo) | Java JDK 5

### Ubuntu 14.04 安装 OpenJDK 8的方法:

对应的命令:

```
sudo add-apt-repository ppa:openjdk-r/ppa
sudo apt-get update
sudo apt-get install openjdk-8-jdk
```
安装成功后需要配置一下:

```
sudo update-alternatives --config java
sudo update-alternatives --config javac
```
选择最新安装的 openjdk 8.


参照链接:

http://ubuntuhandbook.org/index.php/2015/01/install-openjdk-8-ubuntu-14-04-12-04-lts/

## 安装 git, curl, repo
#### git 安装
```
sudo apt-add-repository ppa:git-core/ppa
sudo apt-get update
sudo apt-get install git
```

git 安装完成后需要配置:
```
git config --global user.name <your name>
git config --global user.email <your email>
```

#### 安装 curl
```
sudo apt-get install curl
```

#### 安装 repo
```
cd ~
mkdir ~/bin
PATH=~/bin:$PATH
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
```

# 同步源码
我使用的是清华大学TUNA镜像源 ( 感谢 )
https://mirrors.tuna.tsinghua.edu.cn/help/AOSP/

网速ok的 ( 或者想不开的 ) 可以直接从google 同步
https://source.android.com/source/downloading.html

## 建立工作目录
```
mkdir WORKING_DIRECTORY
cd WORKING_DIRECTORY
```
## 初始化仓库
```
repo init -u https://aosp.tuna.tsinghua.edu.cn/platform/manifest
```
如果需要指定特定的源码版本, 使用下面的命令:

```
repo init -u https://aosp.tuna.tsinghua.edu.cn/platform/manifest -b android-4.0.1_r1

```
版本列表参照:

https://source.android.com/source/build-numbers.html#source-code-tags-and-builds

>我出现了无法连接的情况, 安装提示把 ~/bin/repo 的 REPO_URL 一行替换成:
REPO_URL = 'https://gerrit-google.tuna.tsinghua.edu.cn/git-repo' 使用命令  "gedit ~/bin/repo " 可以打开编辑器, 替换后保存即可.



## 同步源码树
```
repo sync
```

执行到一半中断了, 只需要重新执行 repo sync 即可.


>Syncing work tree : 100%

当出现项目的提示时,说明同步完成.


# 编译
拼 RP 的时候到了

在终端输入以下命令开始编译
```
source build/envsetup.sh
lunch
选择一个, 我只是需要看下源码, 选择第一个即可
make -j16
```

编译过程中出现了OOM:
>Out of memory error (version 1.2-rc2 'Carnac' (297700 b8d58d63a1bd8b9e35b8bbb502a0b4b2ff5a3f5b by android-jack-team@google.com)). GC overhead limit exceeded. Try increasing heap size with java option '-Xmx<size>'. Warning: This may have produced partial or corrupted output. [ 84% 26777/31631] Building with Jack:...ES/hamcrest_intermediates/classes.jack ninja: build stopped: subcommand failed. make: *** [ninja_wrapper] 错误 1

解决办法:

https://liuzhichao.com/2016/osx-download-and-build-android-source.html

按照链接中的提示操作
```
export JVM_ARGS="-Xmx4096m -XX:MaxPermSize=1024m"
```

重新开始编译.


# 运行

运行 emulator 启动

出现错误: emulator 命令未找到
# 环境准备

## 安装 Ubuntu
我使用的是ubuntu 麒麟 64位
对应的下载地址是
http://cdimage.ubuntu.com/ubuntukylin/releases/14.04/release/

一开始安装了32位15.10 Ubuntu . 编译时报错:
>build/core/envsetup.mk:89: *** Unable to determine HOST_ARCH from uname -sm: Linux i686!。

发现是系统版本问题.乖乖换成64位

## 安装 jdk8
### JDK 版本选择
关于 jdk 版本需要注意一下. 编译不同版本的源码需要安装对应的 JDK(https://source.android.com/source/requirements.html#jdk),
参照下表:

源码版本| JDK 版本
---|---
The master branch of Android in AOSP | OpenJDK 8
Android 5.x (Lollipop) - Android 6.0 (Marshmallow) | OpenJDK 7
Android 2.3.x (Gingerbread) - Android 4.4.x (KitKat) | Java JDK 6
Android 1.5 (Cupcake) - Android 2.2.x (Froyo) | Java JDK 5

### Ubuntu 14.04 安装 OpenJDK 8的方法:

对应的命令:

```
sudo add-apt-repository ppa:openjdk-r/ppa
sudo apt-get update
sudo apt-get install openjdk-8-jdk
```
安装成功后需要配置一下:

```
sudo update-alternatives --config java
sudo update-alternatives --config javac
```
选择最新安装的 openjdk 8.


参照链接:

http://ubuntuhandbook.org/index.php/2015/01/install-openjdk-8-ubuntu-14-04-12-04-lts/

## 安装 git, curl, repo
#### git 安装
```
sudo apt-add-repository ppa:git-core/ppa
sudo apt-get update
sudo apt-get install git
```

git 安装完成后需要配置:
```
git config --global user.name <your name>
git config --global user.email <your email>
```

#### 安装 curl
```
sudo apt-get install curl
```

#### 安装 repo
```
cd ~
mkdir ~/bin
PATH=~/bin:$PATH
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
```

# 同步源码
我使用的是清华大学TUNA镜像源 ( 感谢 )
https://mirrors.tuna.tsinghua.edu.cn/help/AOSP/

网速ok的 ( 或者想不开的 ) 可以直接从google 同步
https://source.android.com/source/downloading.html

## 建立工作目录
```
mkdir WORKING_DIRECTORY
cd WORKING_DIRECTORY
```
## 初始化仓库
```
repo init -u https://aosp.tuna.tsinghua.edu.cn/platform/manifest
```
如果需要指定特定的源码版本, 使用下面的命令:

```
repo init -u https://aosp.tuna.tsinghua.edu.cn/platform/manifest -b android-4.0.1_r1

```
版本列表参照:

https://source.android.com/source/build-numbers.html#source-code-tags-and-builds

>我出现了无法连接的情况, 安装提示把 ~/bin/repo 的 REPO_URL 一行替换成:
REPO_URL = 'https://gerrit-google.tuna.tsinghua.edu.cn/git-repo' 使用命令  "gedit ~/bin/repo " 可以打开编辑器, 替换后保存即可.



## 同步源码树
```
repo sync
```

执行到一半中断了, 只需要重新执行 repo sync 即可.


>Syncing work tree : 100%

当出现项目的提示时,说明同步完成.


# 编译
拼 RP 的时候到了

在终端输入以下命令开始编译
```
source build/envsetup.sh
lunch
选择一个, 我只是需要看下源码, 选择第一个即可
make -j16
```

编译过程中出现了OOM:
>Out of memory error (version 1.2-rc2 'Carnac' (297700 b8d58d63a1bd8b9e35b8bbb502a0b4b2ff5a3f5b by android-jack-team@google.com)). GC overhead limit exceeded. Try increasing heap size with java option '-Xmx<size>'. Warning: This may have produced partial or corrupted output. [ 84% 26777/31631] Building with Jack:...ES/hamcrest_intermediates/classes.jack ninja: build stopped: subcommand failed. make: *** [ninja_wrapper] 错误 1

解决办法:

https://liuzhichao.com/2016/osx-download-and-build-android-source.html

按照链接中的提示操作
```
export JVM_ARGS="-Xmx4096m -XX:MaxPermSize=1024m"
```

重新开始编译.

编译成功后会有如下提示:
```
#### make completed successfully (01:04:05 (hh:mm:ss)) ####
```

# 运行

运行 emulator 启动

出现错误: emulator 命令未找到

解决办法:
```
source build/envsetup.sh
lunch
选择之前编译的
emulator
```

