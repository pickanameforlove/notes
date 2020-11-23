### vscode显示程序运行程序在output这个面板中，导致无法进行输入的交互。

vscode设置里面有一个run in terminal使用集成终端的选项，勾上即可。

### VScode终端显示乱码

直接设置右下角的编码方式，改为GB2312即可。

### Switch 一个case里面声明了int t1，在另一个case里面同样声明会报错

switch各case是在一个作用域里面。

### switch 如何使用break跳出外层循环

在switch里面无法实现，只能在外面判定。

### 右键添加新建md文件

```
[HKEY_CLASSES_ROOT\.md]
@="Typora.md"
"Content Type"="text/markdown"
"PerceivedType"="text"

[HKEY_CLASSES_ROOT\.md\ShellNew]
"NullFile"=""
```

### 右键添加windows terminal

```
[HKEY_CLASSES_ROOT\Directory\Background\shell\wt]
@="Windows Terminal here"
"Icon"="D:\\Document\\terminal.ico"

[HKEY_CLASSES_ROOT\Directory\Background\shell\wt\command]
@="C:\\Users\\lmk\\AppData\\Local\\Microsoft\\WindowsApps\\wt.exe"
```

### 本地git与github同步

github那边先新建一个repo

本地git产生一个ssh  key，添加到github上面

```c++
//建一个新的本地repo同步。
echo "# teset" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:pickanameforlove/teset.git
git push -u origin main
//push一个已经存在的本地repo
git remote add origin git@github.com:pickanameforlove/teset.git
git branch -M main
git push -u origin main

```

