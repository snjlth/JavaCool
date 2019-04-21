

<h3 id="11-相关概念">1.1 相关概念</h3>



<h4 id="java-io"><strong>Java IO</strong></h4>

<p>Java IO即Java 输入输出系统。不管我们编写何种应用，都难免和各种输入输出相关的媒介打交道，其实和媒介进行IO的过程是十分复杂的，这要考虑的因素特别多，比如我们要考虑和哪种媒介进行IO（文件、控制台、网络），我们还要考虑具体和它们的通信方式（顺序、随机、二进制、按字符、按字、按行等等）。Java类库的设计者通过设计大量的类来攻克这些难题，这些类就位于<strong>java.io</strong>包中。</p>

<p>在JDK1.4之后，为了提高Java IO的效率，Java又提供了一套新的IO，Java New IO简称<strong>Java NIO</strong>。它在标准java代码中提供了高速的面向块的IO操作。本篇文章重点介绍Java IO，关于Java NIO请参考我的另两篇文章： <br>
<a href="http://blog.csdn.net/suifeng3051/article/details/48160753" rel="nofollow">Java NIO详解(一)</a> <br>
<a href="http://blog.csdn.net/suifeng3051/article/details/48441629" rel="nofollow">Java NIO详解(二)</a></p>

<h4 id="流"><strong>流</strong></h4>

<p>在Java IO中，流是一个核心的概念。流从概念上来说是一个连续的数据流。你既可以从流中读取数据，也可以往流中写数据。流与数据源或者数据流向的媒介相关联。在Java IO中流既可以是字节流(以字节为单位进行读写)，也可以是字符流(以字符为单位进行读写)。</p>



<h4 id="io相关的媒介"><strong>IO相关的媒介</strong></h4>

<p>Java的IO包主要关注的是从原始数据源的读取以及输出原始数据到目标媒介。以下是最典型的数据源和目标媒介：</p>

<ul>
<li>文件</li>
<li>管道</li>
<li>网络连接</li>
<li>内存缓存</li>
<li>System.in, System.out, System.error(注：Java标准输入、输出、错误输出)</li>
</ul>



<h2 id="二java-io类库的框架">二、Java IO类库的框架</h2>



<h3 id="21-java-io的类型">2.1 Java IO的类型</h3>

<p>虽然java IO类库庞大，但总体来说其框架还是很清楚的。从是读媒介还是写媒介的维度看，Java IO可以分为：</p>

<ol>
<li>输入流：InputStream和Reader</li>
<li>输出流：OutputStream和Writer</li>
</ol>

<p>而从其处理流的类型的维度上看，Java IO又可以分为：</p>

<ol>
<li>字节流：InputStream和OutputStream</li>
<li>字符流：Reader和Writer</li>
</ol>

<p>下面这幅图就清晰的描述了JavaIO的分类：</p>

<table>
<thead>
<tr>
  <th>-</th>
  <th>字节流</th>
  <th>字符流</th>
</tr>
</thead>
<tbody><tr>
  <td>输入流</td>
  <td>InputStream</td>
  <td>Reader</td>
</tr>
<tr>
  <td>输出流</td>
  <td>OutputStream</td>
  <td>Writer</td>
</tr>
</tbody></table>


<p>我们的程序需要通过InputStream或Reader从数据源读取数据，然后用OutputStream或者Writer将数据写入到目标媒介中。其中，InputStream和Reader与数据源相关联，OutputStream和writer与目标媒介相关联。 以下的图说明了这一点：</p>

<p><img src="res/20150910154348754.png" alt="这里写图片描述" title=""></p>



<h3 id="22-io-类库">2.2 IO 类库</h3>

<p>上面我们介绍了Java IO中的四各类：<strong>InputStream、OutputStream、Reader、Writer</strong>，其实在我们的实际应用中，我们用到的一般是它们的子类，之所以设计这么多子类，目的就是让每一个类都负责不同的功能，以方便我们开发各种应用。各类用途汇总如下：</p>

<ul>
<li>文件访问</li>
<li>网络访问</li>
<li>内存缓存访问</li>
<li>线程内部通信(管道)</li>
<li>缓冲</li>
<li>过滤</li>
<li>解析</li>
<li>读写文本 (Readers / Writers)</li>
<li>读写基本类型数据 (long, int etc.)</li>
<li>读写对象</li>
</ul>

<p>下面我们就通过两张图来大体了解一下这些类的继承关系及其作用</p>

<p>图1：java io 类的集成关系</p>

