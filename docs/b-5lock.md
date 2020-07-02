

## 2.5 锁 

### 2.5.1 公平锁和非公平锁

### 2.5.2 可重入锁（又名递归锁）
    
    可重入锁指的是同一个线程外层函数获得锁后，内层递归函数仍然能获得该锁的代码，在同一个线程在外层方法获取锁的时候，在进入内层方法会自动获取锁。

    ReentranLock和synchronized就是一个典型的可重入锁，可重入锁最大的作用就是避免死锁
    
    public synchronized void methord1(){
      methord2();
    }
    public synchronized void methord2(){
      
    }
    methord2()带不带synchronized都一样
### 2.5.3 独占锁/共享锁

### 2.5.4 自旋锁
```java
public class 自旋锁 {
  AtomicReference<Thread> atomicReference = new AtomicReference<>();

  public static void main(String[] args) throws InterruptedException {
      自旋锁 test = new 自旋锁();
      new Thread(()->{
          test.myLock();
          try {
              TimeUnit.SECONDS.sleep(5);
          } catch (InterruptedException e) {
              e.printStackTrace();
          }
          test.myUnLock();
      },"t1").start();
      TimeUnit.SECONDS.sleep(1);
      new Thread(()->{
          test.myLock();
          test.myUnLock();
      },"t2").start();
  }

  public void myLock() {
    Thread thread = Thread.currentThread();
    System.out.println(Thread.currentThread().getName() + "\t come in");
    while (!atomicReference.compareAndSet(null, thread)){}
  }

  public void myUnLock() {
      Thread thread = Thread.currentThread();
      atomicReference.compareAndSet(thread,null);
      System.out.println(Thread.currentThread().getName() + "\t come out");
  }
}
```


