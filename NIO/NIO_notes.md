### 核心部分
* Channels
* Buffers
* Selectors

### Channel 和 Buffer
基本上，所有的 IO 在NIO 中都从一个Channel 开始。Channel 有点象流。 数据可以从Channel读到Buffer中，也可以从Buffer 写到Channel中

![](.NIO_notes_images\78ba1b85.png)

* FileChannel 从文件中读写数据
* DatagramChannel 能通过UDP读写网络中的数据
* SocketChannel 能通过TCP读写网络中的数据
* ServerSocketChannel
  可以监听新进来的TCP连接，像Web服务器那样。对每一个新进来的连接都会创建一个SocketChannel

正如你所看到的，这些通道涵盖了UDP 和 TCP 网络IO，以及文件IO。

以下是Java NIO里关键的Buffer实现：

* ByteBuffer
* CharBuffer
* DoubleBuffer
* FloatBuffer
* IntBuffer
* LongBuffer
* ShortBuffer

这些Buffer覆盖了你能通过IO发送的基本数据类型：byte, short, int, long, float,
double 和 char, Java NIO 还有个 MappedByteBuffer，用于表示内存映射文件

### Selector
Selector允许单线程处理多个 Channel。如果你的应用打开了多个连接（通道），但每个连接的流量都很低，使用Selector就会很方便。例如，在一个聊天服务器中。
![](.NIO_notes_images\d0a5a3eb.png)

要使用Selector，得向Selector注册Channel，然后调用它的select()方法。这个方法会一直阻塞到某个注册的通道有事件就绪。一旦这个方法返回，线程就可以处理这些事件，事件的例子有如新连接进来，数据接收等。


### FileChannel读取数据到Buffer
```
/*
1. 这类不算是IO体系中的子类。
2. 而是直接继承Object
3. 但是它是IO包中的成员，因为它具备读和写的功能。
4. 能完成读写的原理是内部封装了字节输入流和输出流。
5. 而且内部还封装了一个数组，通过指针对数组的元素进行操作。
6. 可通过getFilePointer获取指针位置。
7. 也可通过seek改变指针的位置。
8. 通过构造函数可以看出，该类只能操作文件。
9.
10. 操作文件有4种模式："r"、"rw"、"rws" 或 "rwd"
11.
12. 如果模式为只读r。则不会创建文件，而是会去读取一个已经存在的文件，如果读取的文件不存在则会出现异常。
13. 如果模式为rw读写。如果文件不存在则会去创建文件，如果存在则不会创建。
14.
15. 可以实现多线程下载！因为seek可以调节指针，这样可以开启多个线程，来写相同间隔的数据。

*/
RandomAccessFile aFile = new RandomAccessFile("data/nio-data.txt", "rw");
FileChannel inChannel = aFile.getChannel();

ByteBuffer buf = ByteBuffer.allocate(48);

int bytesRead = inChannel.read(buf);
while (bytesRead != -1) {

System.out.println("Read " + bytesRead);
buf.flip();

while(buf.hasRemaining()){
System.out.print((char) buf.get());
}

buf.clear();
bytesRead = inChannel.read(buf);
}
aFile.close();
```

### Buffer
使用Buffer读写数据一般遵循以下四个步骤：
1. 写入数据到Buffer
2. 调用`flip()`方法
3. 从Buffer中读取数据
4. 调用`clear()`方法或者`compact()`方法

当向buffer写入数据时，buffer会记录下写了多少数据。一旦要读取数据，需要通过flip()方法将Buffer从写模式切换到读模式。在读模式下，可以读取之前写入到buffer的所有数据。

一旦读完了所有的数据，就需要清空缓冲区，让它可以再次被写入。有两种方式能清空缓冲区：调用clear()或compact()方法。clear()方法会清空整个缓冲区。compact()方法只会清除已经读过的数据。任何未读的数据都被移到缓冲区的起始处，新写入的数据将放到缓冲区未读数据的后面。


**capacity,position和limit**
缓冲区本质上是一块可以写入数据，然后可以从中读取数据的内存。这块内存被包装成NIO Buffer对象，并提供了一组方法，用来方便的访问该块内存。

![](.NIO_notes_images\285885fd.png)

#### capacity

作为一个内存块，Buffer有一个固定的大小值，也叫"capacity".你只能往里写capacity个byte、long，char等类型。一旦Buffer满了，需要将其清空（通过读数据或者清除数据）才能继续写数据往里写数据。
#### position

