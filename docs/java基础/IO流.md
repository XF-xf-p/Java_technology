## 1.字符流、字节流

### 字符与字节

Bit最小的二进制单位 ，是计算机的操作部分。取值0或者1

Byte（字节）是计算机操作数据的最小单位由8位bit组成 取值（-128-127）

Char（字符）是用户的可读写的最小单位，在Java里面由16位bit组成 取值（0-65535）

### 字节流

操作byte类型数据，主要操作类是OutputStream、InputStream的子类；不用缓冲区，直接对文件本身操作。

### 字符流

操作字符类型数据，主要操作类是Reader、Writer的子类；使用缓冲区缓冲字符，不关闭流就不会输出任何内容。

### 互相转换

整个IO包实际上分为字节流和字符流，但是除了这两个流之外，还存在一组字节流-字符流的转换类。

OutputStreamWriter：是Writer的子类，将输出的字符流变为字节流，即将一个字符流的输出对象变为字节流输出对象。

InputStreamReader：是Reader的子类，将输入的字节流变为字符流，即将一个字节流的输入对象变为字符流的输入对象。

## 2.输入流、输出流

输入、输出，有一个参照物，参照物就是存储数据的介质。如果是把对象读入到介质中，这就是输入。从介质中向外读数据，这就是输出。

所以，输入流是把数据写入存储介质的。输出流是从存储介质中把数据读取出来。

## 3.字节流和字符流之间的相互转换

想要实现字符流和字节流之间的相互转换需要用到两个类：

OutputStreamWriter 是字符流通向字节流的桥梁。

InputStreamReader 是字节流通向字符流的桥梁。

### 字符流转成字节流

```
File f = new File("test.txt");
OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream(f),"UTF-8");
```

### 字节流转成字符流

```
File f = new File("test.txt");
InputStreamReader inr = new InputStreamReader(new FileInputStream(f),"UTF-8");
```

## 4.ObjectOutput和ObjectInput 接口

ObjectInput接口 扩展自 DataInput 接口以包含对象的读操作。

> DataInput 接口用于从二进制流中读取字节，并根据所有 Java 基本类型数据进行重构。同时还提供根据 UTF-8 修改版格式的数据重构 String 的工具。
>
> 对于此接口中的所有数据读取例程来说，如果在读取所需字节数之前已经到达文件末尾 (end of file)，则将抛出 EOFException（IOException 的一种）。如果因为到达文件末尾以外的其他原因无法读取字节，则将抛出 IOException 而不是 EOFException。尤其是，在输入流已关闭的情况下，将抛出 IOException。

ObjectOutput 扩展 DataOutput 接口以包含对象的写入操作。

> DataOutput 接口用于将数据从任意 Java 基本类型转换为一系列字节，并将这些字节写入二进制流。同时还提供了一个将 String 转换成 UTF-8 修改版格式并写入所得到的系列字节的工具。
>
> 对于此接口中写入字节的所有方法，如果由于某种原因无法写入某个字节，则抛出 IOException。