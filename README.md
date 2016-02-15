# vimconfig_bundle
vim config for linux devices driver development 
配置文件是隐藏文件

# 一、配置前注意：
	1)在配置之前，确保已经安装好vim,
	  	即之前做过sudo apt-get install vim 这样的操作,如果没有请先安装vim
	  	
       安装vim+ctags+cscope+taglist
		sudo apt-get install vim ctags cscope

	2)执行脚本前，注意自己的~/.bashrc文件尾部是否添加过java配置等方面内容。如果有，
	  在执行完sudo ./config.sh后，在执行脚本备份的~/.bakvim/.bashrc中将其追加到新的~/.bashrc尾部即可。
	  
	3)160125更新：
	  	增加bundle管理插件，执行脚本前确保联网。bundle用于插件管理，使用bundle安装新的插件。
	  	可利用bundle安装vim-gitgutter插件，gitv插件。	  
	  	vim-gitgutter 可用于查看自己修改的地方。
	  	

# 二、配置步骤：

1)进入vimconfig_bundle/目录。（脚本中会利用目录下到config.sh获取用户名和用户组）

2)输入sudo ./config.sh 即可自动完成配置。

3)打开vim，利用vundle安装几个插件
	底行模式命令：
		:BundleList 查看要安装的插件
		:BundleInstall 安装插件
	  
配置中，已将vim映射为来vi，用vi打开即等同于vim打开来。

	测试：
        vi a.c
        输入main后，按tab键看是否成功自动补全。

# 三、vim使用说明
151208

## 1、插件及功能列表
    TagList
    NERDTree
    MRU
    LookupFile
	bufexplorer
    用vimgrep搜索光标所在的单词
    生成tags
    生成 filename tags
    生成cscope的数据库
    vimdiff

## 2、快捷键说明 
vim打开在源码目录打开文件后
普通模式下：
F2    打开TagList
F3    打开NERDTree
F4    打开MRU
F5    打开LookupFile
F6    用vimgrep搜索光标所在的单词
F7    生成tags
F8    生成 filename tags
F9    生成cscope的数据库
F10  
F12		实现递归查找上级目录中的ctags和cscope并自动载入，向上查找包含当前目录在内的5级目录，


说明：
		修改源码时，自动补全依赖于tags，需要在源码kernel/uboot目录下分别生成tags文件；

## 3、tags和cscope库文件的生成
方法一：
	在要生成库的目录下打开3个vim，普通模式下，分别按F7、F8、F9 ；等待生成结束即可
 
方法二：
   kernel/下只生成与arm架构有关的 :（kernel中建议用方法二）
   	1)生成tags文件： 
   		make tags ARCH=arm
   	
   	2)生成cscope的库 
   		make cscope ARCH=arm
   		
  u-boot64/下生成tags
   	ctags -R
		cscope -Rbq
		
注意：
		在生成tags和cscope前已打开的文件不能跟踪代码，重新打开即可；
		
## 4、tags和cscope使用方法
### ctags用法：
用于跟踪源码
	跟踪代码：Ctrl+]
	回退：	Ctrl+t
	
### cscoe用法：
1)普通模式下，光标在要查找的符号上，快速按下以下对应快捷键。
	,sc	 查找调用本函数的函数
	,sd	 查找本函数调用的函数
	,se	 查找egrep模式，相当于egrep功能，但查找速度快多了
	,sf	 查找并打开文件，类似vim的find功能
	,sg	 查找函数、宏、枚举等定义的位置，类似c tags的功能
	,si	 查找包含本文件的文件
	,ss	 查找C语言符号，即查找函数名、宏、枚举值等出现的地方
	,st	 查找指定的字符串

注意：
	按的慢，如按下,s停顿后，会删除一个字符进入插入模式，只依次需按Esc u即可恢复（回到普通模式，撤销）。

回退按：
		Ctrl+t
		
## 5、TagList
	基于ctags,分割窗口显示当前的代码结构概览		

