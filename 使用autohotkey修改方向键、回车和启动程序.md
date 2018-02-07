---
title: 使用autohotkey修改方向键、回车和启动程序
date: 2017-04-07 21:34:40
---

[autohotkey官网](https://autohotkey.com/)

[autohotkey官方文档](https://autohotkey.com/docs/AutoHotkey.htm)

[官方文档中文版](http://ahkcn.sourceforge.net/docs/AutoHotkey.htm)

### 具体步骤

1.  下载并安装autohotkey。

2.  在你觉得合适的地方鼠标右键-新建-autohotkey script（脚本）；或者创建一个别的文件，再把后缀改成ahk也可以

3.  一个新建的ahk文档里面会有这些东西

   ~~~
   #NoEnv  ; Recommended for performance and compatibility with future AutoHotkey releases.
   ; #Warn  ; Enable warnings to assist with detecting common errors.
   SendMode Input  ; Recommended for new scripts due to its superior speed and reliability.
   SetWorkingDir %A_ScriptDir%  ; Ensures a consistent starting directory.
   ~~~

   不用管这些

4.  在下面输入

   ~~~
   !j::
      Send, {Down}
   Return

   !l::
      Send, {Right}
   Return

   !h::
      Send, {Left}
   Return

   !k::
      Send, {Up}
   Return
   ~~~

   这几句话是把↑改成了alt+k；↓为alt+j；←为alt+h；→为alt+l。如果想用ctrl代替alt，就把`!` 换成`^` 。其他的`+` 代表shift，`#` 代表windows键，更详细的看[这个](http://ahkcn.sourceforge.net/docs/Hotkeys.htm)。如果不需要其他功能了，直接保存并关闭，跳到第 步。

5.  **加入修改回车的功能**。键盘左侧的Capslock（锁定大小写）键使用频率相对还是不高的，而回车键的位置又有点坑，改之。直接复制这段这两行。

   ~~~
   $CapsLock::Enter

   LAlt & Capslock::SetCapsLockState, % GetKeyState("CapsLock", "T") ? "Off" : "On"
   ~~~

   这样，左侧的Capslock键就成了回车，以后小量的大写字母用shift+字母，如果有大量的大写字母的输入，用`alt+Capslock` ，跟之前Capslock是一样的。

6.  autohotkey也可以用快捷键启动软件，例如

   ~~~
   !o::
      Run, C:\Program Files\Everything\Everything.exe
   Return
   ~~~

   现在`alt+o`就是启动everything的快捷键了。同样的，如果想用ctrl代替alt，就把`!` 换成`^` 。其他的`+` 代表shift，`#` 代表windows键，更详细的看[这个](http://ahkcn.sourceforge.net/docs/Hotkeys.htm)。如果想启动其他的软件，就把上面的路径换成你想启动的那个软件的路径机就可以了。

7.  保存，关闭

8.  在编辑好的文件点击鼠标右键，选择编译脚本，也可能是Compile script，也可能是Compile脚本，都是一样的。编译之后就会生成一个`.exe`的文件，把这个文件放到开机启动文件夹中（C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp），每次开机就可以自动启动了。
