# 第4章 介绍 Python 对象类型

在 Python 中，数据以对象的形式出现--无论是 Python 提供的内置对象，还是使用 Python 或者是像 C 扩展库这样的扩展语言工具创建的对象。

- 对象是内存中的一部分，包含数值和相关操作的集合。
- Python 脚本中的一切都是对象，甚至简单的带有值或支持运算操作（加法、减法等）的数字也是如此。

**为什么要使用内置类型**
- 内置对象使程序更容易编写
- 内置对象使可扩展的组件
- 内置对象往往比定制的数据结构更有效率
- 内置对象是语言标准的一部分

## Python 核心数据类型
在Python程序中处理的每样东西都是对象。

### 数字
~~~python
123 + 222   # Integer addition

1.5 * 4     # Floating-point multiplication

2 ** 100    # 2 to the power 100, again
~~~
Python3.X 的整数类型在需要的时候会自动提供额外的精度，以用于较大的数值。这些嵌套的调用从内向外运行 -- 首先将 ** 的运算结果利用内嵌的 str 函数转型成字符串，接着用 len 函数得到该字符串的长度。最后的结果就是这个位数的个数。**Python 中很多对象类型都支持 str 和 len**

模块 math 中包含一些数学函数以提供更高级的数学运算。而 random 模块主要用于随机数的生成器和随机选择器。

### 字符串
字符串用于记录文本信息和任意的字节集合。是一种序列（包含其他对象的有序集合），序列中的元素包含了一个从左到右的顺序：序列中的元素根据它们的相对位置进行存储和读取。

#### 序列操作
字符串支持假设其中各个元素包含位置顺序的操作。
~~~python
S = 'Spam'      # Make a 4-character string, and assign it to a name
len(S)          # Length    
S[0]            # The first item is S by zero-based position
S[1]            # The second item from the left
~~~~

Python 的变量不需要提前声明。当给一个变量赋值的时候就创建了它，可能赋的是任何类型的对象，并且当变量出现在一个表达式中的时候，就会用其值替换它。在使用变量的值之前，必须对其进行赋值。

**在 Python 中，可以反响索引，从最后一个开始（正向索引是从左边开始计算，反向索引是从右边开始计算）**
~~~ python
S[-1]           # The last item in S
S[-2]           # The second-to-last item from the end
S[len(S) - 1]   # Negative indexing, the hard way
~~~
我们可以在方括号中使用任意表达式，而不仅仅是使用数字字面量--可以使用字面量、变量或任意表达式

**序列支持分片操作（Slice）**
~~~python
S               # A 4-character string
S[1:3]          # Slice of S from offsets 1 through 2 (not 3)
~~~
分片可以被看作从一个字符串中一次提取一部分的方法，一般形式为：**X[I, J]**，表示在 X 中从偏移量为 I，直到但不包括偏移量为 J 的内容。结果是返回一个新的对象。常用变体有：
~~~python
S[1:]           # Everything past the first (1:len(S))
S               # S itself hasn't changed
S[0:3]          # Everything but the last
S[:3]           # Same as S[0:3]
S[:-1]          # Everything but the last again, but simpler (0:-1)
S[:]            # All of S a top-level copy (0:len(S)
~~~
注意负偏移量是如何用在分片的边界上的。最后一个命令是一种常用的序列操作。

**字符串支持使用加号进行拼接（Concatenation）（将两个字符串合并为一个新的字符串），或者重复（通过再重复一次创建一个新的字符串）**
~~~python
S + 'xyz'
S * 8
~~~
**多态**：一个操作的意义取决于被操作的对象。对于数字而言，“+”表示加法，但对于符串而言“+s”表示拼接。

#### 不可变性

每个字符串操作都被定义为生成新的字符串作为其结果，因为字符串在Python中具有不可变性--在创建后不能原位置（in place）改变--永远不可能覆盖不可变对象的值。但在Python中可以建立一个新的字符串并以同一个变量名对其进行赋值，因为Python在运行过程中会清理旧的对象。
~~~python
S
S[0] = 'z'          # Immutable objects cannot be changed
S = 'z' + S[1:]     # But we can run expressions to make new objects
~~~

