---
title: missing-semester-vim
date: 2022-05-10 23:12:31
tags: [missing-semester,vim]
---

`c` - correct，修改。对于修改行来说，因为`c2`后如果接表示行首的0，会导致误解，所以在输入`c2`后敲击回车即可修改从光标处开始的两整行，并进入插入模式。

`curl -L https://missing-semester-cn.github.io/2020/files/vimrc -o ~/.vimrc` rc表示runtime configuration.

CtrlP是一个查看缓冲区的插件，以标签的形式切换缓冲文件是一个不太好的方法，所以CtrlP不支持。详见此[reddit问答](https://www.reddit.com/r/vim/comments/1hkpd1/vim_how_to_jump_to_an_open_tab_using_ctrlp/)。此thread提及，**buffer**是你正编辑的文件在内存中的一个表示，不能表明这个buffer和硬盘上的文件有无关联，切换标签的做法强制了每个tab使用一个buffer，会显得有点不太好。**window**是buffer的一个视图。tab是一组window。
