# QT中使用静态文件库

报错：:-1:error: No rule to make target 'C:/Users/zw/QT/FileSafe/./liblibusb-1.0d.a', needed by '../bin/FileSafe.exe'.  Stop.

在导入静态文件库时，找到静态库的.a和.h文件

右击项目名->添加库->外部库->库文件名选择.a，包含路径选择include。

在.pro文件就会生成代码。

注意：ide会在.a文件名前加上lib前缀，记得删除掉，不然会报上述错误