Python 中的每个对象都可以被归类为可变的或不可变的。核心数据类型中，数字、字符串和元组是不可变的，列表、字典和集合是可变的（可以在任何位置完全自由的改变）。

如果需要在原位置更改基于文本的数据有两种方法：
- 将字符串扩展成为一个由独立字符构成的列表
    ~~~python
    S = 'shrubbery'
    L = list(S)                 # Expand to a list: [...]
    L
    L[1] = S                    # Change it in place
    ''.join(L)                  # Join with empty delimiter
    ~~~
- 使用 'bytearray' 类型
    ~~~python
    B = bytearray(b'Spam')      # A bytes/list bybrid (ahead)
    B.extend(b'eggs')           # 'b' needed in 3.X
    B                           # B[i] = ord(c) works here too
    B.decode()                  # Translate to normal string
    ~~~
    bytearray 支持文本的原位置转换，但只适用于字符编码最多8位宽（ASCII码）的文本，其他的所有字符串依然是不可变的。bytearray融合了不可变的字节字符串和可变列表两者的特性。

#### 特定类型的方法
除了一般的序列操作，字符串还有独有的作为方法存在的操作。**方法**：依赖并作用在对象上的函数，并通过一个调用表达式触发。字符串的方法有：
- find ： 子字符串查找操作（将返回一个传入子字符串的偏移量，或者在没有找到的情况下返回-1）
    ~~~python
    S = 'Spam'
    S.find('pa')                # find the offset 
    ~~~
- replace ： 对全局进行搜索和替换
    ~~~python
    S.replece('pa', 'XYZ')
    S
    ~~~
    这find 和 repalce两种方法都针对它们所依赖和被调用的对象而进行。但都不会改变原始的字符串，而是会创建一个新的字符串作为结果。
- 还有一些其他的字符串方法
    ~~~python
    line = 'aaa,bbb,cccc,dd'
    line.split()                # Split on a delimiter into a list of substrings

    S = 'Spam'
    S.upper()                   # Upper-and lowercase conversions
    S.isalpha()                 # Content tests: isalpha, isdigit, etc

    line = 'aaa,bbb,cccc,dd\n'
    line.rstrip()               # Remove whitespace characters on the right side
    line.rstrip().split(',')    # Combine two operationss
    # 在调用 split 之前，调用了 rstrip 函数
    # Python遵循从左到右的执行顺序，每次前一步方法调用结束都会为后一步方法调用产生一个临时对象。
    ~~~
- 字符串还支持一个叫做格式化的高级替代操作，可以以一个表达式的形式和一个字符串方法调用形式使用。在2.7和3.1中的字符串方法调用可以允许省略相应的参数序号：
    ~~~python
    '%s, eggs, and %s' % ('spam', 'SPAM!')          s# Formatting expression (all)
    '{0}, eggs, and {1}'.format('spam', 'SPAM!')    # Formatting method (2.6+, 3.0+)
    '{}, eggs, and {}'.format('spam', 'SPAM!')      # Formatting method (2.7+, 3.1+)
    ~~~

序列操作通用，但是方法不是通用的。一条简明的法则是，Python的工具库是呈层级分布的：可作用于多种类型的通用操作都是以内置函数或表达式的形式出现的（len(X), X[0]），但是看类型特定的操作是以方法调用的形式出现的（aString.upper()）

#### 寻求帮助
调用内置dir函数（dir(S)），函数列出了在调用者的作用域内，形参函数的默认值，会返回一个列表，包含对象的所有属性。**dir 函数简单的给出了方法的名称**

类型的双下划线方法，代表了字符串对象的实现方式，支持定制（重载中会学到）。
- e.g. 字符串的 \_\_add\_\_ 方法才是真正执行字符串拼接的函数。虽然 Python 内部将 + 操作映射到 \_\_add\_\_ 中，但是应该尽量避免直接使用第二种形式（这不仅十分晦涩，而且会让程序运行的很慢）：
    ~~~python
    S + 'NI!'           #
    S.__add__('NI!')    # 
    ~~~
    一般来说，以双下划线开头并结尾的变量名用来表示 Python 实现细节的命名模式

**help 函数可用于查询方法的具体用途。**
~~~python
help(S.replace)
# 使用 shift + Q 退出
~~~

#### 字符串编程的其他方式