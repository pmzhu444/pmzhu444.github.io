---
title: Java面试复习
date: 2021-04-02 16:03:45
tags: [笔记, 面试]
---

# Java面试

小菜鸡也是要找工作的嘛，虽然才研一，基础不好，早早就要开始看咯。反正看了也会忘，忘了就来这再看一遍呗~

---

本文是在网上到处搜集的喔，侵权删。

---

持续更新中~~~

<!-- more -->

<br>

## 1、面试

##### 1.1、介绍简历上的项目，自己主要做了什么？

##### 1.2、项目里给你最大的挑战是什么？遇到了什么问题？如何解决的？从中学到了什么？

- 项目里面会不断出现各种问题，比如数据量过大造成的内存溢出问题，如何让程序运行效率更高，如何证明我们的算法比别人的算法效率高，如何找到新的观点来支撑我们现有的理论，如何向导师和师兄进行沟通完成接下来的工作

##### 1.3、项目架构图能画一下吗？

##### 1.4、觉得项目有哪些地方可以改进完善（比如：可以加一个redis缓存把热点数据缓存起来）

##### 1.5、有没有遇到过内存泄漏的场景？

<br>

## 2、操作系统

#### 2.1、进程与线程的区别？

- 进程是资源分配的最小单位，线程是任务执行的最小单位。
- 进程有自己独立的地址空间，每启动一个进程，系统就会为他分配地址空间，建立数据表来维护代码段、数据栈和数据段，这种操作非常昂贵。而线程是共享进程中的数据的，使用相同的地址空间，因此CPU启动一个线程的花费远比进程要小的多，同时创建一个线程的开销也比进程要小很多。
- 线程之间的通信更方便，同一进程下的线程下线程共享全局变量、静态变量等数据，而进程之间的通信需要以通信的方式**（IPC）**进行。不过如何处理好同步与互斥是编写多线程程序的难点。
- 多线程程序更强壮，多线程程序只要有一个线程死掉，整个进程也死掉了，而一个进程死掉不会对另外的进程造成影响，因为进程有自己独立的地址空间。

```
进程间通信（IPC，Inter-Process Communication），指至少两个进程或线程间传送数据或信号的一些技术或方法。
```

#### 2.2、进程间通信

- 尝见的通信方式
  - 管道pipe：管道是一种半双工的通信方式，数据只能单向流动，而且只能在具有亲缘关系的进程间使用。进程的亲缘关系通常是指父子进程关系。
  - 命名管道FIFO：命名管道也是半双工的通信方式，但是它允许无亲缘关系进程间的通信。
  - 消息队列MessageQueue：消息队列是由消息的链表，存放在内核中并由消息队列标识符标识。消息队列克服了信号传递信息少、管道只能承载无格式字节流以及缓冲区大小受限等缺点。
  - 共享存储SharedMemory：共享内存就是映射一段能被其他进程所访问的内存，这段共享内存由一个进程创建，但多个进程都可以访问。**共享内存是最快的 IPC 方式，它是针对其他进程间通信方式运行效率低而专门设计的。**它往往与其他通信机制，如信号量，配合使用，来实现进程间的同步和通信。
  - 信号量Semaphore：信号量是一个计数器，可以用来控制多个进程对共享资源的访问。它常作为一种锁机制，防止某进程正在访问共享资源时，其他进程也访问该资源。因此，主要作为进程间以及同一进程内不同线程之间的同步手段。
  - 套接字Socket：套解字也是一种进程间通信机制，与其他通信机制不同的是，它可用于不同及其间的进程通信。
  - 信号 ( sinal ) ： 信号是一种比较复杂的通信方式，用于通知接收进程某个事件已经发生。
- 通信类型
  - 共享存储器系统
    - 基于共享数据结构的通信方式（仅适用于传递相对少量的数据，通信效率低，属于低级通信）
    - 基于共享存储区的通信方式
  - 管道通信系统
    - 管道是指用于连接一个读进程和一个写进程以实现它们之间通信的一个共享文件（pipe文件）
      - 管道机制需要提供一下几点的协调能力
        - 互斥，即当一个进程正在对pipe执行读/写操作时，其它进程必须等待
        - 同步，当一个进程将一定数量的数据写入，然后就去睡眠等待，直到读进程将数据取走，再去唤醒。读进程与之类似
        - 确定对方是否存在
  - 消息传递系统
    - 直接通信方式
      - 发送进程利用OS所提供的发送原语直接把消息发给目标进程
    - 间接通信方式
      - 发送和接收进程都通过共享实体（邮箱）的方式进行消息的发送和接收
  - 客户机服务器系统
    - 套接字 – 通信标识型的数据结构是进程通信和网络通信的基本构件
      - 基于文件型的 （当通信进程都在同一台服务器中）其原理类似于管道
      - 基于网络型的（非对称方式通信，发送者需要提供接收者命名。通信双方的进程运行在不同主机环境下被分配了一对套接字，一个属于发送进程，一个属于接收进程）
    - 远程过程调用和远程方法调用

#### 2.3、进程间的主要调度算法有哪些

- 先来先去服务 FCFS

- 时间片轮转法 RR

- 短作业优先 SJF

- 多级反馈队列调度算法

- 优先级调度 PSA