<p><img src="res/20150910154516952.png" alt="这里写图片描述" title=""></p>

<p>图2：java io中各个类所负责的媒介</p>

<p><img src="res/20150910154540639.png" alt="这里写图片描述" title=""></p>



<h2 id="三java-io的基本用法">三、Java IO的基本用法</h2>



<h3 id="31-java-io-字节流">3.1 Java IO ：字节流</h3>

<p>通过上面的介绍我们已经知道，<strong>字节流</strong>对应的类应该是<strong>InputStream</strong>和<strong>OutputStream</strong>，而在我们实际开发中，我们应该根据不同的媒介类型选用相应的子类来处理。下面我们就用字节流来操作文件媒介：</p>

<p><strong><em>例1，用字节流写文件</em></strong></p>

<pre><code>  public static void writeByteToFile() throws IOException{
        String hello= new String( "hello word!");
         byte[] byteArray= hello.getBytes();
        File file= new File( "d:/test.txt");
         //因为是用字节流来写媒介，所以对应的是OutputStream 
         //又因为媒介对象是文件，所以用到子类是FileOutputStream
        OutputStream os= new FileOutputStream( file);
         os.write( byteArray);
         os.close();
  }
</code></pre>

<p><strong><em>例2，用字节流读文件</em></strong></p>

<pre><code>public static void readByteFromFile() throws IOException{
        File file= new File( "d:/test.txt");
         byte[] byteArray= new byte[( int) file.length()];
         //因为是用字节流来读媒介，所以对应的是InputStream
         //又因为媒介对象是文件，所以用到子类是FileInputStream
        InputStream is= new FileInputStream( file);
         int size= is.read( byteArray);
        System. out.println( "大小:"+size +";内容:" +new String(byteArray));
         is.close();
  }
</code></pre>



<h3 id="32-java-io-字符流">3.2 Java IO ：字符流</h3>

<p>同样，<strong>字符流</strong>对应的类应该是<strong>Reader</strong>和<strong>Writer</strong>。下面我们就用字符流来操作文件媒介:</p>

<p><strong><em>例3，用字符流读文件</em></strong></p>

<pre><code>public static void writeCharToFile() throws IOException{
        String hello= new String( "hello word!");
        File file= new File( "d:/test.txt");
         //因为是用字符流来读媒介，所以对应的是Writer，又因为媒介对象是文件，所以用到子类是FileWriter
        Writer os= new FileWriter( file);
         os.write( hello);
         os.close();
  }
</code></pre>

<p><strong><em>例4，用字符流写文件</em></strong></p>

<pre><code>  public static void readCharFromFile() throws IOException{
        File file= new File( "d:/test.txt");
         //因为是用字符流来读媒介，所以对应的是Reader
         //又因为媒介对象是文件，所以用到子类是FileReader
        Reader reader= new FileReader( file);
         char [] byteArray= new char[( int) file.length()];
         int size= reader.read( byteArray);
        System. out.println( "大小:"+size +";内容:" +new String(byteArray));
         reader.close();
  }
</code></pre>



<h3 id="33-java-io-字节流转换为字符流">3.3 Java IO ：字节流转换为字符流</h3>

<p>字节流可以转换成字符流，java.io包中提供的<strong>InputStreamReader</strong>类就可以实现，当然从其命名上就可以看出它的作用。其实这涉及到另一个概念，IO流的<strong>组合</strong>，后面我们详细介绍。下面看一个简单的例子：</p>

<p><strong><em>例5 ，字节流转换为字符流</em></strong></p>

<pre><code>public static void convertByteToChar() throws IOException{
        File file= new File( "d:/test.txt");
         //获得一个字节流
        InputStream is= new FileInputStream( file);
         //把字节流转换为字符流，其实就是把字符流和字节流组合的结果。
        Reader reader= new InputStreamReader( is);
         char [] byteArray= new char[( int) file.length()];
         int size= reader.read( byteArray);
        System. out.println( "大小:"+size +";内容:" +new String(byteArray));
         is.close();
         reader.close();
  }
</code></pre>



<h3 id="34-java-io-io类的组合">3.4 Java IO ：IO类的组合</h3>

<p>从上面字节流转换成字符流的例子中我们知道了IO流之间可以组合（或称嵌套），其实组合的目的很简单，就是把多种类的特性融合在一起以实现更多的功能。组合使用的方式很简单，通过把一个流放入另一个流的构造器中即可实现，两个流之间可以组合，三个或者更多流之间也可组合到一起。当然，并不是任意流之间都可以组合。关于组合就不过多介绍了，后面的例子中有很多都用到了组合，大家好好体会即可。</p>



