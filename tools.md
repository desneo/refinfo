# Markdown语法

# 换行

连续两个空格

## 标题

  1~6个#号，表示1~6级标题， #号后加空格

## 列表


## 图片与链接

  区别在一个!号，图片需要url  
  [baidu](www.baidu.com)  
  ![插入的图片](http://cdn.sspai.com/attachment/thumbnail/2014/04/15/f96c892fc63933ab186235f7c910753b10f77_mw_800_wm_1_wmp_3.jpg)
  



#vim/gvim（安装目录下vimrc）
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
set termencoding=utf-8
set fileencoding=utf-8
set fileencodings=ucs-bom,utf-8,gbk,cp936,gb18030,big5,euc-jp,euc-kr,latin1
##gvim中文菜单乱码乱
"gvim解决菜单乱码  
source $VIMRUNTIME/delmenu.vim  
source $VIMRUNTIME/menu.vim  
"gvim解决consle输出乱码  
language messages zh_CN.utf-8 
