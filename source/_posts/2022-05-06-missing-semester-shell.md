---
title: missing-semester-shell
date: 2022-05-06 22:39:17
tags: [missing-semester,bash,shell]
---
```shell
foo=bar #不能写成foo = bar，空格很重要
echo "foo is $foo"  # 输出 foo is bar
echo 'foo is $foo'  # 输出 foo is $foo
foo=$(pwd)# 获取当前工作目录放到foo变量内
cat <(ls) <(ls ..) #把当前目录的文件和上级目录的文件列表拼接起来，先左后右
```

```bash mcd.sh
mcd (){ #mcd.sh
	mkdir -p "$1"
	cd "$1"
}
```

可以通过```source mcd.sh```在shell里执行脚本，加载```mcd```函数
bash的特殊变量见此：https://www.tldp.org/LDP/abs/html/special-chars.html

常用的如下

- `#!` - shebang，指定解释器，让shell知道如何运行这个脚本
- `$0` - 脚本名
- `$1` 到 `$9` - 脚本的参数。 `$1` 是第一个参数，依此类推。
- `$@` - 所有参数
- `$#` - 参数个数
- `$?` - 前一个命令的返回值，如0为成功。
- `$$` - 当前脚本的进程识别码
- `!!` - 完整的上一条命令，包括参数。常见应用：当你因为权限不足执行命令失败时，可以使用 `sudo !!`再尝试一次。
- `$_` - 上一条命令的最后一个参数。如果你正在使用的是交互式 shell，你可以通过按下 `Esc` 之后键入 . 来获取这个值。
- `$(command)`和``command` ` - 命令替换
- `$(( ))` - 整数运算，进制转换
- `$varible` - 表示变量，一般与`${varible}`相同，但是后者更精确。例如：
```shell
a=test;
echo $ab #输出变量AB
echo ${a}b #输出testb
```



zsh和bash真的有好多细节之处不一样，例如`history x`命令，在zsh中是显示第5条开始的所有历史记录，而bash则是倒序前5条记录。

此外，假设在文件系统中，目前有`a.txt`而没有`b.txt`，分别在zsh和bash中执行同样命令

```bash
ls a.txt b.txt> file.out 1>&2
cat file.out
```

结果如下