### 底行模式打开:
	:TlistOpen   打开并将焦点置于标签列表窗口
	:TlistClose  关闭标签列表窗口
	:TlistToggle 切换标签列表窗口状态（打开--关闭），标签列表窗口是否获得焦点取决于其他配置

### 在TagList窗口操作：
	回车键： 跳到光标所在标记的定义处
	o: 新建一个水平分割窗口（上部），跳到标记定义处
	p: 预览标记定义（焦点仍然在taglist窗口）
	空格: 在底行显示标记的原型（如函数原型）
	u: 更新标记列表（比如源文件新增一个函数，保存后，在taglist窗口按u）
	d: 删除光标所在的taglist文件
	x: 放大/缩小taglist窗口
	[[: 将光标移到前一个文件的起点
	]]: 将光标移到后一个文件的起点
	+: 展开标记
	-: 折叠
	*: 全部展开
	=: 全部折叠
	s: 选择排序字段
	q: 退出taglist窗口

 
## 6、NERDTree --用于文件浏览
列出当前路径的目录树。
浏览项目的总体目录结构和创建删除重命名文件或文件名。
内核中_defconfig  .mk等文件可用nerd tree 打开

在NERDTree中选中目录，按ma，新建文件或者目录
o       在已有窗口中打开文件、目录或书签，并跳到该窗口
go      在已有窗口中打开文件、目录或书签，但不跳到该窗口
t       在新 Tab 中打开选中文件/书签，并跳到新 Tab
T       在新 Tab 中打开选中文件/书签，但不跳到新 Tab
i       split 一个新窗口打开选中文件，并跳到该窗口
gi      split 一个新窗口打开选中文件，但不跳到该窗口
s       vsplit 一个新窗口打开选中文件，并跳到该窗口
gs      vsplit 一个新 窗口打开选中文件，但不跳到该窗口
!       执行当前文件
O       递归打开选中结点下的所有目录
x       合拢选中结点的父目录
X       递归合拢选中结点下的所有目录
e       Edit the current dif

双击    相当于 NERDTree-o
中键    对文件相当于 NERDTree-i，对目录相当于 NERDTree-e

D       删除当前书签

P       跳到根结点
p       跳到父结点
K       跳到当前目录下同级的第一个结点
J       跳到当前目录下同级的最后一个结点
k       跳到当前目录下同级的前一个结点
j       跳到当前目录下同级的后一个结点

C       将选中目录或选中文件的父目录设为根结点
u       将当前根结点的父目录设为根目录，并变成合拢原根结点
U       将当前根结点的父目录设为根目录，但保持展开原根结点
r       递归刷新选中目录
R       递归刷新根结点
m       显示文件系统菜单 #！！！然后根据提示进行文件的操作如新建，重命名等
cd      将 CWD 设为选中目录

I       切换是否显示隐藏文件
f       切换是否使用文件过滤器
F       切换是否显示文件
B       切换是否显示书签

q       关闭 NerdTree 窗口
?       切换是否显示 Quick Help	

## 7、MRU -- Most Recently Used 最近打开文件列表
###1) 打开一个新窗口，显示最新打开的文件列表。
    :MRU
        在该命令后加空格，然后TAB或者Ctrl+D会自动补全。
        在文件列表中选择后，
        Enter：	会在当前窗口打开，
        o：	可以在新窗口打开该文件，
        v：	可以只读打开，
        t：	会在新的tab打开。

###2) 打开符合vim正则的文件列表
    :MRU vim
        打开文件名中包含vim的文件
        	
## 8、LookupFile -- 文件搜索用
tags.fn 用于文件搜索,包含项目中所有文件名
tab键开始扫描
ctrl+o:	水平分割窗口打开

###查找文件，
        在打开的缓冲区中查找，
        按目录查找。
####(1)项目文件查找

				按”<F5>“或输入”:LookupFile“
				    在当前窗口上方打开一个lookupfile小窗口，开始输入文件名（至少4个字符）。
        文件名可以使用vim的正则表达式。

        CTRL-N ：向上选择
        CTRL-P ：向下选择
    或者用上、下光标键 在下拉列表中选择文件。
    
		选中文件后，按回车，在当前窗口中打开此文件。
		ctrl+o:	水平分割窗口打开


