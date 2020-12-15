
## NIO
总结自http://ifeve.com/java-nio-all/
### 概述
* Java NIO(New IO)是一个可以替代标准Java IO API的IO API（从Java 1.4开始),Java NIO提供了与标准IO不同的IO工作方式.
* 核心部分:Channels,Buffers,Selectors
* 数据可以从Channel读到Buffer中,也可以从Buffer 写到Channel中
* 同步非阻塞

### Channel
FileChannel 从文件中读写数据.

DatagramChannel 能通过UDP读写网络中的数据.
  
SocketChannel 能通过TCP读写网络中的数据.
  
ServerSocketChannel可以监听新进来的TCP连接,像Web服务器那样.对每一个新进来的连接都会创建一个SocketChannel.
#### FileChannel
* FileChannel无法设置为非阻塞模式,它总是运行在阻塞模式下.
* 无法直接打开,需要通过使用一个InputStream、OutputStream或RandomAccessFile来获取一个FileChannel实例
````
//获取channel
RandomAccessFile aFile = new RandomAccessFile("data/nio-data.txt", "rw");
FileChannel inChannel = aFile.getChannel();
//从channel读
ByteBuffer buf = ByteBuffer.allocate(48);
int bytesRead = inChannel.read(buf);//返回读取的数量
//向channel写
String newData = "New String to write to file..." + System.currentTimeMillis();
ByteBuffer buf = ByteBuffer.allocate(48);
buf.clear();
buf.put(newData.getBytes());
buf.flip();
while(buf.hasRemaining()) {
	channel.write(buf);
}
//用完关闭
channel.close();
````
#### DatagramChannel
* 收发UDP包
````
//打开
DatagramChannel channel = DatagramChannel.open();
channel.socket().bind(new InetSocketAddress(9999));
//receive方法接收
ByteBuffer buf = ByteBuffer.allocate(48);
buf.clear();
channel.receive(buf);
//send()方法发送
String newData = "New String to write to file..." + System.currentTimeMillis();
ByteBuffer buf = ByteBuffer.allocate(48);
buf.clear();
buf.put(newData.getBytes());
buf.flip();

int bytesSent = channel.send(buf, new InetSocketAddress("jenkov.com", 80));

//连接特定的地址,并调用read,write
channel.connect(new InetSocketAddress("jenkov.com", 80));
int bytesRead = channel.read(buf);
int bytesWritten = channel.write(but);
````
#### SocketChannel
* 连接到TCP网络套接字的通道
    1. 打开一个SocketChannel并连接到互联网上的某台服务器.
    2. 一个新连接到达ServerSocketChannel时,会创建一个SocketChannel.
````
//打开
SocketChannel socketChannel = SocketChannel.open();
socketChannel.connect(new InetSocketAddress("http://jenkov.com", 80));
//读取
ByteBuffer buf = ByteBuffer.allocate(48);
int bytesRead = socketChannel.read(buf);
//写入
String newData = "New String to write to file..." + System.currentTimeMillis();

ByteBuffer buf = ByteBuffer.allocate(48);
buf.clear();
buf.put(newData.getBytes());

buf.flip();

while(buf.hasRemaining()) {
    channel.write(buf);
}
````
* 非阻塞模式
````
socketChannel.configureBlocking(false);
socketChannel.connect(new InetSocketAddress("http://jenkov.com", 80));

while(! socketChannel.finishConnect() ){
    //wait, or do something else...
}
````
#### ServerSocketChannel
* 监听新进来的TCP连接的通道
````
ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();

serverSocketChannel.socket().bind(new InetSocketAddress(9999));

while(true){
    SocketChannel socketChannel =
            serverSocketChannel.accept();

    //do something with socketChannel...
}


//非阻塞模式
ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();

serverSocketChannel.socket().bind(new InetSocketAddress(9999));
serverSocketChannel.configureBlocking(false);

while(true){
    SocketChannel socketChannel =
            serverSocketChannel.accept();

    if(socketChannel != null){
        //do something with socketChannel...
    }
}
````

