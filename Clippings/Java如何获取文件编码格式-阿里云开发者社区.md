# Java如何获取文件编码格式-阿里云开发者社区
[Java如何获取文件编码格式-阿里云开发者社区](https://developer.aliyun.com/article/48804) 

 1：简单判断是UTF-8或不是UTF-8，因为一般除了UTF-8之外就是GBK，所以就设置默认为GBK。  

-------------------------------------------------------

 按照给定的字符集存储文件时，在文件的最开头的三个字节中就有可能存储着编码信息，所以，基本的原理就是只要读出文件前三个字节，判定这些字节的值，就可以得知其编码的格式。其实，如果项目运行的平台就是中文操作系统，如果这些文本文件在项目内产生，即开发人员可以控制文本的编码格式，只要判定两种常见的编码就可以了：GBK和UTF-8。由于中文Windows默认的编码是GBK，所以一般只要判定UTF-8编码格式。

   对于UTF-8编码格式的文本文件，其前3个字节的值就是-17、-69、-65，所以，判定是否是UTF-8编码格式的代码片段如下：

          File file = new File(path);
          InputStream in= new java.io.FileInputStream(file);
          byte\[\] b = new byte\[3\];
          in.read(b);
          in.close();
          if (b\[0\] == -17 && b\[1\] == -69 && b\[2\] == -65)
              System.out.println(file.getName() + "：编码为UTF-8");
          else
              System.out.println(file.getName() + "：可能是GBK，也可能是其他编码");

2：若想实现更复杂的文件编码检测，可以使用一个开源项目cpdetector，它所在的网址是：[http://cpdetector.sourceforge.net/](http://cpdetector.sourceforge.net/)。它的类库很小，只有500K左右，cpDetector是基于统计学原理的，不保证完全正确，利用该类库判定文本文件的代码如下：
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

读外部文件(先利用cpdetector检测文件的编码格式，然后用检测到的编码方式去读文件):

    /**
         * 利用第三方开源包cpdetector获取文件编码格式
         * 
         * @param path
         *            要判断文件编码格式的源文件的路径
         * @author huanglei
         * @version 2012-7-12 14:05
         */
        public static String getFileEncode(String path) {
                /*
                 * detector是探测器，它把探测任务交给具体的探测实现类的实例完成。
                 * cpDetector内置了一些常用的探测实现类，这些探测实现类的实例可以通过add方法 加进来，如ParsingDetector、
                 * JChardetFacade、ASCIIDetector、UnicodeDetector。
                 * detector按照“谁最先返回非空的探测结果，就以该结果为准”的原则返回探测到的
                 * 字符集编码。使用需要用到三个第三方JAR包：antlr.jar、chardet.jar和cpdetector.jar
                 * cpDetector是基于统计学原理的，不保证完全正确。
                 */
                CodepageDetectorProxy detector = CodepageDetectorProxy.getInstance();
                /*
                 * ParsingDetector可用于检查HTML、XML等文件或字符流的编码,构造方法中的参数用于
                 * 指示是否显示探测过程的详细信息，为false不显示。
                 */
                detector.add(new ParsingDetector(false));
                /*
                 * JChardetFacade封装了由Mozilla组织提供的JChardet，它可以完成大多数文件的编码
                 * 测定。所以，一般有了这个探测器就可满足大多数项目的要求，如果你还不放心，可以
                 * 再多加几个探测器，比如下面的ASCIIDetector、UnicodeDetector等。
                 */
                detector.add(JChardetFacade.getInstance());// 用到antlr.jar、chardet.jar
                // ASCIIDetector用于ASCII编码测定
                detector.add(ASCIIDetector.getInstance());
                // UnicodeDetector用于Unicode家族编码的测定
                detector.add(UnicodeDetector.getInstance());
                java.nio.charset.Charset charset = null;
                File f = new File(path);
                try {
                        charset = detector.detectCodepage(f.toURI().toURL());
                } catch (Exception ex) {
                        ex.printStackTrace();
                }
                if (charset != null)
                        return charset.name();
                else
                        return null;
        }

String charsetName = getFileEncode(configFilePath);
System.out.println(charsetName);
inputStream = new FileInputStream(configFile);
BufferedReader in = new BufferedReader(new InputStreamReader(inputStream, charsetName));

读jar包内部资源文件(先利用cpdetector检测jar内部的资源文件的编码格式，然后以检测到的编码方式去读文件)：

    /**
         * 利用第三方开源包cpdetector获取URL对应的文件编码
         * 
         * @param path
         *            要判断文件编码格式的源文件的URL
         * @author huanglei
         * @version 2012-7-12 14:05
         */
        public static String getFileEncode(URL url) {
                /*
                 * detector是探测器，它把探测任务交给具体的探测实现类的实例完成。
                 * cpDetector内置了一些常用的探测实现类，这些探测实现类的实例可以通过add方法 加进来，如ParsingDetector、
                 * JChardetFacade、ASCIIDetector、UnicodeDetector。
                 * detector按照“谁最先返回非空的探测结果，就以该结果为准”的原则返回探测到的
                 * 字符集编码。使用需要用到三个第三方JAR包：antlr.jar、chardet.jar和cpdetector.jar
                 * cpDetector是基于统计学原理的，不保证完全正确。
                 */
                CodepageDetectorProxy detector = CodepageDetectorProxy.getInstance();
                /*
                 * ParsingDetector可用于检查HTML、XML等文件或字符流的编码,构造方法中的参数用于
                 * 指示是否显示探测过程的详细信息，为false不显示。
                 */
                detector.add(new ParsingDetector(false));
                /*
                 * JChardetFacade封装了由Mozilla组织提供的JChardet，它可以完成大多数文件的编码
                 * 测定。所以，一般有了这个探测器就可满足大多数项目的要求，如果你还不放心，可以
                 * 再多加几个探测器，比如下面的ASCIIDetector、UnicodeDetector等。
                 */
                detector.add(JChardetFacade.getInstance());// 用到antlr.jar、chardet.jar
                // ASCIIDetector用于ASCII编码测定
                detector.add(ASCIIDetector.getInstance());
                // UnicodeDetector用于Unicode家族编码的测定
                detector.add(UnicodeDetector.getInstance());
                java.nio.charset.Charset charset = null;
                try {
                        charset = detector.detectCodepage(url);
                } catch (Exception ex) {
                        ex.printStackTrace();
                }
                if (charset != null)
                        return charset.name();
                else
                        return null;
        }

URL url = CreateStationTreeModel.class.getResource("/resource/" + "配置文件");
URLConnection urlConnection = url.openConnection();
inputStream=urlConnection.getInputStream();
String charsetName = getFileEncode(url);
System.out.println(charsetName);
BufferedReader in = new BufferedReader(new InputStreamReader(inputStream, charsetName));

3：探测任意输入的文本流的编码，方法是调用其重载形式：   

charset=detector.detectCodepage(待测的文本输入流,测量该流所需的读入字节数);

上面的字节数由程序员指定，字节数越多，判定越准确，当然时间也花得越长。要注意，字节数的指定不能超过文本流的最大长度。

4：判定文件编码的具体应用举例：

    属性文件(.properties)是Java程序中的常用文本存储方式，象STRUTS框架就是利用属性文件存储程序中的字符串资源。它的内容如下所示：

    #注释语句

    属性名=属性值

    读入属性文件的一般方法是：

      FileInputStream ios=new FileInputStream(“属性文件名”);
      Properties prop=new Properties();
      prop.load(ios);
      String value=prop.getProperty(“属性名”);
      ios.close();

    利用java.io.Properties的load方法读入属性文件虽然方便，但如果属性文件中有中文，在读入之后就会发现出现乱码现象。发生这个原因是load方法使用字节流读入文本，在读入后需要将字节流编码成为字符串，而它使用的编码是“iso-8859-1”,这个字符集是ASCII码字符集，不支持中文编码，

    方法一：使用显式的转码：

       String value=prop.getProperty(“属性名”);
       String encValue=new String(value.getBytes(“iso-8859-1″),”属性文件的实际编码”);

 方法二：象这种属性文件是项目内部的，我们可以控制属性文件的编码格式，比如约定采用Windows内定的GBK，就直接利用”gbk”来转码，     如果约定采用UTF-8，就使用”UTF-8″直接转码。

    方法三：如果想灵活一些，做到自动探测编码，就可利用上面介绍的方法测定属性文件的编码，从而方便开发人员的工作

补充：可以用下面代码获得Java支持编码集合：

    Charset.availableCharsets().keySet();

    可以用下面的代码获得系统默认编码：

    Charset.defaultCharset();

特别说明：尊重作者的劳动成果，转载请注明出处哦~~~http://blog.yemou.net/article/query/info/tytfjhfascvhzxcyt209