| SHELL |                           屏幕结果                           |                       file.out文件内容                       |
| :---: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|  zsh  | ![zsh-屏幕输出](https://s2.loli.net/2022/05/09/rNCcShdpaWiVz7o.png) | ![zsh-file.out](https://s2.loli.net/2022/05/09/MrXlizxh4YLDpbU.png) |
| bash  | ![bash-屏幕输出](https://s2.loli.net/2022/05/09/G4AkaImjqVULPW3.png) | ![bash-file.out](C:/Users/blood/AppData/Roaming/Typora/typora-user-images/image-20220509224705892.png) |

会发现在zsh中执行后，文件`file.out`中有标准输出，而bash则什么都没有。

可以发现，bash的重定向规则为：确定了每个输出流的目的地后再执行。如下所示：

```bash
ls a.txt b.txt 2>file2.out 2>&1  # 此处&符号为取输出流1的地址，即屏幕
# 屏幕输出
# ls: cannot access b.txt: No such file or directory
# a.txt
cat file2.out # 什么都没有

ls a.txt b.txt 2>file2.out 2>&1 2>file2.out
# 屏幕输出
# a.txt
cat file2.out
# ls: cannot access b.txt: No such file or directory

ls a.txt b.txt 2>file2.out 2>&1 1>file3.out # 取输出流1的地址，取到屏幕的地址，所以输出流2最后输出到屏幕上

```

而zsh则不然，同一个输出流可以输出到多个文件/屏幕，或者说计算到哪里就执行到哪里，如下所示。

```shell
ls a.txt b.txt 2>file2.out 2>&1
# 屏幕输出
# a.txt
# ls: cannot access b.txt: No such file or directory
cat file2.out # ls: cannot access b.txt: No such file or directory

ls a.txt b.txt 2>file2.out 2>&1 2>file3.out 
# 屏幕输出
# a.txt
# ls: cannot access b.txt: No such file or directory
cat file2.out file3.out # 都是ls: cannot access b.txt: No such file or directory

ls a.txt b.txt 2>file2.out 2>&1 1>file3.out
# 屏幕输出
# ls: cannot access b.txt: No such file or directory
cat file2.out # ls: cannot access b.txt: No such file or directory
cat file3.out # a.txt
```



先赋值一个变量为一个路径，如下：

```shell
file=/dir1/dir2/dir3/my.file.txt
```

对此变量执行以下命令，


|      命令      |                           解释                            | 结果                         |
| :------------: | :-------------------------------------------------------: | ---------------------------- |
|  `${file#*/}`  | 拿掉第一条`/` **及其左边**的字符串，`/`也可以是别的字符串 | `dir1/dir2/dir3/my.file.txt` |
| `${file##*/}`  |           拿掉最后一条 `/` **及其左边**的字符串           | `my.file.txt`                |
|  `${file%/*}`  |           拿掉最后一条 `/` **及其右边**的字符串           | `/dir1/dir2/dir3`            |
| `${file%%/*}`  |            拿掉第一条 `/` **及其右边**的字符串            | 空值                         |
| `${file:10:5}` |            提取第 10个字节右边的连续 5 个字节             | `dir3/`                      |

其余可参考https://www.cnblogs.com/chengd/p/7803664.html

下面代码中，`-ne`用来表示not equal，还有[其他比较符号](https://man7.org/linux/man-pages/man1/test.1.html)可供使用。zsh使用`[[]]`计算表达式。

```bash
#!/bin/bash
for file in "$@";do
    grep foobar "$file" > grepout 2> greperr # 注意，此处如果没有在文件中找到字符串，err信息为空。“没有文件”等错误才会显示err信息
    t=$?
    if [[ "$t" -ne 0 ]];then
        echo "$file dont have foobar,errcode = $t"
    fi
done
```

bash、zsh可以使用`{}`作为展开式，可以在一条命令里使用多个展开式，会做笛卡尔积。

在使用zsh展开式之前，可能需要`setopt BRACE_CCL `来激活，而且可能需要使用`{a-k}`而非`{a..k}`的形式。可见：https://stackoverflow.com/questions/2394728/zsh-brace-expansion-seq-for-character-lists-how

```shell
convert img.{png,jpg} # 等于convert img.png img.jpg
touch foo{,1} # 等于 touch foo,foo1
touch foo{a..c} # 等于 touch fooa foob fooc
diff <(ls dir1) <(ls dir2) # 比较目录的不同
```

可以用`shellcheck`命令检查shell脚本

在 `shebang` 行中使用 [`env`](https://man7.org/linux/man-pages/man1/env.1.html) 命令，会利用环境变量中的程序来解析该脚本，提高了脚本的可移植性。`env` 会利用`PATH` 环境变量来进行定位。 使用了`env`的shebang可以如此：`#!/usr/bin/env python`

shell函数和脚本有如下一些不同点：

- 函数只能与shell使用相同的语言，脚本可以使用任意语言。因此在脚本中包含 `shebang` 是很重要的。
- 函数仅在定义时被加载，脚本会在每次被执行时加载。这让函数的加载比脚本略快一些，但每次修改函数定义，都要重新加载一次。
- 函数会在当前的shell环境中执行，脚本会在单独的进程中执行。因此，函数可以对环境变量进行更改，比如改变当前工作目录，脚本则不行。脚本需要使用 [`export`](httsp://man7.org/linux/man-pages/man1/export.1p.html) 将环境变量导出，并将值传递给环境变量。
- 与其他程序语言一样，函数可以提高代码模块性、代码复用性并创建清晰性的结构。shell脚本中往往也会包含它们自己的函数定义。

```shell
# 查找所有名称为src的文件夹，d是目录，f是文件
find . -name src -type d

# 查找所有文件夹路径中包含test的python文件
find . -path '*/test/*.py' -type f #也可以找出当前目录下的，因为*包括了.（即当前目录），因查找内容中包含/，所以要用path或者wholename作为flag

# 查找前一天修改的文件
find . -mtime -1
# 前60分钟修改的文件
find . -mmin 60

# 查找所有大小在500k至10M的tar.gz文件
find . -size +500k -size -10M -name '*.tar.gz'

# 删除全部扩展名为.tmp 的文件
find . -name '*.tmp' -exec rm {} \; #考虑到不同系统的分号意义不同，所以最后加上了\
# 查找全部的 PNG 文件并将其转换为 JPG
find . -name '*.png' -exec convert {} {}.jpg \;
```

[`fd`](https://github.com/sharkdp/fd) 就是一个更简单、更快速、更友好的程序，它可以用来作为`find`的替代品。它有很多不错的默认设置，例如输出着色、默认支持正则匹配、支持unicode，而且其语法更符合直觉。



locate命令：和find类似，但是建立索引、使用索引，会更快，使用前需要使用`updatedb`更新，一般每日会自动更新。

tldr: 简洁版的man

`grep` 有很多选项，例如有 `-C` ：获取查找结果的上下文（Context）；`-v` 将对结果进行反选（Invert），也就是输出不匹配的结果。举例来说， `grep -C 5` 会输出匹配结果前后五行。当需要搜索大量文件的时候，使用 `-R` 会递归地进入子目录并搜索所有的文本文件。

但是，我们有很多办法可以对 `grep -R` 进行改进，例如使其忽略`.git` 文件夹，使用多CPU等等。

因此也出现了很多它的替代品，包括 [ack](https://beyondgrep.com/), [ag](https://github.com/ggreer/the_silver_searcher) 和 [rg](https://github.com/BurntSushi/ripgrep)。它们都特别好用，但是功能也都差不多，我比较常用的是 ripgrep (`rg`) ，因为它速度快，而且用法非常符合直觉。例子如下：

```shell
# 在当前目录递归查找
grep -R foobar .
# 查找所有使用了 requests 库的文件
rg -t py 'import requests'
# 查找所有没有写 shebang 的文件（包含隐藏文件），-u表示dont ignore hidden files
rg -u --files-without-match "^#!"
# 查找所有的foo字符串，并打印其之后的5行
rg foo -A 5
# 打印匹配的统计信息（匹配的行和文件的数量）
rg --stats PATTERN
```

alias: 别名

注意：`zsh`的`man`手册和`bash`的`man`手册有所区别，例如`history 1`在`zsh`中为从第一行开始的历史记录，而在`bash`中返回最近的一条记录。

同一命令在不同shell里面的表现形式可能不同，例如上述的history，是shell builtin命令，因此不同shell例如zsh和bash的实现就不同。

可以使用`type`区分一条命令是shell builtin还是用户命令。

可参考[Shell 的内置（builtin）命令是什么，常常傻傻分不清](https://tinylab.org/shell-builtin-command/)一文

使用`man zshbuiltins`查看zsh内置shell命令。

参照[此文](https://stackoverflow.com/questions/4405382/how-can-i-read-documentation-about-built-in-zsh-commands)设置`.zshrc`文件，完成后`source`加载。可在`oh my zsh`环境下使用，输入命令后使用`alt+h`即可查询。或者使用`help command`在zsh的手册中查询。和`man`一样，可以使用`/text`查询`text`文本在手册中的位置，找到后分别使用`n`和`N`定位下一个和上一个。

```sh
unalias run-help 2>/dev/null
autoload run-help
HELPDIR=/path/to/zsh_help_directory 
# /usr/local/share/zsh/help on OSX 暂未尝试
# /usr/share/zsh/5.0.2/functions/run-help on CentOS 7 可用
alias help=run-help
```

`Ctrl+R`可在交互式shell里查询历史命令。敲 `Ctrl+R` 后您可以输入子串来进行匹配，查找历史命令行。反复按下就会在所有搜索结果中循环。在 [zsh](https://github.com/zsh-users/zsh-history-substring-search) 中，使用方向键上或下也可以完成这项工作。`Ctrl+R` 可以配合 [fzf](https://github.com/junegunn/fzf/wiki/Configuring-shell-key-bindings#ctrl-r) 使用。`fzf` 是一个通用对模糊查找工具，它可以和很多命令一起使用。这里我们可以对历史命令进行模糊查找并将结果以赏心悦目的格式输出。

你可以修改 shell history 的行为，例如，如果在命令的开头加上一个空格，它就不会被加进shell记录中。当你输入包含密码或是其他敏感信息的命令时会用到这一特性。 为此你需要在`.bashrc`中添加`HISTCONTROL=ignorespace`或者向`.zshrc` 添加 `setopt HIST_IGNORE_SPACE`。 如果你不小心忘了在前面加空格，可以通过编辑。`bash_history`或 `.zhistory` 来手动地从历史记录中移除那一项。



`tar`压缩命令，找到所有html文件并且压缩成zip。

```shell
# 建立数据
mkdir html_root
cd html_root
touch {1..10}.html
mkdir html
cd html
touch xxxx.html
cd ../..

#for MacOS
find html_root -name "*.html" -print0 | xargs -0 tar vcf html.zip
#for Linux
find . -type f -name "*.html" | xargs -d '\n'  tar -cvzf html.zip #c表示create新文件，z表示支持gzip，f表示压缩完后文件的名字一般必填，v表示显示指令执行过程。f指令必须在最后，用于指定压缩文件名。
```

```shell
# bash
foo=bar # 定义shell变量foo
export foo # 导出shell变量foo到环境变量
unset $foo # 删除shell变量bar，因为$foo的值是bar
unset foo # 删除shell变量foo
export -n # 删除环境变量foo，如果是函数，flag就为-fn

# zsh
unset foo # 会把shell变量foo和环境变量foo都删除
```





## 文件夹导航

之前对所有操作我们都默认一个前提，即您已经位于想要执行命令的目录下，但是如何才能高效地在目录 间随意切换呢？有很多简便的方法可以做到，比如设置alias，使用 [ln -s](https://man7.org/linux/man-pages/man1/ln.1.html) 创建符号连接等。而开发者们已经想到了很多更为精妙的解决方案。

由于本课程的目的是尽可能对你的日常习惯进行优化。因此，我们可以使用[`fasd`](https://github.com/clvv/fasd)和 [autojump](https://github.com/wting/autojump) 这两个工具来查找最常用或最近使用的文件和目录。

Fasd 基于 [*frecency* ](https://developer.mozilla.org/en-US/docs/Mozilla/Tech/Places/Frecency_algorithm)对文件和文件排序，也就是说它会同时针对频率（*frequency*）和时效（*recency*）进行排序。默认情况下，`fasd`使用命令 `z` 帮助我们快速切换到最常访问的目录。例如， 如果您经常访问`/home/user/files/cool_project` 目录，那么可以直接使用 `z cool` 跳转到该目录。对于 autojump，则使用`j cool`代替即可。

还有一些更复杂的工具可以用来概览目录结构，例如 [`tree`](https://linux.die.net/man/1/tree), [`broot`](https://github.com/Canop/broot) 或更加完整的文件管理器，例如 [`nnn`](https://github.com/jarun/nnn) 或 [`ranger`](https://github.com/ranger/ranger)。
