## 入门

### JVM、JRE、JDK的区别

* JVM
  * Java Virtual Machine
  * Java虚拟机，能认识.class文件，将.class文件中的字节码指令进行识别并调用操作系统向上的API完成动作。
* JRE
  * Java Runtime Environment
  * Java运行时环境，包含两部分：
    * JVM的标准实现
    * Java基本类库
* JDK
  * Java Development Kit
  * Java开发工具包，集成了JRE和开发工具

JVM、JRE、JDK有层层嵌套关系。

### JDK的下载和安装

下载方式：搜索JDK即可，找到Oracle官方网站进行下载。

安装过程中不必再单独安装JRE。

### 环境变量的配置

需要配置：       

* JAVA_HOME     JDK安装路径
* PATH          %JAVA_HOME%\bin
* 检测方法：在CMD中输入java -version，能显示Java版本即配置成功。
