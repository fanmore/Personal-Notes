```java
import java.net.InetSocketAddress;

/**
 *  端口
 *  同一个协议下端口不能冲突
 */

public class Demo02 {
    public static void main(String[] args) {
        // 包含端口
        InetSocketAddress socketAddress1 = new InetSocketAddress("128.0.0.1", 8080);
        InetSocketAddress socketAddress2 = new InetSocketAddress("localhost", 9000);
        System.out.println(socketAddress1.getHostName());
        System.out.println(socketAddress2.getAddress());
    }
}
```
