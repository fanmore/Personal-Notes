```java
import java.net.InetAddress;
import java.net.UnknownHostException;

/**
 *  IP：定位一个节点
 *  InetAddress
 *      1、getLocalHost 获取本机
 *      2、getByName 通过域名或IP获取
 *  两个成员方法
 *      1、getHostAddress() 获得IP地址
 *      2、getHostName() 获得域名
 */

public class Demo01 {
    public static void main(String[] args) throws UnknownHostException {
        // 使用getLocalHost方法创建InetAddress对象
        InetAddress address1 = InetAddress.getLocalHost();
        System.out.println(address1.getHostAddress()); // 获取本机IP
        System.out.println(address1.getHostName()); // 获取本机名

        // 根据域名得到InetAddress对象
        InetAddress address2 = InetAddress.getByName("www.baidu.com"); // 输入域名
        System.out.println(address2.getHostAddress()); // 获取百度网的IP
        System.out.println(address2.getHostName()); // 获取百度的域名

        // 根据IP得到InetAddress对象
        InetAddress address3 = InetAddress.getByName("192.168.0.100");
        System.out.println(address3.getHostAddress()); // 获取IP
        System.out.println(address3.getHostName()); // 获得域名，如果IP不存在返回IP地址
    }
}
```
