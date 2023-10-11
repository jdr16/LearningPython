# 第三章 你应如何运行程序
本章目标：如何运行Python代码

本章重点：预备知识、说明约定、代码调试技巧总览、模块导入的概述 -->  理解Python程序架构的必要知识


## 运行代码
### 交互式运行代码 
~~~ python
print('Hello wordl')
print(2**8)
~~~

为什么要使用交互式命令行模式：
- **实验**：当对一段代码有疑问的时候就使用交互命令行运行并实验代码，查看结果；
- **测试**：测试程序组件

使用注意事项：
- 只能输入Python命令；
- print命令在文件中才是必须的；
- 提示符变化；
- 在交互式命令行模式中，使用一个空行结束复合语句；
- 交互式命令模式，一次运行一条语句。

不能在交互命令模式中复制并粘贴多行代码，除非这段代码的每条复合语句的后面都包含空行。

### 系统命令行和文件

~~~python
# A first Python script
# 储存在当前文件夹的 script1.py  中
import sys
print(sys.platform)     # 输出当前工作的计算机类型
print(2 ** 100)
x = 'Spam!'
print(x * 8)
~~~

在 cmd 中，调用脚本并在shell中打印输出
~~~
python script1.py 
~~~

在 cmd 中，调用脚本并将输出储存在 saveit.txt 中：
~~~
python script1.py > saveit.txt
~~~
流重定向（stream redirection），用于文本输入和输出，在 Windows 和 UNIX 系统中通用。

## 模块导入与重载
每一个 '.py' 结尾的 Python 源代码文件都是一个模块。不需要任何特殊的代码或语法来使文件成为模块。任何一个文件都可以通过导入一个模块，以读取这个模块定义的内容 -- 导入操作从本质上来讲就是载入另一个文件，并给予那个文件内容的权限。

Python 程序架构：更大的程序往往以多个模块文件的形式出现，并从其他模块文件导入工具。其中的一个模块文件指定为主文件，或叫做顶层文件，或“脚本” -- 就是那个启动后能够运行整个程序的文件，它照常逐行运行。这一层之下，全部是导入模块。

~~~python
import script1
import script1
# 导入只运行一次，只在第一次有输出

from imp import reload
reload(script1)
# 再次导入需要使用reload函数
# reload 函数中传入参数应为一个已被加载了的模块对象名称
# 因此，在重载模块之前，应该确保已经成功导入了这个模块
~~~

模块：变量名的包，命名空间；
变量名称：属性  绑定在特定对象 or 模块上的变量。

**从表面上看，一个模块文件的变量名可以通过两种Python语句读取 -- import 和 from 以及 reload 调用。**
~~~python
import myfile
print(myfile.title)

from myfile import title 
print(title)
~~~

~~~
>>> dir(threenames)
['__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__', 'a', 'b', 'c']
>>> threenames.__file__
'/Users/jdr/Desktop/code/Python学习手册/03/threenames.py'
~~~
dir 函数，传入参数为已导入模块的名称，返回的是模块内部的所有属性。

**避免使用 import 和 reload 来启动程序**


## 使用 exec 运行模块文件
使用交互命令行模式启动文件，而不用导入以及随后重载的另一种方法
~~~python
exec(open('script1.py').read())
~~~
exec都运行从文件读取的当前版本的代码，为不需要随后的重载。

exec 调用有类似 import 的用法， 但是不会真的导入模块。默认情况下，每次用这种方式调用 exec 时，都重新运行文件。类似于把文件粘贴到了调用 exec 的地方。缺点是，可能会覆盖正在使用的变量。

但基本 import语句，在每个进程中只运行一次文件，并且会把文件生成到一个单独的模块运行空间中，以便它的赋值不会改变原有作用域中的变量。为模块名称空间分隔所付出的代价是在修改后需要重载。

【Python3 中 exec 形式需要许多录入参数，以至于最佳的文件运行方式是不用它】
【**最好的启动文件方式是，输入系统shell命令行或者是IDLE菜单选项**】

## IDLE 用户界面

IDLE：一个能够编辑、运行、浏览和调试Python程序的桌面GUI，能够在一个单独的界面实现。

## 其他启动选项

- 嵌入式调用
- 冻结二进制可执行文件
- 文本编辑器启动方式


