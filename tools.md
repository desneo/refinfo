# sublime3
**快捷键**  
```
Ctrl+P  project(见插件3)中检索文件，不需通配符*
Ctrl+G  跳转指定行
Ctrl+L  选择行，重复则增加选择下一行
Ctrl+C  复制当前行
Ctrl+X  删除当前行
Ctrl+G  跳转指定行
Ctrl+Shift+方向  交换上下行
Ctrl+J  合并行

```
**插件**  
```
注： preference-->package control -->输入install
1、Emmet
2、gbk，GBK编码兼容
3、sidebarEnhancements -->左侧边栏Folders,View -> Side Bar, Project -> Add Folder to Project
4、Bracket Highlighter：匹配括号提示
5、ChineseLocalization 界面中文，切换语言，帮助(H)/Language/简体中文，繁体中文，日本语，English。
6、TrailingSpaces 尾部空格标注
```
**手动安装package Control**  
```
  1) [点击下载文件](https://sublime.wbond.net/Package%20Control.sublime-package)
  2) sublime-->preference-->Browser Packages 会打开安装目录(如C:\Users\z00316474\AppData\Roaming\Sublime Text 3\Packages)
  3) ../Installed Packages , 将1）中文件放入目录, 重启
  4) 安装成功后，Preferences菜单最下边是否有Package Settings 和Package Control两个选项
```
**其它**  
****  
```
//黑色背景下需调整鼠标
  桌面-->右键-->性化-->改鼠标样式-->“自定义”框中选中“文本选择”-->浏览-->找到beam_r.cur 样式，使用即可！
//设置4个空格代替tab键
视图-->缩进-->Tab 4个空格
```

# Markdown语法
```
换行 --> 连续两个空格
分割线 --> *** 连续3个*号
代码框 --> `print` 两个`夹起
粗体 --> **此处粗体** 双*夹起
斜体 --> *此处斜体* 单*夹起
代码块 -->```java, 三个`夹起，可指定语言
标题 --> 1~6个#号，表示1~6级标题， #号后加空格
图片与链接
  区别在一个!号，图片需要url  
  [baidu](www.baidu.com)  
  ![插入的图片](http://cdn.sspai.com/attachment/thumbnail/2014/04/15/f96c892fc63933ab186235f7c910753b10f77_mw_800_wm_1_wmp_3.jpg)
列表
```
  
# git  
```
索引词：git关键字、github关键字  
1、生成公私钥对： ssh-keygen -t rsa -C "desneo@163.com"  
    （不需密码，默认即可）， 用户主目录（/c/Users/Administrator/.ssh）下生成id_rsa和id_rsa.pub文件  
2、github设置账户公钥：settings-->SSH and GPG keys -->New SSH key --> 将公钥内容全部复制-->添加  
3、获取远程仓库，并命名别名：  
    //不要使用 git clone https://github.com/desneo/refinfo （https形式，否则每次push都要输入用户名目录）  
    git clone git@github.com:desneo/refinfo.git  
    git remote add refinfo git@github.com:desneo/refinfo.git  
4、推送到远程仓库：  
    git add tool.md	//添加修改的文件  
    //git add .  //添加所有变化的文件
    git commit -m '修改的注释'  	
    git push refinfo gh-pages	//本地仓库推送到指定分支  
5、更新： git pull
6、删除文件: git rm [-r] *    //若是删除目录需 -r
7、重命名： git mv helloworld.c helloworld1.c
8、强制覆盖： git push -f   //强覆盖方式用你本地的代码替代git仓库内的内容
```

#vim/gvim（安装目录下vimrc）
```
##打开utf-8乱码
"显示行号  
set nu  
"自动折行  
set wrap  
"tab间距  
set tabstop=4  
set softtabstop=4  
"文件编码，打开utf-8乱码问题  
set encoding=utf-8  
"终端编码需与当前主机保持一致，否则展示乱码  
set termencoding=gbk  
set fileencoding=utf-8  
set fileencodings=ucs-bom,utf-8,gbk,cp936,gb18030,big5,euc-jp,euc-kr,latin  
##gvim中文菜单乱码乱  
"gvim解决菜单乱码   
source $VIMRUNTIME/delmenu.vim  
source $VIMRUNTIME/menu.vim  
"gvim解决consle输出乱码  
language messages zh_CN.utf-8 
```


# 其它  
**安装新字体**  
```
方法a) 字体文件（*.ttf）直接拖进 C:\windows\fonts 目录中。
方法b) 字体下载到硬盘，然后打开“控制面板”->字体 ->再点击菜单“文件”->安装新字体。
```

**windows上关闭站口占用的程序**  
```
1) 查找占用端口的进程号， netstat -ano | findstr 80
2) 进程号找到进程名，tasklist | findstr 2000
3) 进程管理器中关闭进程即可
```

# word相关
**不能输入英文双引号**  
```
左上角office图标-->word选项-->校对-->自动更正选项-->自动套用格式-->取消“直引号替换为弯引号”
                                                  &&“键入时自动套用格式”取消“直引号替换为弯引号”
```

**粘贴图片显示空白**  
```
点击左上角office图标word选项高级”显示文档内容”栏下的“显示图片框”， 不勾选	。	即可显示图片。
```
