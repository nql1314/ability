## IO分类
![](../resources/io.jpg)
* 磁盘操作:File
* 字节操作:InputStream 和 OutputStream
* 字符操作:Reader 和 Writer
* 对象操作:Serializable
* 网络操作:IO.Socket
* 新的输入/输出:NIO
## 流
* 流:代表任何有能力产出数据的数据源对象或者是有能力接受数据的接收端对象<Thinking in Java>
* 流的本质:数据传输,根据数据传输特性将流抽象为各种类,方便更直观的进行数据操作.
### 输入字节流InputStream
    1. InputStream是所有的输入字节流的父类,它是一个抽象类.
    2. ByteArrayInputStream、StringBufferInputStream(上图的StreamBufferInputStream)、FileInputStream是三种基本的介质流,它们分别从Byte数组、StringBuffer、和本地文件中读取数据.
    3. PipedInputStream是从与其它线程共用的管道中读取数据.
    4. ObjectInputStream和所有FilterInputStream的子类都是装饰流（装饰器模式的主角）.
### 输出字节流OutputStream
    1. OutputStream是所有的输出字节流的父类,它是一个抽象类.
    2. ByteArrayOutputStream、FileOutputStream是两种基本的介质流,它们分别向Byte数组、和本地文件中写入数据.PipedOutputStream是向与其它线程共用的管道中写入数据.
    3. ObjectOutputStream和所有FilterOutputStream的子类都是装饰流.
### 字符输入流Reader
    1. Reader是所有的输入字符流的父类,它是一个抽象类.
    2. CharReader、StringReader是两种基本的介质流,它们分别将Char数组、String中读取数据.PipedReader是从与其它线程共用的管道中读取数据.
    3. BufferedReader很明显就是一个装饰器,它和其子类负责装饰其它Reader对象.   
    4. FilterReader是所有自定义具体装饰流的父类,其子类PushbackReader对Reader对象进行装饰,会增加一个行号.   
    5. InputStreamReader是一个连接字节流和字符流的桥梁,它将字节流转变为字符流.FileReader可以说是一个达到此功能、常用的工具类,在其源代码中明显使用了将FileInputStream转变为Reader的方法.我们可以从这个类中得到一定的技巧.Reader中各个类的用途和使用方法基本和InputStream中的类使用一致.后面会有Reader与InputStream的对应关系.
### 字符输出流Writer
    1. Writer是所有的输出字符流的父类,它是一个抽象类.
    2. CharArrayWriter、StringWriter是两种基本的介质流,它们分别向Char数组、String中写入数据.PipedWriter是向与其它线程共用的管道中写入数据,  
    3. BufferedWriter是一个装饰器为Writer提供缓冲功能.
    4. PrintWriter和PrintStream极其类似,功能和使用也非常相似.
    5. OutputStreamWriter是OutputStream到Writer转换的桥梁,它的子类FileWriter其实就是一个实现此功能的具体类（具体可以研究一SourceCode）.功能和使用和OutputStream极其类似.
## 序列化
* 将java对象转成二进制流保存在磁盘中或者网络传输,并且能恢复.
* 实现Serializable或者Extmalizable接口
* 使用transient修饰的变量不被序列化工具存储
* 静态变量不会序列化

    
## AIO(NOI.2)
* 异步非阻塞

## Socket