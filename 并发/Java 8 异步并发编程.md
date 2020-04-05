## Java 5 前时代
### 并发实现
- Java Green Thread
  受早期 Linux 内核限制，
- Java Native Thread

### 并发模型
- Thread
- Runnable

编程模型

```java
public class ThreadMain {

    public static void main(String[] args) {
        Thread thread = new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("Hello,World!");
            }
        },"Sub");

        thread.start();
        System.out.println("main");
    }
}
```



### 实现局限性

- 缺少线程管理的原生支持
- 缺少 “锁” API
- 缺少执行完成的原生支持
- 执行结果获取困难
- Double Check Locking 不确定性

## Java 5 时代

### 并发框架

- J.U.C = java.util.concurrent

### 编程模型

- Executor
- Runnable、Callback
- Future
- 