#### 2.4、僵尸进程产生的原因？

- 僵尸进程是指它的父进程没有等待(调用 wait/waitpid)。如果子进程先结束而父进程后结束，即子进程结束后，父进程还在继续运行但是并未调用 wait/waitpid 那子进程就会成为僵尸进程。但如果子进程后结束，即父进程先结束了，但没有调用 wait/waitpid 来等待子进程的结束， 此时子进程还在运行，父进程已经结束。那么并不会产生僵尸进程。应为每个进程结束时， 系统都会扫描当前系统中运行的所有进程，看看有没有哪个进程时刚刚结束的这个进程的子 进程，如果有就有 init 来接管它，成为它的父进程。
- 进程设置僵尸状态的目的是维护子进程的信息，以便父进程在以后某个时间获取。要在当前 进程中生成一个子进程，一般需要调用 fork 这个系统调用，fork 这个函数的特别之处在于一次调用，两次返回，一次返回到父进程中，一次返回到子进程中，可以通过返回值来判断其 返回点。如果子进程先于父进程退出， 同时父进程又没有调用 wait/waitpid，则该子进程将成为僵尸进程
- 在每个进程退出的时候，内核释放该进程所有的资源，包括打开的文件，占用的内存。但是 仍然保留了一些信息（如进程号 pid 退出状态 运行时间等）。这些保留的信息直到进程通过调用 wait/waitpid 时才会释放。这样就导致了一个问题，如果没有调用 wait/waitpid 的话，那 么保留的信息就不会释放。比如进程号就会被一直占用了。但系统所能使用的进程号的有限 的，如果产生大量的僵尸进程，将导致系统没有可用的进程号而导致系统不能创建进程。所 以我们应该避免僵尸进程。

#### 2.5、孤儿进程产生的原因？

孤儿进程：一个父进程退出，而它的一个或多个子进程还在运行，那么那些子进程将成为孤 儿进程。孤儿进程将被 init 进程(进程号为1)所收养，并由init进程对它们完成状态收集工作。孤儿进程是没有父进程的进程，管理孤儿进程这个重任就落到了 init 进程身上，因此孤儿进程并 不会有什么危害。

#### 2.6、讲一下虚拟内存。虚拟内存和物理内存的关系是什么？

- 虚拟内存使得应用程序认为它拥有一个连续的地址空间，而实际上，它通常是被分隔成多个物理内存碎片，还有一部分存储在外部磁盘存储器上，在需要时进行数据交换。

- 虚拟内存可以让程序可以拥有超过系统物理内存大小的可用内存空间。虚拟内存让每个进程拥有一片连续完整的内存空间。

  

- 局部性原理表现在以下两个方面：
  - 时间局部性 ：如果程序中的某条指令一旦执行，不久以后该指令可能再次执行；如果某数据被访问过，不久以后该数据可能再次被访问。
  - 空间局部性 ：一旦程序访问了某个存储单元，在不久之后，其附近的存储单元也将被访问。

操作系统将内存抽象成地址空间。每个程序拥有自己的地址空间，这个地址空间被分割成多个块， 每一块称为一页。这些页被映射到物理内存，但不需要映射到连续的物理内存，也不需要所有页都 必须在物理内存中。当程序引用到不在物理内存中的页时，会将缺失的部分从磁盘装入物理内存。

##### 页面置换算法

- OPT 页面置换算法（最佳页面置换算法）：所选择的被换出的页面将是最长时间内不再被访问， 通常可以保证获得最低的缺页率。

- FIFO（First In First Out） 页面置换算法（先进先出页面置换算法） : 总是淘汰最先进入内存的页面，即选择在内存中驻留时间最久的页面进行淘汰。

- LRU （Least Currently Used）页面置换算法（最近最久未使用页面置换算法）：将最近最久未使用的页面换出。需要在内存中维护一个所有页面的链表。当一个页面被访问时，将这个页面移到 链表表头。这样就能保证链表表尾的页面是最近最久未访问的。力扣-实现LRU

- LFU （Least Frequently Used）页面置换算法（最少使用页面置换算法）：该置换算法选择在之前时期使用最少的页面作为淘汰页。

<br>

## 3、Java基础

#### 3.1、创建对象的方式有哪几种？

- new Obj..()

- clone()：使用 Object 类的 clone 方法。

- 反射

  - 调用 public 无参构造器 ，若是没有，则会报异常：

    ```java
    Object o = clazz.newInstance();
    ```

  - 有带参数的构造函数的类，先获取到其构造对象，再通过该构造方法类获取实例：

    ```java
    //获取构造函数类的对象
    Constroctor constroctor = User.class.getConstructor(String.class);
    //  使用构造器对象的newInstance方法初始化对象
    Object obj =  constroctor.newInstance("name");
    ```

- 通过反序列化来创建对象： 实现 Serializable 接口。

## 4、大厂面试题

### 字节跳动

#### java实习一面