当你写数据到Buffer中时，position表示当前的位置。初始的position值为0.当一个byte、long等数据写到Buffer后， position会向前移动到下一个可插入数据的Buffer单元。position最大可为capacity -- 1.

当读取数据时，也是从某个特定位置读。当将Buffer从写模式切换到读模式，position会被重置为0. 当从Buffer的position处读取数据时，position向前移动到下一个可读的位置。

#### limit

在写模式下，Buffer的limit表示你最多能往Buffer里写多少数据。 写模式下，limit等于Buffer的capacity。

当切换Buffer到读模式时， limit表示你最多能读到多少数据。因此，当切换Buffer到读模式时，limit会被设置成写模式下的position值。换句话说，你能读到之前写入的所有数据（limit被设置成已写数据的数量，这个值在写模式下就是position）

> 一句话总结：capacity代表buffer空间大小，limit代表能写入和能读取的数据大小(limit-postion),postion代表当前读写的位置

* ByteBuffer
* MappedByteBuffer
* CharBuffer
* DoubleBuffer
* FloatBuffer
* IntBuffer
* LongBuffer
* ShortBuffer

如你所见，这些Buffer类型代表了不同的数据类型。换句话说，就是可以通过char，short，int，long，float 或 double类型来操作缓冲区中的字节。

MappedByteBuffer 有些特别

### Buffer的分配

要想获得一个Buffer对象首先要进行分配。 每一个Buffer类都有一个allocate方法。下面是一个分配48字节capacity的ByteBuffer的例子。

```
ByteBuffer buf = ByteBuffer.allocate(48);
```

这是分配一个可存储1024个字符的CharBuffer：

```
CharBuffer buf = CharBuffer.allocate(1024);
```

### 向Buffer中写数据

写数据到Buffer有两种方式：
* 从Channel写到Buffer。
* 通过Buffer的put()方法写到Buffer里。

从Channel写到Buffer的例子

```
int bytesRead = inChannel.read(buf); //read into buffer.
```

通过put方法写Buffer的例子：

```
buf.put(127);
```

put方法有很多版本，允许你以不同的方式把数据写入到Buffer中。例如， 写到一个指定的位置，或者把一个字节数组写入到Buffer。 更多Buffer实现的细节参考JavaDoc。

#### flip()方法

flip方法将Buffer从写模式切换到读模式。调用flip()方法会将position设回0，并将limit设置成之前position的值。

换句话说，position现在用于标记读的位置，limit表示之前写进了多少个byte、char等 ------ 现在能读取多少个byte、char等。

### 从Buffer中读取数据

从Buffer中读取数据有两种方式：
1. 从Buffer读取数据到Channel。
2. 使用get()方法从Buffer中读取数据。

从Buffer读取数据到Channel的例子：

```
//read from buffer into channel.
int bytesWritten = inChannel.write(buf);
```

使用get()方法从Buffer中读取数据的例子

```
byte aByte = buf.get();
```

get方法有很多版本，允许你以不同的方式从Buffer中读取数据。例如，从指定position读取，或者从Buffer中读取数据到字节数组。更多Buffer实现的细节参考JavaDoc。

### rewind()方法

Buffer.rewind()将position设回0，所以你可以重读Buffer中的所有数据。limit保持不变，仍然表示能从Buffer中读取多少个元素（byte、char等）。

### clear()与compact()方法

一旦读完Buffer中的数据，需要让Buffer准备好再次被写入。可以通过clear()或compact()方法来完成。

如果调用的是clear()方法，position将被设回0，limit被设置成 capacity的值。换句话说，Buffer 被清空了。Buffer中的数据并未清除，只是这些标记告诉我们可以从哪里开始往Buffer里写数据。

如果Buffer中有一些未读的数据，调用clear()方法，数据将"被遗忘"，意味着不再有任何标记会告诉你哪些数据被读过，哪些还没有。

如果Buffer中仍有未读的数据，且后续还需要这些数据，但是此时想要先先写些数据，那么使用compact()方法。

compact()方法将所有未读的数据拷贝到Buffer起始处。然后将position设到最后一个未读元素正后面。limit属性依然像clear()方法一样，设置成capacity。现在Buffer准备好写数据了，但是不会覆盖未读的数据。

### mark()与reset()方法

通过调用Buffer.mark()方法，可以标记Buffer中的一个特定position。之后可以通过调用Buffer.reset()方法恢复到这个position。例如：

```
buffer.mark();
buffer.reset();  //set position back to mark.
```

### equals()与compareTo()方法

可以使用equals()和compareTo()方法两个Buffer。

#### equals()

