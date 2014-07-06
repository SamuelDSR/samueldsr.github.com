---
layout: post
title: "Windows下 gvim 开发环境配置"
description: "vim "
category: vim
tags: [vim插件]
---
{% include JB/setup %}
[vim](http://www.vim.org) 是一款在程序员之中非常流行的文本编辑器，和 [Emacs](http://www.gnu.org/software/emacs/) 并列为类Unix系统上用户最喜欢的编辑器。vim 虽然小巧，但是功能却异常强大，各式丰富的插件可以实现代码高亮、自动补全、编译以及错误跳转等功能，丝毫不逊于 Visual Studio、Eclipse 等集成开发环境。用好 vim 能大大提高你的编码效率。

vim 能工作在各种平台之上：windows 下有 gvim, Mac OS X下有 Macvim, 这里主要介绍 windows 下 gvim 等配置，从最基本的vim 开始，一步一步搭建一个包括 cscope, taglist, neocomplcache, snipmate 在内的功能强大的 vim 集成开发环境。

### 1.安装gvim

可以到 vim 的[官方网站](http://www.vim.org/download.php)选择对应 windows 系统的 gvim 安装程序或者可以直接点击[这里](ftp://ftp.vim.org/pub/vim/pc/gvim74.exe)。对于64位系统，一个优化但没有经过严格测试的版本 (支持>4GB 大小的文件编辑) 可以在[这里](http://vim-win3264.googlecode.com/files/vim73-x64.zip)下载。

下载完后，直接点击exe文件安装，安装目录随便，安装完后 vim 所在的目录的结构是这样的：
* &nbsp;&nbsp;`vim`
*	&nbsp;&nbsp;&nbsp;&nbsp;`vim73`
*	&nbsp;&nbsp;&nbsp;&nbsp;`vimfiles`
*	&nbsp;&nbsp;&nbsp;&nbsp;`_vimrc`

其中，`vim73` 存放 gvim 的主执行文件和一些必要的插件，`vimfiles` 里面存放我们想要安装的各种插件(推荐插件安装到 `vimfiles`, 这样方便与管理和卸载，下文中所有插件都是安装到此目录)，`_vimrc` 就是 vim 的配置文件所在，一些 vim 的基本设置，如文本编码，主题，快捷键都可以写在这里。

### 2.安装vim的中文帮助文档

vim 自带的用户手册是英文的，如果想看中文帮助，可以到这里下载 [vimcdoc-1.8.0.tar.gz](http://sourceforge.net/projects/vimcdoc/files/vimcdoc/1.8.0/), 之后将解压得到的 `doc` 文件夹复制到 `vimfiles/doc`, `vimcdoc.vim` 复制到 `vimfiles/plugin`, `help_cn.vim` 复制到 `vimfiles/syntax` (其余的文件不用管)。

当然，你可以选择直接下载编译好的 [vimcdoc-1.8.0-setup.exe](http://sourceforge.net/projects/vimcdoc/files/win32-install/1.8.0/) 然后一路点下去即可。

最后，还需要在 `_vimrc` 里面讲帮助文档的语言设为中文，即添加下面一句话：

{% highlight vim script %}
set helplang=cn
{% endhighlight %}

### 3.安装Vundle来管理插件

vim 中有很多插件管理工具，比如说 [pathogen](http://www.vim.org/scripts/script.php?script_id=2332), 这里推荐用 [Vundle](http://www.vim.org/scripts/script.php?script_id=3458), vundle跟git整合，可以方便地用git来安装，更新，卸载 vim 中的插件，你只需要知道插件的名字即可，在使用 vundle 之前，确保你的电脑上以及安装了 git (推荐直接安装 [github for windows](https://windows.github.com/)，安装好之后也顺便有了git )。

打开 git shell, `cd` 到你的 `vimfiles` 下，然后输入以下代码：
{% highlight bash %}
$ git clone https://github.com/gmarik/vundle.git ./bundle/vundle
{% endhighlight %}

这样就下载安装好了 vundle, vundle 管理的插件都安装在 `vimfiles/bundle` 之下,
并且能通过 git 来对插件进行更新，vundle 支持下面三种源：
* [vim-scripts](http://vim-scripts.org/index.html)，这种方式只需要知道插件的名字即可，例如插件 L9 只需要 Bundle 'L9' 即可
* github 中的 vim scripts 源，这种方式需要知道 github 上的 repositories 的名字，如 Bundle 'Lokaltog/vim-easymotion'
* 其他 git 源, 需要知道完整的 git 路径

在用 vundle 安装管理插件之前，首先需要在 `_vimrc` 里面配置好想要 vundle 管理的 vim 插件，我的配置如下:
{% highlight vim script %}

set nocompatible 
filetype off "必须的
"此处规定Vundle的路径
set rtp+=$VIM/vimfiles/bundle/vundle/
"此处规定插件的安装路径
call vundle#rc('$VIM/vimfiles/bundle/')

"让 Vundle 管理 Vundle "必须的
Bundle 'gmarik/vundle'

"Syntax
Bundle 'asciidoc.vim'
Bundle 'html5.vim'
Bundle 'xml.vim'

"color
Bundle 'flazz/vim-colorschemes'
Bundle 'vibrantink'

"Plugin
Bundle 'DoxygenToolkit.vim' 
Bundle 'L9'
Bundle 'FuzzyFinder'
Bundle 'snipMate'
Bundle 'Tagbar'
Bundle 'taglist.vim'
Bundle 'The-NERD-Commenter'
Bundle 'Tabular'
Bundle 'Chiel92/vim-autoformat'
Bundle 'The-NERD-tree'
Bundle 'Lokaltog/vim-easymotion'
Bundle 'minibufexpl.vim'
Bundle 'neocomplcache'
Bundle 'LargeFile'
Bundle 'LaTeX-Suite-aka-Vim-LaTeX'
Bundle 'Auto-Pairs'
Bundle 'Lokaltog/vim-powerline'
Bundle 'ShowMarks'
Bundle 'gregsexton/VimCalc'
{% endhighlight %}

然后打开 vim, 然后在命令模式下输入 `:BundleInstall`, vundle 就会自动安装上面相应的插件，Vundle 的插件都安装在 `vimfiles\bundle` 之下，并且能够很方便的通过 vundle 的命令进行管理：

* `:BundleList`: 列出所安装的所有插件列表
* `:BundleInstall`: 安装插件 `_vimrc` 里面配置的用 vundle 管理的所有插件
* `:BundleClean`: 清除不必要的插件。在 vundle 里面，如果你想要卸载某插件，只需要在 `_vimrc` 文件里面删除该插件，然后调用 `:BundleClean` 即可卸载此插件
* `BundleSearch`: 搜索相关插件，这是一个非常实用的命令，比如说你想安装 vim 里面的 Latex 插件，但是你不知道插件的详细名字，你可以通过 `:BundleSearch latex` 列出所有包含 `latex` 的插件，然后在左侧弹出的窗口里面定位到你想安装的插件，按 `i` 即可快速安装 (提醒，安装完之后一定要在 `_vimrc` 里面添加用 Bundle 管理该插件，不然你下次启动 vim 时该插件不会生效)

你可以通过 `:help vundle` 来获取更为详细的 vundle 帮助。

### 4. 配置 ctags 和 cscope
大多数集成开发环境 (IDE) 都支持跳转到某个函数或变量的定义，这在编写或阅读源代码时非常有用。vim 里面也能通过 ctags 和 cscope 来完美地实现这一功能。
#### ctags
ctags 能通过生成相应的标记文件来实现在源文件中快速定位对象，在使用 ctags 实现跳转之前，首先需要生成相应的 tags文件。ctags中支持的 tag 对象有：
* 用#define定义的宏
* 枚举型变量的值
* 函数的定义、原型和声明
* 名字空间（namespace）
* 类型定义（typedefs）
* 变量（包括定义和声明）
* 类（class）、结构（struct）、枚举类型（enum）和联合（union）
* 类、结构和联合中成员变量或函数 

首先需要下载安装ctags, 下载 [ctags58.zip](http://prdownloads.sourceforge.net/ctags/ctags58.zip), 解压后将ctags.exe拷贝至`vim/vim73` 下面，之后就可以下载 vim 中使用ctags了。

使用ctags建立tags文件的语句如下：
{% highlight bash shell script %}
ctags -R –c++-kinds=+px –fields=+iaS –extra=+q . 
{% endhighlight %}

每个参数的解释如下：

* -R:	ctags循环生成子目录的tags
* –c++-kinds=+px: ctags记录c++文件中的函数声明和各种外部和前向声明
* –fields=+iaS: ctags 要求描述的信息，其中i表示如果有继承，则标识出父类；a 表示如果元素是类成员的话，要标明其调用权限(即是public还是private); S 表示如果是函数，则标识函数的signature。
* –extra=+q: 强制要求ctags做如下操作—如果某个语法元素是类的一个成员，ctags默认会给其记录一行，可以要求ctags对同一个语法元素再记一行，这样可以保证在VIM中多个同名函数可以通过路径不同来区分。 

最方便的是在 vim 中建立ctags 的命令的快捷键，可以设定为 `<leader>bt` (build tags)
{% highlight vim script %}
nmap <silent> <leader>bt :!ctags -R --c++-kinds=+p --fields=+iaS --extra=+q .<CR>
{% endhighlight %}

习惯按功能键的话可以绑定到 `<F12>` 键
{% highlight vim script %}
nmap <F12> :!ctags -R --c++-kinds=+p --fields=+iaS --extra=+q .<CR>
{% endhighlight %}

建立好tags之后，在对应源代码你想跳转的地方按`Ctrl+]`键即可。
许多其他插件，如显示源代码结构和函数列表 taglist, 自动补全插件 omincppcomplete, neocomplcache 等都依赖ctags生成的tags文件。
另外, tags 文件必须在 vim 运行的当前目录才能正确实现跳转，如果想添加其他路径的tags文件，可以这样:
{% highlight vim script %}
set tags+=tags文件路径
{% endhighlight %}

#### cscope
cscope 是另外一种用来生成tags文件的工具，cscope主要用来协助浏览C/C++语言，功能方面要强于ctags，不仅支持变量/函数的定义查询，还记录了函数的调用处查询等功能，所以在安装了ctags之后还可以继续安装cscope
vim 默认是支持ctags生成的tags文件的，但是对于cscope而言，windows系统下的gvim可能支持不是太好，
这时你需要在vim中输入`:version`来查看自己的vim是否支持cscope, 如果有`+cscope`选项的话就没有问题，如果是`-cscope`, 你需要重新安装一个支持cscope的版本或者从 vim 源代码重新编译，加上 `-enable-cscope` 选项。
下载 windows版本的 [cscope.exe](http://sourceforge.net/projects/mslk/files/Cscope/cscope-15.7/cscope-15.7.zip/download),
将得到的zip文件解压后全部放入`vim/vim73`下面。这样就可以使用 cscope 来生成程序数据库文件了。
使用 cscope 之前需要利用 cscope 命令生成相应的程序数据库也就是 tags 文件，下面是一个方便生成数据库文件 vim 函数，将之添加到 `_vimrc` 文件中。
{% highlight vim script %}
function Do_CsTag()
    if(executable("cscope") && has("cscope") )
        if(has('win32'))
            silent! execute "!dir /b *.c,*.cpp,*.h,*.java,*.cs >> cscope.files"
        else
            silent! execute "!find . -name "*.h" -o -name "*.c" -o -name "*.cpp" -o -name "*.m" -o -name "*.mm" -o -name "*.java" -o -name "*.py" > cscope.files"
        endif
        silent! execute "!cscope -b"
        if filereadable("cscope.out")
            execute "cs add cscope.out"
        endif
    endif
endf
{% endhighlight %}

调用这个函数就可以用 cscope 生成程序数据库，然后 vim 就可以利用它来进行跳转了。
函数的代码的详细解释见 [http://www.vimer.cn/2009/10/%E5%9C%A8vimgvim%E4%B8%AD%E4%BD%BF%E7%94%A8cscope.html](http://www.vimer.cn/2009/10/%E5%9C%A8vimgvim%E4%B8%AD%E4%BD%BF%E7%94%A8cscope.html).
当然最方便的是在 vim 中映射这个函数，代码如下：

{% highlight vim script %}
nmap <leader>bct :call Do_CsTag()<CR>
{% endhighlight %}

cscope的常用生成数据库选项可以用在 vim 中命令模式下输入 `!cscope --help` 来查看，这里就不详细介绍了。

cscope的原理就介绍到这里，在 vim 中 cscope 的用法你可以在命令模式下输入 `cs` 会列出 cscope 的帮助信息，常用的是 cscope find，用法如下：

* cs find c|d|e|f|g|i|s|t name
* 0 或 s 查找本 C 符号(可以跳过注释)
* 1 或 g 查找本定义
* 2 或 d 查找本函数调用的函数
* 3 或 c 查找调用本函数的函数
* 4 或 t 查找本字符串
* 6 或 e 查找本 egrep 模式
* 7 或 f 查找本文件
* 8 或 i 找包含本文件的文件 

为了避免每次都输入这么多命令，可以在 vim 中建立映射：
{% highlight vim script %}
nmap <C-\>s :cs find s <C-R>=expand("<cword>")<CR><CR>:copen<CR>
nmap <C-\>g :cs find g <C-R>=expand("<cword>")<CR><CR>
nmap <C-\>c :cs find c <C-R>=expand("<cword>")<CR><CR>:copen<CR>
nmap <C-\>t :cs find t <C-R>=expand("<cword>")<CR><CR>:copen<CR>
nmap <C-\>e :cs find e <C-R>=expand("<cword>")<CR><CR>:copen<CR>
nmap <C-\>f :cs find f <C-R>=expand("<cfile>")<CR><CR>:copen<CR>
nmap <C-\>i :cs find i ^<C-R>=expand("<cfile>")<CR>$<CR>:copen<CR>
nmap <C-\>d :cs find d <C-R>=expand("<cword>")<CR><CR>:copen<CR>
{% endhighlight %}
或者直接下载 [cscope_maps.vim](http://cscope.sourceforge.net/cscope_maps.vim) 然后放入 `vimfiles\plugin` 之下即可。

### 参考链接
[http://blog.csdn.net/bokee/article/details/6633193](http://blog.csdn.net/bokee/article/details/6633193)

[www.vimer.cn](www.vimer.cn)
