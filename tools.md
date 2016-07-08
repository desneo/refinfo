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
  
#git  
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
	git commit -m '修改的注释'  	
	git push refinfo gh-pages	//本地仓库推送到指定分支  


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
"终端编码需与当前主机保持一致，否则展示乱码
set termencoding=gbk
set fileencoding=utf-8
set fileencodings=ucs-bom,utf-8,gbk,cp936,gb18030,big5,euc-jp,euc-kr,latin1
##gvim中文菜单乱码乱
"gvim解决菜单乱码  
source $VIMRUNTIME/delmenu.vim  
source $VIMRUNTIME/menu.vim  
"gvim解决consle输出乱码  
language messages zh_CN.utf-8 
