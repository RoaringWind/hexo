---
title: missing-semester-data-wrangling
date: 2022-05-17 14:24:44
tags: [sed,data-processing]
---

[sed](https://www.gnu.org/software/sed/): stream editor is a non-interactive command-line text editor.

```shell
echo 'abcaba' | sed -E 's/(ab)*//g' # ca. -E 表示现代版本的正则表达式，不加可能会出错。
```

```shell
ssh myserver 'journalctl | grep sshd | grep "Disconnected from"' | less # 使用''包括命令，使得命令可在远端执行后再返回，节省带宽。
```

一个学习正则表达式的网站：[RegexOne](https://regexone.com/lesson/misc_meta_characters?)

一个正则表达式解析的网站：[Regex101](https://regex101.com/)
