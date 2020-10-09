---
title: Master Sketching on Windows 10 18363 x64 & Ubuntu18.04.2 x64
date: 2020-06-07 15:19:50
tags: [项目,Python]
---
注意，`torch v0.4.1`似乎并不支持`python3.8`,故使用`python3.6`.`Ubuntu18.04.2`自带的python3版本就是`Python3.6`
个人配置供参考：`i5-10210U DDR4-12GB MX250-2GB`

需要对`Master Sketching`文件夹内`simplified.py`文件进行两处修改。

```Python
# simplified.py
use_cuda=0
load_lua(opt.model,long_size=8)
```

`Ubuntu`为虚拟机环境`Vmware Workstation Pro 15.5.1`, `Ubuntu 18.04.2`, `4GB RAM`, `40GB Disk`, `Cpu 2x2`核心

```Bash
# Ubuntu18.04.2 X64
sudo apt install python3-pip
pip3 install pip
pip3 install torch==0.4.1
pip3 install torchvision==0.2.1
```



`Windows10`需要安装`Anaconda`，在`https://repo.anaconda.com/archive/`内下载安装`Anaconda3-2020.02-Windows-x86_64.exe` ，之后打开`Anaconda prompt`



```Bash
# Conda,Windows10 x64
pip install pip
pip install torch==0.4.1
pip install torchvision=0.2.1
```



一切就绪后即可在对应目录下使用如下命令输出`out.png`图片啦

```Bash
#Ubuntu
python3 simplified.py
```

```Bash
#conda,Windows 10
python simplified.py
```