当满足下列条件时，表示两个Buffer相等：
1. 有相同的类型（byte、char、int等）。
2. Buffer中剩余的byte、char等的个数相等。
3. Buffer中所有剩余的byte、char等都相同。

如你所见，equals只是比较Buffer的一部分，不是每一个在它里面的元素都比较。实际上，它只比较Buffer中的剩余元素。

#### compareTo()方法

compareTo()方法比较两个Buffer的剩余元素(byte、char等)， 如果满足下列条件，则认为一个Buffer"小于"另一个Buffer：
1. 第一个不相等的元素小于另一个Buffer中对应的元素 。
2. 所有元素都相等，但第一个Buffer比另一个先耗尽(第一个Buffer的元素个数比另一个少)。

### Scatter/Gather 分散 聚集

```
ByteBuffer header = ByteBuffer.allocate(128);
ByteBuffer body   = ByteBuffer.allocate(1024);
ByteBuffer[] bufferArray = { header, body };
channel.read(bufferArray);
```
![](.NIO_notes_images\44343de4.png)

```
ByteBuffer header = ByteBuffer.allocate(128);
ByteBuffer body   = ByteBuffer.allocate(1024);
ByteBuffer[] bufferArray = { header, body };
channel.write(bufferArray);
```
![](.NIO_notes_images\3f6db356.png)

### transferTo
指定原通道传递到对应通道包括起电和大小

### transferFrom
指定对应通道从原通道传递，包括起点和大小
```
FileChannel from = new FileInputStream(path + "/1.txt").getChannel();
FileChannel to = new FileOutputStream(path + "/2.txt").getChannel();
/**
 *  位置 长度 通道？
 * long position, long count,WritableByteChannel target
 *  transfer 传递
 */
from.transferTo(0, from.size(), to);
/**
 * 通道 位置 长度  直接将文件1.txt 复制到 2.txt
 * ReadableByteChannel src,long position, long count
 */
to.transferFrom(from, 0, from.size());
```

### Selector
```
//创建通道管理器，向管理器注册通道
Selector selector = Selector.open();
//与Selector一起使用时，Channel必须处于非阻塞模式下。这意味着不能将
//FileChannel与Selector一起使用，因为FileChannel不能切换到非阻塞模式。而套接字通道都可以。

channel.configureBlocking(false);
SelectionKey key = channel.register(selector, Selectionkey.OP_READ);
```
四种不同类型的事件
1. SelectionKey.OP_CONNECT 连接就绪
2. SelectionKey.OP_ACCEPT 接收就绪
3. SelectionKey.OP_READ 读就绪
4. SelectionKey.OP_WRITE 写就绪

#### 附加对象

```
//可以将一个对象或者更多信息附着到SelectionKey上，这样就能方便的识别某个给定的通道
selectionKey.attach(theObject);
Object attachedObj = selectionKey.attachment();
//向Selector注册Channel的时候附加对象
SelectionKey key = channel.register(selector, SelectionKey.OP_READ, theObject);
//返回管理器所有注册通道
Set selectedKeys = selector.selectedKeys();

```

* int select() 阻塞到至少有一个通道在你注册的事件上就绪了，返回当前就绪通道数
* int select(long timeout) 同上，最长阻塞时间
* int selectNow() 不会阻塞，此方法执行非阻塞的选择操作。如果自从前一次选择操作后，没有通道变成可选择的，则此方法直接返回零

### FileChannel
```
RandomAccessFile aFile = new RandomAccessFile("data/nio-data.txt", "rw");
FileChannel inChannel = aFile.getChannel();
ByteBuffer buf = ByteBuffer.allocate(48);
//读取数据
int bytesRead = inChannel.read(buf);

//清除数据，写入数据，改读模式，写入文件管道
buf.clear();
buf.put(newData.getBytes());
buf.flip();
//FileChannel.write()是在while循环中调用的。因为无法保证write()方法一次能向FileChannel写入多少字节，因此需要重复调用write()方法，直到Buffer中已经没有尚未写入通道的字节。
while(buf.hasRemaining()) {
	inChannel.write(buf);
}

//truncate 截断 将指定长度后的部分删除
channel.truncate(1024);

//force 方法将通道里尚未写入磁盘的数据强制写到磁盘上。出于性能方面的考虑，操作系统会将数据缓存在内存中，所以无法保证写入到FileChannel里的数据一定会即时写到磁盘上。要保证这一点，需要调用force()方法
inChannel.force(true);
```

