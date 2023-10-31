# 实验二：RMI 应用





<center>
  姓名：王磊
</center>





<center>
  学号：2020211538
</center>





<center>
  提交日期：2023-06
</center>





## Contents

[toc]

## 实验内容

1. 设计并实现⼀个简单的RMI应⽤程序，该应⽤程序包含⼀个远程接⼝和⼀个远程对象。 
远程接⼝ Calculator 包含两个⽅法 add 和 subtract ，⽤于实现加法和减法操作。远程 
对象 CalculatorImpl 实现了上述远程接⼝中的两个⽅法，并且通过RMI⽅式向客户端提 
供服务。
2. 设计并实现⼀个HTTP服务器应⽤程序，该应⽤程序能够接收客户端的HTTP请求，并且 
根据请求的内容调⽤RMI应⽤程序中的远程对象，并且将结果返回给客户端。
3. 设计并实现⼀个HTTP客户机应⽤程序，该应⽤程序能够向HTTP服务器发送HTTP请求， 
并且接收服务器返回的结果。
4. 可⾃⾏添加部分功能，包括但不限于：异常处理、⽂件传输、注册登录等，视情况加 
分。

## 实验环境

1. 实验平台：MacOS13 Ventura
2. 编程语言：Java——JDK18
3. IDE：IntelliJ IDEA 2021 (Ultimate Edition)



## 编码设计

### Java 类

#### Calculator 远程接口

代码如下：

```java
public interface Calculator extends Remote {
    int add(int a, int b) throws RemoteException;
    int subtract(int a, int b) throws RemoteException;
}
```

功能：包含 add 和 substract 方法用于实现加法和减法操作

注：接口声明满足两点要求：

1. 扩展 Remote 接口
2. 方法抛出 RemoteException 异常

#### CalculatorImpl 远程对象

在远程对象中，实现了 add 和 substract 方法

```java
@Override
    public int add(int a, int b) throws RemoteException {
        return a + b;
    }

    @Override
    public int subtract(int a, int b) throws RemoteException {
        return a - b;
    }
```

### 执行程序

#### 远程对象注册程序

在 RegistryServer 程序中，实现了对 CalculatorImlp 对象的创建，然后将该对象注册到 RMI 注册表中，以便客户端可以查找和使用它。

#### Http 服务器程序

在 Http 服务器程序中，实现了对 Http 请求的接收和处理，当接收到一个 Http 请求时，该程序会解析请求中的参数，然后查找 RMI 注册表中的 Calculator 对象并调用相应的方法，然后将结果返回客户端。

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230630180426774.png" alt="image-20230630180426774" style="zoom:50%;" />

相应的处理逻辑由重写 handle 函数实现。

#### Http 客户端程序

最后创建了一个 Http 客户端应用程序，该程序向 Http 服务器发送请求，并接收和显示服务器返回的结果。

## 运行结果及分析

1. 首先运行远程对象注册程序，显示结果表明远程对象已就绪
   - <img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230630180541860.png" alt="image-20230630180541860" style="zoom:50%;" />
2. 运行 Http 服务器应用程序
3. 运行 Http 客户端应用程序，按照程序提示输入操作符和操作数
   - 加法运算
   - <img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230630180701553.png" alt="image-20230630180701553" style="zoom:50%;" />
   - 减法运算
   - ![image-20230630180756731](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230630180756731.png)

4. 结果无误，实验结束。

## 主要问题及解决方法

1. RMI 注册表的启动：我们在启动 RMI 注册表时遇到了问题，最初我们试图在命令行中手动启动 RMI 注册表，但是我们发现这样做很麻烦。后来，我们在程序中使用了 LocateRegistry.createRegistry 方法来启动 RMI 注册表，这样我们可以在启动应用程序时自动启动 RMI 注册表。
2. 请求参数的解析：在 HTTP 服务器应用程序中，我们需要解析 HTTP 请求的参数。我们最初试图使用 Java 的 URL 类的 getQuery 方法来获取请求参数，但是我们发现这种方法无法正确地解析参数。后来，我们直接操作字符串，用 "&" 和 "=" 分割字符串，成功地解析了请求参数。

## 源程序清单

### Calculator.java