### Buffer
* 本质上是一块可以写入数据,然后可以从中读取数据的内存
    1. 写入数据到Buffer
    2. 调用flip()方法
    3. 从Buffer中读取数据
    4. 调用clear()方法或者compact()方法
* Buffer的capacity,position和limit
![](http://ifeve.com/wp-content/uploads/2013/06/buffers-modes.png)
capacity: Buffer固定的大小值
position: 写入:当前的位置,初始0,最大capacity – 1;读取:重置为0
limit:写入:等于capacity;读取:limit设置为写模式的position值
* buffer的类型

    ByteBuffer,MappedByteBuffer,CharBuffer,DoubleBuffer,FloatBuffer,IntBuffer,LongBuffer,ShortBuffer
### Scatter/Gather
* Scatter 在读操作时将读取的数据写入多个buffer中
![](http://ifeve.com/wp-content/uploads/2013/06/scatter.png)
````
ByteBuffer header = ByteBuffer.allocate(128);
ByteBuffer body   = ByteBuffer.allocate(1024);

ByteBuffer[] bufferArray = { header, body };

channel.read(bufferArray);
````
* Gather 在写操作时将多个buffer的数据写入同一个Channel
![](http://ifeve.com/wp-content/uploads/2013/06/gather.png)
````
ByteBuffer header = ByteBuffer.allocate(128);
ByteBuffer body   = ByteBuffer.allocate(1024);

//write data into buffers

ByteBuffer[] bufferArray = { header, body };

channel.write(bufferArray);
````
### 通道之间的数据传输
* transferFrom()
````
RandomAccessFile fromFile = new RandomAccessFile("fromFile.txt", "rw");
FileChannel      fromChannel = fromFile.getChannel();

RandomAccessFile toFile = new RandomAccessFile("toFile.txt", "rw");
FileChannel      toChannel = toFile.getChannel();

long position = 0;
long count = fromChannel.size();

toChannel.transferFrom(position, count, fromChannel);
````
* transferTo()
````
RandomAccessFile fromFile = new RandomAccessFile("fromFile.txt", "rw");
FileChannel      fromChannel = fromFile.getChannel();

RandomAccessFile toFile = new RandomAccessFile("toFile.txt", "rw");
FileChannel      toChannel = toFile.getChannel();

long position = 0;
long count = fromChannel.size();

fromChannel.transferTo(position, count, toChannel);
````
###  Selector
````
//创建
Selector selector = Selector.open();
//注册
channel.configureBlocking(false);
SelectionKey key = channel.register(selector, SelectionKey.OP_READ);
while(true) {
  int readyChannels = selector.select();
  if(readyChannels == 0) continue;
  Set selectedKeys = selector.selectedKeys();
  Iterator keyIterator = selectedKeys.iterator();
  while(keyIterator.hasNext()) {
    SelectionKey key = keyIterator.next();
    if(key.isAcceptable()) {
        // a connection was accepted by a ServerSocketChannel.
    } else if (key.isConnectable()) {
        // a connection was established with a remote server.
    } else if (key.isReadable()) {
        // a channel is ready for reading
    } else if (key.isWritable()) {
        // a channel is ready for writing
    }
    keyIterator.remove();
  }
}
````
### Pipe
![](http://ifeve.com/wp-content/uploads/2013/06/pipe.bmp)
```
Pipe pipe = Pipe.open();
//写数据 需要访问sink()通道
Pipe.SinkChannel sinkChannel = pipe.sink();
//写入
String newData = "New String to write to file..." + System.currentTimeMillis();
ByteBuffer buf = ByteBuffer.allocate(48);
buf.clear();
buf.put(newData.getBytes());

buf.flip();

while(buf.hasRemaining()) {
    sinkChannel.write(buf);
}
//读取访问source()通道
Pipe.SourceChannel sourceChannel = pipe.source();
ByteBuffer buf = ByteBuffer.allocate(48);

int bytesRead = sourceChannel.read(buf);
```



