## 交叉编译Qt应用程序

* 配置PC机的Qt编译环境：

```c
    $ export QT_PREFIX=/opt/qt-4.8.5
    $ export PATH=${QT_PREFIX}/bin:$PATH
    $ export QMAKESPEC=${QT_PREFIX}/mkspecs/qws/linux-arm-g++
```

* 创建代码：

```c
    $ mkdir hellomyir
    $ cd hellomyir
    $ gedit hellomyir.cpp
```

输入如下代码：

```c
    #include <QApplication>
    #include <QLabel>
    int main(int argc, char **argv)
    {
    QApplication app(argc,argv);
    QLabel label("Make Your idea Real!");
    label.show();
    return app.exec();
    }
```

* 编译：

```c
    $ qmake -project
    $ qmake
    $ make
```
* 将生成的hellomyir拷贝到开发板，并在开发板上执行：

```c
    # ./hellomyir -qws
```

将编译生成的可执行文件拷贝到开发板中运行，将会在LCD屏幕上看到“Make Your idea Real!”的Qt窗口。
