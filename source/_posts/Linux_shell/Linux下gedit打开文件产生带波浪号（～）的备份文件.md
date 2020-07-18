---
title: Linux下gedit打开文件产生带波浪号（～）的备份文件
date: 2016-12-20 08:00:17
tag: 
 - Linux
categories: 
 - 技术 
---
  在ubuntu系统下，使用gedit编辑器打开文件（如test.md），会自动生成一个备份文件，文件名字为（原文件名＋波浪号 test.md~）,这种情况是为了防止系统或者编辑器突然崩溃，以恢复文件使用。但是有时候看着特别繁琐和凌乱。所以一下为解决办法，相反也可以开启自动备份功能。
# 如何快速删除此类备份文件
	rm -rf *.*~

+ -f --force 忽略不存在文件，强制删除并不提示
+ -r --recursive 删除该目录及其子目录下所有符合条件的文件
+ *.*~ *代表任意，任意文件名+'.'+任意文件类型后缀+'~',满足该条件的文件都删除

# 如何设置取消生成自动备份文件
+ gedit编辑器：　最大化窗口->Edit->Preferences->Editor->file saving->取消'Creat a backup copy of files before saving'选项，中文系统同理。
+ Vi/Vim编辑器: 
	vi ~/.vimrc
	set nobackup
通过以上方法可以实现删除或者取消自动备份文件功能，相反可以开启自动备份功能。