<h3 id="35-java-io文件媒介操作">3.5 Java IO：文件媒介操作</h3>

<p>File是Java IO中最常用的读写媒介，那么我们在这里就对文件再做进一步介绍。</p>



<h4 id="351-file媒介"><strong>3.5.1 File媒介</strong></h4>

<p><strong><em>例6 ，File操作</em></strong></p>

<pre><code>public class FileDemo {
  public static void main(String[] args) {
         //检查文件是否存在
        File file = new File( "d:/test.txt");
         boolean fileExists = file.exists();
        System. out.println( fileExists);
         //创建文件目录,若父目录不存在则返回false
        File file2 = new File( "d:/fatherDir/subDir");
         boolean dirCreated = file2.mkdir();
        System. out.println( dirCreated);
         //创建文件目录,若父目录不存则连同父目录一起创建
        File file3 = new File( "d:/fatherDir/subDir2");
         boolean dirCreated2 = file3.mkdirs();
        System. out.println( dirCreated2);
        File file4= new File( "d:/test.txt");
         //判断长度
         long length = file4.length();
         //重命名文件
         boolean isRenamed = file4.renameTo( new File("d:/test2.txt"));
         //删除文件
         boolean isDeleted = file4.delete();
        File file5= new File( "d:/fatherDir/subDir");
         //是否是目录
         boolean isDirectory = file5.isDirectory();
         //列出文件名
        String[] fileNames = file5.list();
         //列出目录
        File[]   files = file4.listFiles();
  }
</code></pre>

<p>}</p>



<h4 id="353-随机读取file文件"><strong>3.5.3 随机读取File文件</strong></h4>

<p>通过上面的例子我们已经知道，我们可以用FileInputStream（文件字符流）或FileReader（文件字节流）来读文件，这两个类可以让我们分别以字符和字节的方式来读取文件内容，但是它们都有一个不足之处，就是只能从文件头开始读，然后读到文件结束。</p>

<p>但是有时候我们只希望读取文件的一部分，或者是说随机的读取文件，那么我们就可以利用<strong>RandomAccessFile</strong>。RandomAccessFile提供了<code>seek()</code>方法，用来定位将要读写文件的指针位置，我们也可以通过调用<code>getFilePointer()</code>方法来获取当前指针的位置，具体看下面的例子:</p>

<p><strong><em>例7，随机读取文件</em></strong></p>

<pre><code>  public static void randomAccessFileRead() throws IOException {
         // 创建一个RandomAccessFile对象
        RandomAccessFile file = new RandomAccessFile( "d:/test.txt", "rw");
         // 通过seek方法来移动读写位置的指针
         file.seek(10);
         // 获取当前指针
         long pointerBegin = file.getFilePointer();
         // 从当前指针开始读
         byte[] contents = new byte[1024];
         file.read( contents);
         long pointerEnd = file.getFilePointer();
        System. out.println( "pointerBegin:" + pointerBegin + "\n" + "pointerEnd:" + pointerEnd + "\n" + new String(contents));
         file.close();
  }
</code></pre>

<p><strong><em>例8，随机写入文件</em></strong></p>

<pre><code>  public static void randomAccessFileWrite() throws IOException {
         // 创建一个RandomAccessFile对象
        RandomAccessFile file = new RandomAccessFile( "d:/test.txt", "rw");
         // 通过seek方法来移动读写位置的指针
         file.seek(10);
         // 获取当前指针
         long pointerBegin = file.getFilePointer();
         // 从当前指针位置开始写
         file.write( "HELLO WORD".getBytes());
         long pointerEnd = file.getFilePointer();
        System. out.println( "pointerBegin:" + pointerBegin + "\n" + "pointerEnd:" + pointerEnd + "\n" );
         file.close();
  }
</code></pre>



<h3 id="36-java-io管道媒介">3.6 Java IO：管道媒介</h3>

<p>管道主要用来实现同一个虚拟机中的两个线程进行交流。因此，一个管道既可以作为数据源媒介也可作为目标媒介。</p>

<p>需要注意的是java中的管道和Unix/Linux中的管道含义并不一样，在Unix/Linux中管道可以作为两个位于不同空间进程通信的媒介，而在java中，管道只能为同一个JVM进程中的不同线程进行通信。和管道相关的IO类为：<strong>PipedInputStream</strong>和<strong>PipedOutputStream</strong>，下面我们来看一个例子：</p>