### SocketChannel
```
SocketChannel socketChannel = SocketChannel.open();
socketChannel.connect(new InetSocketAddress("http://jenkov.com", 80));

while(! socketChannel.finishConnect() ){
    //连接未关闭，可以处理事情
    ByteBuffer buffer = ByteBuffer.allocate(10);
    //将通道里信息读取到buffer
    socketChannel.read(buffer);
    ByteBuffer outBuffer = ByteBuffer.wrap("通道写入信息".getBytes("urf-8"));
    socketChannel.write(buffer);
}


socketChannel.close();

```

### DatagramChannel 是一个能收发UDP包的通道。因为UDP是无连接的网络协议，所以不能像其它通道那样读取和写入。它发送和接收的是数据包
```
DatagramChannel channel = DatagramChannel.open();
channel.socket().bind(new InetSocketAddress(9999));
ByteBuffer buf = ByteBuffer.allocate(48);
buf.clear();
channel.receive(buf);

buf.clear();
buf.put("发送消息".getBytes());
buf.flip();
// UDP 发送消息无需建立连接，直接发送
int bytesSent = channel.send(buf, new InetSocketAddress("jenkov.com", 80));

/**
可以将DatagramChannel“连接”到网络中的特定地址的。由于UDP是无连接的，
连接到特定地址并不会像TCP通道那样创建一个真正的连接。而是锁住DatagramChannel ，
让其只能从特定地址收发数据。绑定后 发送接收消息都一样
**/
channel.connect(new InetSocketAddress("jenkov.com", 80));
int bytesRead = channel.read(buf);
int bytesWritten = channel.write(but);

```

### pipe
Java NIO 管道是2个线程之间的单向数据连接。
Pipe有一个source通道和一个sink通道。数据会被写到sink通道，从source通道读取

![](.NIO_notes_images\93d2ba63.png)

```

Pipe pipe = Pipe.open();
Pipe.SinkChannel sinkChannel = pipe.sink();

ByteBuffer buf = ByteBuffer.allocate(48);
buf.clear();
buf.put("写入信息".getBytes());
buf.flip();
while(buf.hasRemaining()) {
    sinkChannel.write(buf);
}

//读取管道信息
Pipe.SourceChannel sourceChannel = pipe.source();
ByteBuffer buf = ByteBuffer.allocate(48);
int bytesRead = sourceChannel.read(buf);
```

#### Java NIO和IO的主要区别

| IO |    NIO | |
| :-------- | :--------|:---|
| 面向流  | 面向缓冲 | Java NIO和IO之间第一个最大的区别是，IO是面向流的，NIO是面向缓冲区的。 Java IO面向流意味着每次从流中读一个或多个字节，直至读取所有字节，它们没有被缓存在任何地方。此外，它不能前后移动流中的数据。如果需要前后移动从流中读取的数据，需要先将它缓存到一个缓冲区。 Java NIO的缓冲导向方法略有不同。数据读取到一个它稍后处理的缓冲区，需要时可在缓冲区中前后移动。这就增加了处理过程中的灵活性。但是，还需要检查是否该缓冲区中包含所有您需要处理的数据。而且，需确保当更多的数据读入缓冲区时，不要覆盖缓冲区里尚未处理的数据。|
| 阻塞IO  | 非阻塞IO | Java IO的各种流是阻塞的。这意味着，当一个线程调用read() 或 write()时，该线程被阻塞，直到有一些数据被读取，或数据完全写入。该线程在此期间不能再干任何事情了。 Java NIO的非阻塞模式，使一个线程从某通道发送请求读取数据，但是它仅能得到目前可用的数据，如果目前没有数据可用时，就什么都不会获取。而不是保持线程阻塞，所以直至数据变的可以读取之前，该线程可以继续做其他的事情。 非阻塞写也是如此。一个线程请求写入一些数据到某通道，但不需要等待它完全写入，这个线程同时可以去做别的事情。 线程通常将非阻塞IO的空闲时间用于在其它通道上执行IO操作，所以一个单独的线程现在可以管理多个输入和输出通道（channel）。|
| 无      | 选择器 | Java NIO的选择器允许一个单独的线程来监视多个输入通道，你可以注册多个通道使用一个选择器，然后使用一个单独的线程来“选择”通道：这些通道里已经有可以处理的输入，或者选择已准备写入的通道。这种选择机制，使得一个单独的线程很容易来管理多个通道。|


![](.NIO_notes_images\b377e6b8.png)

![](.NIO_notes_images\1701aa04.png)

![](.NIO_notes_images\2033477e.png)

![](.NIO_notes_images\b0183378.png)