####(2)缓冲区查找

       同时打开许多文件。在众多buffer中切换到自己所要的文件。
        按缓冲区名字查找缓冲区，输入缓冲区的名字（可以是正则表达式 ），
				匹配的缓冲区列在下拉列表中，同时还会列出该缓冲区内文件的路径。
				
        :LUBufs
        输入缓冲区的名字，在你输入的过程中，符合条件的缓冲区就显示在下拉列表中了，选中所需缓冲区后，按回车，就会切换你所选的缓冲区。


####(3)目录浏览
        :LUWalk
        打开lookupfile窗口，输入目录。
        lookupfile会在下拉列表中列出这个目录中的所有子目录及文件供选择，如果选择了目录，就会显示这个目录下的子目录和文件；
        如果选择了文件，就在vim中打开这个文件。
        需要输入绝对路径？


LUPath和LUArgs两个功能。感兴趣的朋友读一下lookupfile的手册。	

## 9、vimgrep
在打开vim的目录下递归搜索.c 和 .h 文件

在底行模式
	:vimgrep 
QuickFix 窗口操作：

在路径和文件名符合{file}的所有文件中,查找符合{pattern}的字符串：
:vimgrep     /{pattern}/[g][j]     {file} ...
:copen    打开quickfix列表查看结果，回车打开对应文件 

	:copen    打开quickfix
	:cclose    关闭quickfix
	:cc    是在转到当前查找到的位置
	:cn    转到下一个位置
	:cp    转到前一个位置

## 10、bufexplorer
	在各个buffer 之间切换
	,be 全屏方式打开buffer列表
	,bs 水平窗口打开buffer列表
	,bv 垂直窗口打开buffer列表

	:help bufexplorer 帮助 


## 11、Man命令
查看C语言帮助文档。安装有C语言和Posix的帮助手册，
查看printf函数的帮助文档，
:Man 3 printf

## 12、vim-gitgutter 的使用(160125)
最左边的标记列:
    波浪线  ：该行相比HEAD修改过，
    红色的减号：这里删除了一行，
    绿色的+号：新增行。

(1)diff区块之间跳转，默认快捷键为 [c 和 ]c
(2)暂存和回退
    暂存 <Leader>hs ,即git add 操作；新改动会和暂存内容对比，暂存后不能回退到之前
    回退修改 <Leader>hr 
(3)查看diff的修改，<Leader>hp ,显示diff差异。
有时没反应，底行模式输入“gitg”点Tab键跟出“GitGutter”，回车执行,即可

## 13、gitv 的使用 -- gitk for vim
### 浏览模式 Brower mode 
	:Gitv
    显示当前分支的提交记录
    类似gitk 功能，左边显示提交信息，右边显示具体修改。
    退出时，回到原来的窗口
    
### File mode
	:Gitv!
		显示当前文件的修改

	:Git log
		显示提交信息

### 按键映射：
	只在gitv browser modes 模式下有效
	普通模式下:
		<cr> 回车
		q  退出
		s 竖直分割窗口，显示diff信息
		u	更新当前浏览窗口内容

## 14、vimdiff
### 解决 git merge 冲突	
	当合并时出现 merge conflicts 时:
		git mergetool
## 其他
(a)常规模式下输入 cM 清除行尾 ^M 符号
(b)启用每行超过80列的字符提示（字体变蓝并加下划线）(未启用)
(c)窗口焦点切换的映射
	 普通模式或插入模式下：
		ctrl+h    焦点移到左边窗口
		ctrl+j						下边
		ctrl+k						上边
		ctrl+l						右边

(d)插入模式下光标移动(暂时未启用，与当前窗口处于插入模式，窗口焦点切换冲突)：
	光标向上移动 ctrl+K
	光标向下移动 ctrl+J
	光标向左移动 ctrl+H (不生效，哪里映射成了backspace)
	光标向右移动 ctrl+L

(e).bashrc 中更改命令提示行颜色