1. TCP连接过程，哪些过程，分别有什么用

   ---

   - 三次握手（连接）

     TCP连接的建立采用客户-服务器模式：主动发起连接建立的应用进程叫做客户，被动等待连接建立的应用进程叫做服务器

     - 第一次握手：客户端的应用进程主动打开，并向服务端发出请求报文段。其首部中：SYN=1,seq=x。
     - 第二次握手：服务器应用进程被动打开。若同意客户端的请求，则发回确认报文，其首部中：SYN=1,ACK=1,ack=x+1,seq=y。
     - 第三次握手：客户端收到确认报文之后，通知上层应用进程连接已建立，并向服务器发出确认报文，其首部：ACK=1,ack=y+1。当服务器收到客户端的确认报文之后，也通知其上层应用进程连接已建立。

     在这个过程中，通信双方的状态如下图，其中CLOSED：关闭状态、LISTEN：收听状态、SYN-SENT：同步已发送、SYN-RCVD：同步收到、ESTABLISHED：连接已建立
     
     <div align=center> {% asset_img 1.jpg This is an example image %}</div>
     
   - 四次挥手（关闭）

     由于TCP连接是全双工的，因此每个方向都必须单独进行关闭。这原则是当一方完成它的数据发送任务后就能发送一个FIN来终止这个方向的连接。收到一个 FIN只意味着这一方向上没有数据流动，一个TCP连接在收到一个FIN后仍能发送数据。首先进行关闭的一方将执行主动关闭，而另一方执行被动关闭。

     - TCP客户端发送一个FIN，用来关闭客户到服务器的数据传送。
     - 服务器收到这个FIN，它发回一个ACK，确认序号为收到的序号加1。和SYN一样，一个FIN将占用一个序号。
     - 服务器关闭客户端的连接，发送一个FIN给客户端。
     - 客户端发回ACK报文确认，并将确认序号设置为收到序号加1。

     <div align=center> {% asset_img 2.jpg This is an example image %}</div>

   - 状态介绍

     - CLOSED：关闭，初始状态
     - LISTEN：服务器端的某个socket处于监听状态，可接受连接
     - SYN_RCVD：接受到了SYN报文，服务器端的SOCKET在建立TCP连接时的三次握手会话过程中的一个中间状态，很短暂，当收到客户端的ACK报文后，它会进入到ESTABLISHED状态。
     - SYN_SENT：这个状态与SYN_RCVD遥想呼应，当客户端SOCKET执行CONNECT连接时，它首先发送SYN报文，因此也随即它会进入到了SYN_SENT状态，并等待服务端的发送三次握手中的第2个报文。SYN_SENT状态表示客户端已发送SYN报文。
     - ESTABLISHED：连接已经建立
     - FIN_WAIT_1：FIN_WAIT_1和FIN_WAIT_2状态的真正含义都是表示等待对方的FIN报文。而这两种状态的区别是：FIN_WAIT_1状态实际上是当SOCKET在ESTABLISHED状态时，它想主动关闭连接，向对方发送了FIN报文，此时该SOCKET即进入到FIN_WAIT_1状态。而当对方回应ACK报文后，则进入到FIN_WAIT_2状态，当然在实际的正常情况下，无论对方何种情况下，都应该马上回应ACK报文，所以FIN_WAIT_1状态一般是比较难见到的，而FIN_WAIT_2状态还有时常常可以用netstat看到。
     - FIN_WAIT_2：FIN_WAIT_2状态下的SOCKET，表示半连接，也即有一方要求close连接，但另外还告诉对方，我暂时还有点数据需要传送给你，稍后再关闭连接这就是著名的半关闭的状态了，这是在关闭连接时，客户端和服务器两次握手之后的状态。在这个状态下，应用程序还有接受数据的能力，但是已经无法发送数据，但是也有一种可能是，客户端一直处于FIN_WAIT_2状态，而服务器则一直处于WAIT_CLOSE状态，而直到应用层来决定关闭这个状态。
     - TIME_WAIT：表示收到了对方的FIN报文，并发送出了ACK报文，就等2MSL后即可回到CLOSED可用状态了。如果FIN_WAIT_1状态下，收到了对方同时带FIN标志和ACK标志的报文时，可以直接进入到TIME_WAIT状态，而无须经过FIN_WAIT_2状态。
     - CLOSING：这种状态比较特殊，实际情况中应该是很少见，属于一种比较罕见的例外状态。正常情况下，当你发送FIN报文后，按理来说是应该先收到（或同时收到）对方的ACK报文，再收到对方的FIN报文。但是CLOSING状态表示你发送FIN报文后，并没有收到对方的ACK报文，反而却也收到了对方的FIN报文。什么情况下会出现此种情况呢？其实细想一下，也不难得出结论：那就是如果双方几乎在同时close一个SOCKET的话，那么就出现了双方同时发送FIN报文的情况，也就会出现CLOSING状态，表示双方都正在关闭SOCKET连接。
     - CLOSE_WAIT：这种状态的含义其实是表示在等待关闭。怎么理解呢？当对方close一个SOCKET后发送FIN报文给自己，你系统毫无疑问地会回应一个ACK报文给对方，此时则进入到CLOSE_WAIT状态。接下来呢，实际上你真正需要考虑的事情是查看你是否还有数据发送给对方，如果没有的话，那么你也就可以close这个SOCKET，发送FIN报文给对方，也即关闭连接。所以你在CLOSE_WAIT状态下，需要完成的事情是等待你去关闭连接。
     - LAST_ACK：这个状态还是比较容易好理解的，它是被动关闭一方在发送FIN报文后，最后等待对方的ACK报文。当收到ACK报文后，也即可以进入到CLOSED可用状态了。

   ---