```java
import java.rmi.Remote;
import java.rmi.RemoteException;

public interface Calculator extends Remote {
    int add(int a, int b) throws RemoteException;
    int subtract(int a, int b) throws RemoteException;
}

```

### CalculatorImpl.java

```java
import java.rmi.RemoteException;
import java.rmi.server.UnicastRemoteObject;

public class CalculatorImpl extends UnicastRemoteObject implements Calculator {
    protected CalculatorImpl() throws RemoteException {
        super();
    }

    @Override
    public int add(int a, int b) throws RemoteException {
        return a + b;
    }

    @Override
    public int subtract(int a, int b) throws RemoteException {
        return a - b;
    }
}

```

### RegistryServer.java

```java
import java.rmi.registry.LocateRegistry;
import java.rmi.registry.Registry;
import java.rmi.server.UnicastRemoteObject;

public class RegistryServer {
    public static void main(String args[]) {
        try {
            CalculatorImpl obj = new CalculatorImpl();
            // Unexport the remote object
            UnicastRemoteObject.unexportObject(obj, true);
            Calculator stub = (Calculator) UnicastRemoteObject.exportObject(obj, 0);

            // Start the RMI registry
            LocateRegistry.createRegistry(1099);

            // Bind the remote object's stub in the registry
            Registry registry = LocateRegistry.getRegistry();
            registry.bind("Calculator", stub);

            System.out.println("Calculator server ready");

            // ...do some work...


        } catch (Exception e) {
            System.out.println("Calculator server exception: " + e.getMessage());
        }

    }
}

```

### HttpServerApp.java

```java
import com.sun.net.httpserver.HttpServer;
import com.sun.net.httpserver.HttpHandler;
import com.sun.net.httpserver.HttpExchange;

import java.io.IOException;
import java.io.OutputStream;
import java.net.InetSocketAddress;
import java.rmi.registry.LocateRegistry;
import java.rmi.registry.Registry;

public class HttpServerApp {
    public static void main(String[] args) throws Exception {
        HttpServer server = HttpServer.create(new InetSocketAddress(8000), 0);
        server.createContext("/calculate", new CalculateHandler());
        server.start();
    }

    static class CalculateHandler implements HttpHandler {
        @Override
        public void handle(HttpExchange t) throws IOException {
            String response = "";
            try {
                String query = t.getRequestURI().getQuery();
                String[] parts = query.split("&");
                String operation = parts[0].split("=")[1];
                int num1 = Integer.parseInt(parts[1].split("=")[1]);
                int num2 = Integer.parseInt(parts[2].split("=")[1]);

                Registry registry = LocateRegistry.getRegistry();
                Calculator calculator = (Calculator) registry.lookup("Calculator");

                if(operation.equals("add")) {
                    int result = calculator.add(num1, num2);
                    response = Integer.toString(result);
                } else if(operation.equals("subtract")) {
                    int result = calculator.subtract(num1, num2);
                    response = Integer.toString(result);
                } else {
                    response = "Invalid operation";
                }

            } catch (Exception e) {
                response = "Error: " + e.getMessage();
            }

            t.sendResponseHeaders(200, response.length());
            OutputStream os = t.getResponseBody();
            os.write(response.getBytes());
            os.close();
        }
    }
}

```

### HttpClientApp.java

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.Scanner;

public class HttpClientApp {
    public static void main(String[] args) throws Exception {

        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter operation: ");
        String operation = scanner.nextLine();
        System.out.print("Enter first number: ");
        int num1 = scanner.nextInt();
        System.out.print("Enter second number: ");
        int num2 = scanner.nextInt();


        URL url = new URL("http://localhost:8000/calculate?operation=" + operation + "&num1=" + num1 + "&num2=" + num2);
        HttpURLConnection con = (HttpURLConnection) url.openConnection();
        con.setRequestMethod("GET");
        BufferedReader in = new BufferedReader(new InputStreamReader(con.getInputStream()));
        String inputLine;
        StringBuffer content = new StringBuffer();
        while ((inputLine = in.readLine()) != null) {
            content.append(inputLine);
        }
        in.close();
        con.disconnect();
        System.out.println("Response: " + content.toString());
    }
}

```