<p><strong><em>例9，读写管道</em></strong></p>

<pre><code>public class PipeExample {
   public static void main(String[] args) throws IOException {
          final PipedOutputStream output = new PipedOutputStream();
          final PipedInputStream  input  = new PipedInputStream(output);
          Thread thread1 = new Thread( new Runnable() {
              @Override
              public void run() {
                  try {
                      output.write( "Hello world, pipe!".getBytes());
                  } catch (IOException e) {
                  }
              }
          });
          Thread thread2 = new Thread( new Runnable() {
              @Override
              public void run() {
                  try {
                      int data = input.read();
                      while( data != -1){
                          System. out.print(( char) data);
                          data = input.read();
                      }
                  } catch (IOException e) {
                  } finally{
                     try {
                                       input.close();
                                } catch (IOException e) {
                                       e.printStackTrace();
                                }
                  }
              }
          });
          thread1.start();
          thread2.start();
      }
</code></pre>

<p>}</p>



<h3 id="37-java-io网络媒介">3.7 Java IO：网络媒介</h3>

<p>关于Java IO面向网络媒介的操作即<strong>Java 网络编程</strong>，其核心是Socket，同磁盘操作一样，java网络编程对应着两套API，即Java IO和Java NIO，关于这部分我会准备专门的文章进行介绍。</p>



<h3 id="38-java-iobufferedinputstream和bufferedoutputstream">3.8 Java IO：BufferedInputStream和BufferedOutputStream</h3>

<p><strong>BufferedInputStream</strong>顾名思义，就是在对流进行写入时提供一个buffer来提高IO效率。在进行磁盘或网络IO时，原始的InputStream对数据读取的过程都是一个字节一个字节操作的，而BufferedInputStream在其内部提供了一个buffer，在读数据时，会一次读取一大块数据到buffer中，这样比单字节的操作效率要高的多，特别是进程磁盘IO和对大量数据进行读写的时候。</p>

<p>使用BufferedInputStream十分简单，只要把普通的输入流和BufferedInputStream组合到一起即可。我们把上面的例2改造成用BufferedInputStream进行读文件，请看下面例子：</p>

<p><strong><em>例10 ，用缓冲流读文件</em></strong></p>

<pre><code>  public static void readByBufferedInputStream() throws IOException {
        File file = new File( "d:/test.txt");
         byte[] byteArray = new byte[( int) file.length()];
         //可以在构造参数中传入buffer大小
        InputStream is = new BufferedInputStream( new FileInputStream(file),2*1024);
         int size = is.read( byteArray);
        System. out.println( "大小:" + size + ";内容:" + new String(byteArray));
         is.close();
  }
</code></pre>

<p>关于如何设置buffer的大小，我们应根据我们的硬件状况来确定。对于磁盘IO来说，如果硬盘每次读取4KB大小的文件块，那么我们最好设置成这个大小的整数倍。因为磁盘对于顺序读的效率是特别高的，所以如果buffer再设置的大写可能会带来更好的效率，比如设置成4*4KB或8*4KB。</p>

<p>还需要注意一点的就是磁盘本身就会有缓存，在这种情况下，BufferedInputStream会一次读取磁盘缓存大小的数据，而不是分多次的去读。所以要想得到一个最优的buffer值，我们必须得知道磁盘每次读的块大小和其缓存大小，然后根据多次试验的结果来得到最佳的buffer大小。</p>

<p><strong>BufferedOutputStream</strong>的情况和BufferedInputStream一致，在这里就不多做描述了。</p>



<h3 id="39-java-iobufferedreader和bufferedwriter">3.9 Java IO：BufferedReader和BufferedWriter</h3>

<p><strong>BufferedReader、BufferedWriter</strong> 的作用基本和BufferedInputStream、BufferedOutputStream一致，具体用法和原理都差不多 ，只不过一个是面向字符流一个是面向字节流。同样，我们将改造字符流中的例4，给其加上buffer功能，看例子：</p>

<pre><code> public static void readByBufferedReader() throws IOException {
        File file = new File( "d:/test.txt");
         // 在字符流基础上用buffer流包装，也可以指定buffer的大小
        Reader reader = new BufferedReader( new FileReader(file),2*1024);
         char[] byteArray = new char[( int) file.length()];
         int size = reader.read( byteArray);
        System. out.println( "大小:" + size + ";内容:" + new String(byteArray));
         reader.close();
  }
</code></pre>