2. 序列号是随机取的吗

   ---

   **是**

   在TCP的三次握手中，采用随机产生的初始化序列号进行请求，这样做主要是出于网络安全的因素着想。如果不是随机产生初始序列号，黑客将会以很容易的方式获取到你与其他主机之间通信的初始化序列号，并且伪造序列号进行攻击，这已经成为一种很常见的网络攻击手段。

   ---

3. 拥塞控制

   ---

   - 网络拥塞：在某段时间，若**对网络中某一资源的需求超过了该资源所能提供的可用部分，网络性能就要变坏**

     <div align=center> {% asset_img 3.png This is an example image %}</div>

   **TCP的四种拥塞控制算法**

   - 慢开始
   - 拥塞避免
   - 快重传
   - 快恢复

   **过程举例**

   传输轮次：发送方给接收方发送数据报文段后，接收方给发送方发回相应的确认报文段，一个传输轮次所经历的时间就是往返时间RTT(RTT并非是恒定的数值）

   使用传输轮次是为了强调，把拥塞窗口cwnd所允许发送的报文段都连续发送出去，并收到了对已发送的最后一个报文段的确认，拥塞窗口cwnd会随着网络拥塞程度以及所使用的拥塞控制算法动态变化。

   在tcp双方建立逻辑链接关系时， 拥塞窗口cwnd的值被设置为1，还需设置慢开始门限ssthresh,在执行慢开始算法时，发送方每收到一个对新报文段的确认时，就把拥塞窗口cwnd的值加一，然后开始下一轮的传输，当拥塞窗口cwnd增长到慢开始门限值时，就使用拥塞避免算法。

   - 慢开始

   假设当前发送方拥塞窗口cwnd的值为1，而发送窗口swnd等于拥塞窗口cwnd，因此发送方当前只能发送一个数据报文段（拥塞窗口cwnd的值是几，就能发送几个数据报文段），接收方收到该数据报文段后，给发送方回复一个确认报文段，发送方收到该确认报文后，将拥塞窗口的值变为2，发送方此时可以连续发送两个数据报文段，接收方收到该数据报文段后，给发送方一次发回2个确认报文段，发送方收到这两个确认报文后，将拥塞窗口的值加2变为4，发送方此时可连续发送4个报文段，接收方收到4个报文段后，给发送方依次回复4个确认报文，发送方收到确认报文后，将拥塞窗口加4，置为8，发送方此时可以连续发送8个数据报文段，接收方收到该8个数据报文段后，给发送方一次发回8个确认报文段，发送方收到这8个确认报文后，将拥塞窗口的值加8变为16，以此类推

   当前的拥塞窗口cwnd的值已经等于慢开始门限值，之后改用拥塞避免算法。

   - 拥塞避免

   每个传输轮次，拥塞窗口cwnd只能线性加一，而不是像慢开始算法时，每个传输轮次，拥塞窗口cwnd按指数增长。

   同理，16+1……直至到达24，假设24个报文段在传输过程中丢失4个，接收方只收到20个报文段，给发送方依次回复20个确认报文段，一段时间后，丢失的4个报文段的重传计时器超时了，发送发判断可能出现拥塞，更改cwnd和ssthresh.并重新开始慢开始算法，如图所示：

   <div align=center> {% asset_img 4.png This is an example image %}</div>

   <div align=center> {% asset_img 5.png This is an example image %}</div>

   - 快重传

   当发送方收到了累计3个连续的针对某号报文段的重复确认，立即重传该报文段，接收方收到后，给发送方发回针对最后收到的报文的确认，表明，这个报文之前的的报文都收到了，这样就不会造成发送方对某个报文的超时重传，而是提早实现了重传。

   <div align=center> {% asset_img 6.png This is an example image %}</div>

   - 快恢复

   当发送方收到了三个重复的确认时，就知道现在只是丢了个别的报文段，于是不启动慢开始算法，而是执行快恢复算法。

   <div align=center> {% asset_img 7.png This is an example image %}</div>

4. MySQL索引

   - 一般的应用系统，读写比例在10:1左右，而且插入操作和一般的更新操作很少出现性能问题，在生产环境中，我们遇到最多的，也是最容易出问题的，还是一些复杂的查询操作，因此对查询语句的优化显然是重中之重。说起加速查询，就不得不提到索引了。
   - 为什么要有索引
     - 索引在MySQL中也叫做“键”，是存储引擎用于快速找到记录的一种数据结构。
     - 索引对于良好的性能非常关键，尤其是当表中的数据量越来越大时，索引对于性能的影响愈发重要。
     - 索引优化应该是对查询性能优化最有效的手段了。索引能够轻易将查询性能提高好几个数量级。
     - 索引相当于字典的音序表，如果要查某个字，如果不使用音序表，则需要从几百页中逐页去查。
   - 索引原理

   **通过不断地缩小想要获取数据的范围来筛选出最终想要的结果，同时把随机的事件变成顺序的事件，也就是说，有了这种索引机制，我们可以总是用同一种查找方式来锁定数据。**

   - 磁盘IO与预读

   考虑到磁盘IO是非常高昂的操作，计算机操作系统做了一些优化，**当一次IO时，不光把当前磁盘地址的数据，而是把相邻的数据也都读取到内存缓冲区内**，因为局部预读性原理告诉我们，当计算机访问一个地址的数据的时候，与其相邻的数据也会很快被访问到。每一次IO读取的数据我们称之为一页(page)。具体一页有多大数据跟操作系统有关，一般为4k或8k，也就是我们读取一页内的数据时候，实际上才发生了一次IO，这个理论对于索引的数据结构设计非常有帮助。

   - **索引的数据结构**

   每次查找数据时把磁盘IO次数控制在一个很小的数量级，最好是常数数量级。那么我们就想到如果一个高度可控的多路搜索树是否能满足需求呢？就这样，b+树应运而生。

   <div align=center> {% asset_img 8.jpg This is an example image %}</div>

   浅蓝色的块我们称之为一个磁盘块，可以看到每个磁盘块包含几个数据项（深蓝色所示）和指针（黄色所示），如磁盘块1包含数据项17和35，包含指针P1、P2、P3，P1表示小于17的磁盘块，P2表示在17和35之间的磁盘块，P3表示大于35的磁盘块。真实的数据存在于叶子节点即3、5、9、10、13、15、28、29、36、60、75、79、90、99。非叶子节点只不存储真实的数据，只存储指引搜索方向的数据项，如17、35并不真实存在于数据表中。

   - b+树查找过程

   如图所示，如果要查找数据项29，那么首先会把磁盘块1由磁盘加载到内存，此时发生一次IO，在内存中用二分查找确定29在17和35之间，锁定磁盘块1的P2指针，内存时间因为非常短（相比磁盘的IO）可以忽略不计，通过磁盘块1的P2指针的磁盘地址把磁盘块3由磁盘加载到内存，发生第二次IO，29在26和30之间，锁定磁盘块3的P2指针，通过指针加载磁盘块8到内存，发生第三次IO，同时内存中做二分查找找到29，结束查询，总计三次IO。真实的情况是，3层的b+树可以表示上百万的数据，如果上百万的数据查找只需要三次IO，性能提高将是巨大的，如果没有索引，每个数据项都要发生一次IO，那么总共需要百万次的IO，显然成本非常非常高。

   - b+树的性质

     - **索引字段要尽量的小**

       IO次数取决于b+数的高度h，假设当前数据表的数据为N，每个磁盘块的数据项的数量是m，则有$h=log_{m+1}N$，当数据量N一定的情况下，m越大，h越小；而m = 磁盘块的大小 / 数据项的大小，磁盘块的大小也就是一个数据页的大小，是固定的，如果数据项占的空间越小，数据项的数量越多，树的高度越低。这就是为什么每个数据项，即索引字段要尽量的小，比如int占4字节，要比bigint8字节少一半。

       **这也是为什么b+树要求把真实的数据放到叶子节点而不是内层节点，一旦放到内层节点，磁盘块的数据项会大幅度下降，导致树增高。当数据项等于1时将会退化成线性表。**

     - **索引的最左匹配特性（即从左往右匹配）**

       当b+树的数据项是复合的数据结构，比如(name,age,sex)的时候，b+数是按照从左到右的顺序来建立搜索树的，比如当(张三,20,F)这样的数据来检索的时候，b+树会优先比较name来确定下一步的所搜方向，如果name相同再依次比较age和sex，最后得到检索的数据；但当(20,F)这样的没有name的数据来的时候，b+树就不知道下一步该查哪个节点，因为建立搜索树的时候name就是第一个比较因子，必须要先根据name来搜索才能知道下一步去哪里查询。比如当(张三,F)这样的数据来检索时，b+树可以用name来指定搜索方向，但下一个字段age的缺失，所以只能把名字等于张三的数据都找到，然后再匹配性别是F的数据了， 这个是非常重要的性质，即索引的最左匹配特性。
     
   - 

5. B+树对比B树的好处

   **B+树内节点不存储数据，所有 data 存储在叶节点导致查询时间复杂度固定为 log n。而B-树查询时间复杂度不固定，与 key 在树中的位置有关，最好为O(1)。**

   上一个B+树的性质

6. 乐观锁在哪用到，与悲观锁的区别

   - 乐观锁对应于生活中乐观的人总是想着事情往好的方向发展，悲观锁对应于生活中悲观的人总是想着事情往坏的方向发展。这两种人各有优缺点，不能不以场景而定说一种人好于另外一种人。

   - 悲观锁

     总是假设最坏的情况，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会阻塞直到它拿到锁（**共享资源每次只给一个线程使用，其它线程阻塞，用完后再把资源转让给其它线程**）。传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在做操作之前先上锁。Java中`synchronized`和`ReentrantLock`等独占锁就是悲观锁思想的实现。

   - 乐观锁

     总是假设最好的情况，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号机制和CAS算法实现。**乐观锁适用于多读的应用类型，这样可以提高吞吐量**，像数据库提供的类似于**write_condition机制**，其实都是提供的乐观锁。在Java中`java.util.concurrent.atomic`包下面的原子变量类就是使用了乐观锁的一种实现方式**CAS**实现的。

   - 使用场景

     从上面对两种锁的介绍，我们知道两种锁各有优缺点，不可认为一种好于另一种，像**乐观锁适用于写比较少的情况下（多读场景）**，即冲突真的很少发生的时候，这样可以省去了锁的开销，加大了系统的整个吞吐量。但如果是多写的情况，一般会经常产生冲突，这就会导致上层应用会不断的进行retry，这样反倒是降低了性能，所以**一般多写的场景下用悲观锁就比较合适。**

   - 乐观锁的两种实现方式

     - 版本号机制

       一般是在数据表中加上一个数据版本号version字段，表示数据被修改的次数，当数据被修改时，version值会加一。当线程A要更新数据值时，在读取数据的同时也会读取version值，在提交更新时，若刚才读取到的version值为当前数据库中的version值相等时才更新，否则重试更新操作，直到更新成功。

     - CAS算法

       即**compare and swap（比较与交换）**，是一种有名的**无锁算法**。无锁编程，即不使用锁的情况下实现多线程之间的变量同步，也就是在没有线程被阻塞的情况下实现变量的同步，所以也叫非阻塞同步（Non-blocking Synchronization）。**CAS算法**涉及到三个操作数

       - 需要读写的内存值 V
       - 进行比较的值 A
       - 拟写入的新值 B

       当且仅当 V 的值等于 A时，CAS通过原子方式用新值B来更新V的值，否则不会执行任何操作（比较和替换是一个原子操作）。一般情况下是一个**自旋操作**，即**不断的重试**。

   - 乐观锁的缺点

     - ABA问题

       如果一个变量V初次读取的时候是A值，并且在准备赋值的时候检查到它仍然是A值，那我们就能说明它的值没有被其他线程修改过了吗？很明显是不能的，因为在这段时间它的值可能被改为其他值，然后又改回A，那CAS操作就会误认为它从来没有被修改过。这个问题被称为CAS操作的 **"ABA"问题。**

       JDK 1.5 以后的 `AtomicStampedReference 类`就提供了此种能力，其中的 `compareAndSet 方法`就是首先检查当前引用是否等于预期引用，并且当前标志是否等于预期标志，如果全部相等，则以原子方式将该引用和该标志的值设置为给定的更新值。

     - 循环时间长，开销大

       **自旋CAS（也就是不成功就一直循环执行直到成功）如果长时间不成功，会给CPU带来非常大的执行开销。** 如果JVM能支持处理器提供的pause指令那么效率会有一定的提升，pause指令有两个作用，第一它可以延迟流水线执行指令（de-pipeline）,使CPU不会消耗过多的执行资源，延迟的时间取决于具体实现的版本，在一些处理器上延迟时间是零。第二它可以避免在退出循环的时候因内存顺序冲突（memory order violation）而引起CPU流水线被清空（CPU pipeline flush），从而提高CPU的执行效率。

     - **只能保证一个共享变量的原子操作**

       CAS 只对单个共享变量有效，当操作涉及跨多个共享变量时 CAS 无效。但是从 JDK 1.5开始，提供了`AtomicReference类`来保证引用对象之间的原子性，你可以把多个变量放在一个对象里来进行 CAS 操作.所以我们可以使用锁或者利用`AtomicReference类`把多个共享变量合并成一个共享变量来操作。

   - CAS与synchronized的使用场景

     **简单的来说CAS适用于写比较少的情况下（多读场景，冲突一般较少），synchronized适用于写比较多的情况下（多写场景，冲突一般较多）**

     1. 对于资源竞争较少（线程冲突较轻）的情况，使用synchronized同步锁进行线程阻塞和唤醒切换以及用户态内核态间的切换操作额外浪费消耗cpu资源；而CAS基于硬件实现，不需要进入内核，不需要切换线程，操作自旋几率较少，因此可以获得更高的性能。
     2. 对于资源竞争严重（线程冲突严重）的情况，CAS自旋的概率会比较大，从而浪费更多的CPU资源，效率低于synchronized。

7. synchronized和Reentrant Lock的区别

   - 相似点

      这两种同步方式有很多相似之处，它们都是加锁方式同步，而且都是阻塞式的同步，也就是说当如果一个线程获得了对象锁，进入了同步块，其他访问该同步块的线程都必须阻塞在同步块外面等待，而进行线程阻塞和唤醒的代价是比较高的（操作系统需要在用户态与内核态之间来回切换，代价很高，不过可以通过对锁优化进行改善）。

   - 区别

     这两种方式最大区别就是对于Synchronized来说，它是java语言的关键字，是原生语法层面的互斥，需要jvm实现。而ReentrantLock它是JDK 1.5之后提供的API层面的互斥锁，需要lock()和unlock()方法配合try/finally语句块来完成。

8. synchronized底层技术，为什么慢

   - synchronized特性

     - 原子性：**一个操作或者多个操作，要么全部执行并且执行的过程不会被任何因素打断，要么就都不执行。**

     - 可见性：**多个线程访问一个资源时，该资源的状态、值信息等对于其他线程都是可见的**

       synchronized对一个类或对象加锁时，一个线程如果要访问该类或对象必须先获得它的锁，而这个锁的状态对于其他任何线程都是可见的，并且在释放锁之前会将对变量的修改刷新到主存当中，保证资源变量的可见性，如果某个线程占用了该锁，其他线程就必须在锁池中等待锁的释放。

     - 有序性：**有序性值程序执行的顺序按照代码先后执行。**

     - 可重入性：当一个线程试图操作一个由其他线程持有的对象锁的临界资源时，将会处于阻塞状态，但当一个线程再次请求自己持有对象锁的临界资源时，这种情况属于重入锁。通俗一点讲就是说一个线程拥有了锁仍然还可以重复申请锁。

   - 字节码：ACC_SYNCHORNIZED标志：这标志用来告诉JVM这是一个同步方法，在进入该方法之前先获取相应的锁，锁的计数器加1，方法结束后计数器-1，如果获取失败就阻塞住，直到该锁被释放。

   - synchronized底层实现

     在JVM中，对象是分成三部分存在的：对象头、实例数据、对齐填充。

     实例数据和对其填充与synchronized无关。实例数据存放类的属性数据信息，包括父类的属性信息，如果是数组的实例部分还包括数组的长度，这部分内存按4字节对齐；对其填充不是必须部分，由于虚拟机要求对象起始地址必须是8字节的整数倍，对齐填充仅仅是为了使字节对齐。

     对象头是synchronized实现锁的基础，因为synchronized申请锁、上锁、释放锁都与对象头有关。对象头主要结构是由Mark Word 和 Class Metadata Address 组成，其中Mark Word存储对象的hashCode、锁信息或分代年龄或GC标志等信息，Class Metadata Address是类型指针指向对象的类元数据，JVM通过该指针确定该对象是哪个类的实例。

     锁也分不同状态，JDK6之前只有两个状态：无锁、有锁（重量级锁），而在JDK6之后对synchronized进行了优化，新增了两种状态，总共就是四个状态：无锁状态、偏向锁、轻量级锁、重量级锁，其中无锁就是一种状态了。锁的类型和状态在对象头Mark Word中都有记录，在申请锁、锁升级等过程中JVM都需要读取对象的Mark Word数据。

     每一个锁都对应一个monitor对象，在HotSpot虚拟机中它是由ObjectMonitor实现的（C++实现）。每个对象都存在着一个monitor与之关联，对象与其monitor之间的关系有存在多种实现方式，如monitor可以与对象一起创建销毁或当线程试图获取对象锁时自动生成，但当一个monitor被某个线程持有后，它便处于锁定状态。

   - 缺点：

     - 效率低

       synchronized关键字是不可中断的，这也就意味着一个等待的线程如果不能获取到锁将会一直等待，而不能再去做其他的事了（synchronized关键字是不可中断的，这也就意味着一个等待的线程如果不能获取到锁将会一直等待，而不能再去做其他的事了）。

     - 不够灵活

       加锁和解锁的时候，每个锁只能有一个对象处理，这对于目前分布式等思想格格不入。

     - 无法知道是否成功获取到锁

       也就是我们的锁如果获取到了，我们无法得知。既然无法得知我们也就很不容易进行改进。

   - JVM对synchronized的优化

     - 锁膨胀

       上面讲到锁有四种状态，并且会因实际情况进行膨胀升级，其膨胀方向是：**无锁——>偏向锁——>轻量级锁——>重量级锁**，并且膨胀方向不可逆。

       - 偏向锁

         **减少同一线程获取锁的代价。**在大多数情况下，锁不存在多线程竞争，总是由同一鲜橙多次获得，那么此时就是偏向锁。

         核心思想：如果一个线程获得了锁，那么锁就进入偏向模式，此时Mark Word的结构也就变为偏向锁结构，当该线程再次请求锁时，无需再做任何同步操作，即获取锁的过程只需要检查Mark Word的锁标记位为偏向锁以及当前线程ID等于Mark Word的ThreadID即可，这样就省去了大量有关锁申请的操作。

       - 轻量级锁

         轻量级锁是由偏向锁升级而来，当存在第二个线程申请同一个锁对象时，偏向锁就会立即升级为轻量级锁。注意这里的第二个线程只是申请锁，不存在两个线程同时竞争锁，可以是一前一后地交替执行同步块

       - 重量级锁是由轻量级锁升级而来，当**同一时间**有多个线程竞争锁时，锁就会被升级成重量级锁，此时其申请锁带来的开销也就变大。

         重量级锁一般使用场景会在追求吞吐量，同步块或者同步方法执行时间较长的场景。

     - 锁消除

       **消除锁是虚拟机另外一种锁的优化，这种优化更彻底，在JIT编译时，对运行上下文进行扫描，去除不可能存在竞争的锁。**

     - 锁粗化

       **锁粗化是虚拟机对另一种极端情况的优化处理，通过扩大锁的范围，避免反复加锁和释放锁。**

     - 自旋锁与自适应锁

       轻量级锁失败后，虚拟机为了避免线程真实地在操作系统层面挂起，还会进行一项称为自旋锁的优化手段。

       - **自旋锁：**许多情况下，共享数据的锁定状态持续时间较短，切换线程不值得，通过让线程执行循环等待锁的释放，不让出CPU。如果得到锁，就顺利进入临界区。如果还不能获得锁，那就会将线程在操作系统层面挂起，这就是自旋锁的优化方式。但是它也存在缺点：如果锁被其他线程长时间占用，一直不释放CPU，会带来许多的性能开销。
       - **自适应自旋锁**：这种相当于是对上面自旋锁优化方式的进一步优化，它的自旋的次数不再固定，其自旋的次数由前一次在同一个锁上的自旋时间及锁的拥有者的状态来决定，这就解决了自旋锁带来的缺点。

9. synchronized 和 volatile

   对于volatile关键字，当且仅当满足以下所有条件时可使用：

   - 对变量的写入操作不依赖变量的当前值，或者你能确保只有单个线程更新变量的值。
   - 该变量没有包含在具有其他变量的不变式中。

   区别：

   - volatile本质是在告诉jvm当前变量在寄存器（工作内存）中的值是不确定的，需要从主存中读取； synchronized则是锁定当前变量，只有当前线程可以访问该变量，其他线程被阻塞住。
   - volatile仅能使用在变量级别；synchronized则可以使用在变量、方法、和类级别的
   - volatile仅能实现变量的修改可见性，不能保证原子性；而synchronized则可以保证变量的修改可见性和原子性
   - volatile不会造成线程的阻塞；synchronized可能会造成线程的阻塞。
   - volatile标记的变量不会被编译器优化；synchronized标记的变量可以被编译器优化

10. 幻读是什么？MySQL怎么解决幻读

    - **幻读指的是一个事务在前后两次查询同一个范围的时候，后一次查询看到了前一次查询没有看到的行。**

    - 说明：
      - 在可重复读隔离级别下，普通的查询是快照读，是不会看到别的事务插入的数据的。因此，幻读在“当前读”下才会出现。
      - 修改过的行用“当前读”看到，不能称为幻读。幻读仅专指“新插入的行”。

    - 问题：

      当几个事务同时操作数据库时，即使把所有的记录都加上锁，也阻止不了新插入的记录，这时就会发生数据库的不一致性。

    - 间隙锁（GapLock）

      **跟间隙锁存在冲突关系的，是“往这个间隙中插入一个记录”这个操作。**间隙锁之间都不存在冲突关系。

      行锁，分成读锁和写锁。跟行锁有冲突关系的是“另外一个行锁”。

      间隙锁和行锁合称next-key lock，每个next-key lock是前开后闭区间。也就是说，我们的表t初始化以后，如果用select * from t for update要把整个表所有记录锁起来，就形成了7个next-key lock，分别是 (-∞,0]、(0,5]、(5,10]、(10,15]、(15,20]、(20, 25]、(25, +supremum]。（一个表有六条记录：0、5、10、15、20、25）（InnoDB给每个索引加了一个不存在的最大值supremum，这样才符合我们前面说的“都是前开后闭区间”）

11. 介绍一下ES

#### java实习二面

1. JAVA 异步实现，线程池，es 索引
2. MySQL innodb 索引聚簇/非聚簇索引联合索引order by 必须用非聚簇索引怎么加快效率
3. 场景设计题。模拟微博，可以关注人，取关，微博按照关注的人时间倒序
4. 如何优化
5. ThreadLocal
6. MySQL 锁，解决幻读，怎么实现可重复读
7. 堆排序
8. 算法题。z 型遍历二叉树

#### java 实习三面

1. ES/kafka

2. 智力题。圆形湖中间一只鸭，岸边一只老虎，鸭的速度为s，老虎速度为4s，湖半径为r，鸭子到岸边即可安全逃脱，问什么情况下鸭子能顺利逃脱

3. 编程题。k 个一组反转列表，不足k 个也要反转

   ```java
   class Solution {
       public ListNode reverseKGroup(ListNode head, int k) {
           ListNode hair = new ListNode(0);
           hair.next = head;
           ListNode pre = hair;
   
           while (head != null) {
               ListNode tail = pre;
               // 查看剩余部分长度是否大于等于 k
               for (int i = 0; i < k; ++i) {
                   if (tail.next == null) {
                       break;
                   }
                   tail = tail.next;
               }
               ListNode nex = tail.next;
               ListNode[] reverse = myReverse(head, tail);
               head = reverse[0];
               tail = reverse[1];
               // 把子链表重新接回原链表
               pre.next = head;
               tail.next = nex;
               pre = tail;
               head = tail.next;
           }
   
           return hair.next;
       }
   
       public ListNode[] myReverse(ListNode head, ListNode tail) {
           ListNode prev = tail.next;
           ListNode p = head;
           while (prev != tail) {
               ListNode nex = p.next;
               p.next = prev;
               prev = p;
               p = nex;
           }
           return new ListNode[]{tail, head};
       }
   }
   ```

   

#### java后端日常实习

1. ping 是通过什么协议实现的？

   使用的是 ICMP协议，是“Internet Control Message Protocol”（Internet控制消息协议）的缩写，是 TCP/IP协议族的一个子协议，用于在IP主机、路由器之间传递控制消息。

2. DNS 的主要过程

3. 如果服务器的ip 地址发生了变化，如何通知给DNS 服务器？

4. TCP 中的CLOSE-WAIT 状态是什么，为什么要有CLOSE-WAIT 状态？

5. 如何实现UDP 的可靠数据传输？

6. 路由器和交换机具体都实现了什么功能？路由选择是如何实现的？

7. 介绍一下IPv6