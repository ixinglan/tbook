# File
**常用构造器:**  
* `public File(String pathname)` 以pathname为路径创建File对象，可以是绝对路径或者相对路径，如果pathname是相对路径，则默认的当前路径在系统属性user.dir中存储。
    > 绝对路径：是一个固定的路径,从盘符开始  
    > 相对路径：是相对于某个位置开始  
* `public File(String parent,String child)` 以parent为父路径，child为子路径创建File对象。 
* `public File(File parent,String child)` 根据一个父File对象和子文件路径创建File对象

```java
    @Test
    public void test1() throws IOException {
        System.out.println(System.getProperty("user.dir"));//默认路径
        File file = new File("src/io/file/static/hello.txt");

        //创建一个与file同目录下的另外一个文件，文件名为：haha.txt
        File destFile = new File(file.getParent(),"haha.txt");
        boolean newFile = destFile.createNewFile();
        if(newFile){
            System.out.println("创建成功！");
        }
    }
```

**路径分隔符:**  
* 路径中的每级目录之间用一个路径分隔符隔开。
* 路径分隔符和系统有关：
    > windows和DOS系统默认使用“\”来表示
    > UNIX和URL使用“/”来表示
* Java程序支持跨平台运行，因此路径分隔符要慎用。
* 为了解决这个隐患，File类提供了一个常量：`public static final String separator`。根据操作系统，动态的提供分隔符

**常用方法:**  
* 获取
    > `public String getAbsolutePath()`：获取绝对路径  
    > `public String getPath()` ：获取路径  
    > `public String getName()` ：获取名称  
    > `public String getParent()`：获取上层文件目录路径。若无，返回null  
    > `public long length()` ：获取文件长度（即：字节数）。不能获取目录的长度。 > public long lastModified() ：获取最后一次的修改时间，毫秒值  
    > `public String[] list()` ：获取指定目录下的所有文件或者文件目录的名称数组  
    > `public File[] listFiles()` ：获取指定目录下的所有文件或者文件目录的File数组  

* 重命名
    > `public boolean renameTo(File dest)`:把文件重命名为指定的文件路径  

* 判断
    > `public boolean isDirectory()`：判断是否是文件目录  
    > `public boolean isFile()` ：判断是否是文件  
    > `public boolean exists()` ：判断是否存在  
    > `public boolean canRead()` ：判断是否可读  
    > `public boolean canWrite()` ：判断是否可写  
    > `public boolean isHidden()` ：判断是否隐藏    

* 创建
    > `public boolean createNewFile()` ：创建文件。若文件存在，则不创建，返回false  
    > `public boolean mkdir()` ：创建文件目录。如果此文件目录存在，就不创建了。如果此文件目录的上层目录不存在，也不创建。   
    > `public boolean mkdirs()` ：创建文件目录。如果上层文件目录不存在，一并创建  
    > 注意事项：如果你创建文件或者文件目录没有写盘符路径，那么，默认在项目路径下  

* 删除
    > `public boolean delete()`：删除文件或者文件夹  
    > 删除注意事项：Java中的删除不走回收站。 要删除一个文件目录，请注意该文件目录内不能包含文件或者文件目录  

**常见案例:**  
```java
/**
 * 遍历指定目录所有文件名称，包括子文件目录中的文件。
 * 拓展1：并计算指定目录占用空间的大小
 * 拓展2：删除指定文件目录及其下的所有文件
 */
public class ListFilesTest {

    public static void main(String[] args) {
        // 递归:文件目录
        /** 打印出指定目录所有文件名称，包括子文件目录中的文件 */

        // 1.创建目录对象
        File dir = new File("src/io/file");

        // 2.打印目录的子文件
        System.out.println("------遍历目录下文件及子文件--------");
        printSubFile(dir);
        System.out.println("------遍历目录--------");
        listSubFiles(dir);
        System.out.println("-------遍历目录下文件及子文件-------");
        listAllSubFiles(dir);
        System.out.println("------列出指定目录大小--------");
        System.out.println(getDirectorySize(dir));
    }

    public static void printSubFile(File dir) {
        // 打印目录的子文件
        File[] subfiles = dir.listFiles();

        for (File f : subfiles) {
            if (f.isDirectory()) {// 文件目录
                printSubFile(f);
            } else {// 文件
                System.out.println(f.getAbsolutePath());
            }

        }
    }

    // 方式二：循环实现
    // 列出file目录的下级内容，仅列出一级的话
    // 使用File类的String[] list()比较简单
    public static void listSubFiles(File file) {
        if (file.isDirectory()) {
            String[] all = file.list();
            for (String s : all) {
                System.out.println(s);
            }
        } else {
            System.out.println(file + "是文件！");
        }
    }

    // 列出file目录的下级，如果它的下级还是目录，接着列出下级的下级，依次类推
    // 建议使用File类的File[] listFiles()
    public static void listAllSubFiles(File file) {
        if (file.isFile()) {
            System.out.println(file);
        } else {
            File[] all = file.listFiles();
            // 如果all[i]是文件，直接打印
            // 如果all[i]是目录，接着再获取它的下一级
            for (File f : all) {
                listAllSubFiles(f);// 递归调用：自己调用自己就叫递归
            }
        }
    }

    // 拓展1：求指定目录所在空间的大小
    // 求任意一个目录的总大小
    public static long getDirectorySize(File file) {
        // file是文件，那么直接返回file.length()
        // file是目录，把它的下一级的所有大小加起来就是它的总大小
        long size = 0;
        if (file.isFile()) {
            size += file.length();
        } else {
            File[] all = file.listFiles();// 获取file的下一级
            // 累加all[i]的大小
            for (File f : all) {
                size += getDirectorySize(f);// f的大小;
            }
        }
        return size;
    }

    // 拓展2：删除指定的目录
    public static void deleteDirectory(File file) {
        // 如果file是文件，直接delete
        // 如果file是目录，先把它的下一级干掉，然后删除自己
        if (file.isDirectory()) {
            File[] all = file.listFiles();
            // 循环删除的是file的下一级
            for (File f : all) {// f代表file的每一个下级
                deleteDirectory(f);
            }
        }
        // 删除自己
        file.delete();
    }

}
```
