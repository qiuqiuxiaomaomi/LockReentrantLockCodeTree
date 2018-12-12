# LockReentrantLockCodeTree
Lock源码分析技术研究


<pre>
可重入锁 VS 不可重入锁

      可重入锁：
               即若当前线程执行某个方法已经获取了该锁，那么在方法中尝试再次获取该锁时，
      将可以获取到锁，锁计数+1，不会被阻塞。

      不可重入锁：

               所谓不可重入锁，即若当前线程执行某个方法已经获取了该锁，那么在方法中尝试
      再次获取该锁时，将被阻塞。        
</pre>

<pre>
synchronized的缺陷
            
           synchronized是Java中的一个关键字，也就是说是Java语言内置的特性。

           如果一个代码块被synchronized修饰了，当一个线程获取了对应的锁，并执行该代码
       块时，其他线程便只能一直等待，等待获取锁的线程释放锁，而这里获取锁的线程释放锁只
       会有两种情况：
　　       1）获取锁的线程执行完了该代码块，然后线程释放对锁的占有；
　　       2）线程执行发生异常，此时JVM会让线程自动释放锁。

           那么如果这个获取锁的线程由于等待IO或者其他原因（比如调用sleep方法）被阻塞
       了，但是又没有释放锁，其他线程只能等待，这会严重的影响程序执行的效率。

           因此，就需要有一种机制可以不让等待的线程一直无限期的等待下去，比如只等待一段
       时间或者能够响应中断，通过Lock就可以办到。

       再举个例子：当有多个线程读写文件时，读操作和写操作会发生冲突现象，写操作和写操作会发生冲突现象，但是读操作和读操作不会发生冲突现象。

　　       但是采用synchronized关键字来实现同步的话，就会导致一个问题：
　　           如果多个线程都只是进行读操作，所以当一个线程在进行读操作时，其他线程只能
           等待无法进行读操作。

　　       因此就需要一种机制来使得多个线程都只是进行读操作时，线程之间不会发生冲突，通
       过Lock就可以办到

          另外，通过Lock可以知道线程有没有成功获取到锁。这个是synchronized无法办到的。

          总结来说，Lock提供了比synchronized更多的功能，但是要注意以下几点：
              1）Lock不是Java语言内置的，synchronized是Java语言内置的，是关键字，
                 Lock是一个类，通过这个类可以实现同步。
              2）Lock和synchronized有一点非常大的不同，采用synchronized不需要用户手动
                 去释放锁，当synchronized方法或者synchronized代码块执行完以后，系统
                 自动会让线程释放对锁的占用；而Lock则必须要用户手动去释放锁，如果没有
                 主动释放锁，就有可能导致出现死锁。
</pre>

ReentrantLock公平锁加锁

![](https://i.imgur.com/hTNd8j0.png)

ReentrantLock非公平锁加锁

![](https://i.imgur.com/GQWrrWa.png)

<pre>
ReentrantLock：

      两种锁：
            nonfairTryAcquire 非公平锁
            fairTryAcquire 公平锁
</pre>