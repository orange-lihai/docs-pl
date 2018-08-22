##### IO 的基本概念
输入/输出(Input/Output, 简写为 I/O或者IO)是指计算机进程与外部世界(磁盘文件/网络设备/鼠标/键盘/打印机等)之间的通信. 输入是进程接收的信号或数据, 输出则是从其发送的信号或数据.

但是从IO的特征分类来说, 在Java8中, 有2个IO相关的包
* `java.io`  阻塞IO
* `java.nio`  非阻塞IO与异步IO

从IO的应用场景来说, 在Java编程的日常工作中, 用得最多的一般是
* `文件IO`
* `网络IO`

~~~
// 打开一个文件, 并写入一些数据, 然后保存.
private static void writeToFile() {
    String filePathName = "/D:/simple.txt";
    File f = new File(filePathName);
    try {
        // 如果文件不存在, 创建文件
        if (!f.exists()) {
            boolean createSuccess = f.createNewFile();
            if (!createSuccess) { throw new IOException("create new file failed!"); }
        }
        // 向文件中写入一些内容
        try (FileWriter fw = new FileWriter(f)) {
            fw.append("1, open a file by file name."); fw.append(System.lineSeparator());
            fw.append("2, create the file if not exists."); fw.append(System.lineSeparator());
            fw.append("3, write something to the file."); fw.append(System.lineSeparator());
            fw.append("4, flush() and close() the file."); fw.append(System.lineSeparator());
            fw.flush();
            fw.close();
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
}
// 按行读取文件
private static void readFromFile() {
    String filePathName = "/D:/simple.txt";
    File f = new File(filePathName);
    // 如果文件存在, 按行读取并打印文件内容
    if (f.exists() && f.isFile() && f.canRead()) {
        try (BufferedReader br = new BufferedReader(new FileReader(f))){
            br.lines().forEach(System.out::println);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

~~~

~~~
// 监听一个网络端口, 接受和处理网络消息
~~~

~~~
// 链接到一个远端网络, 发送一些消息

~~~