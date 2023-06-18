# Network Programming Notes
<h2 align="left">目录</h2>
[toc]

Linux下， **socket对应一个文件描述符 fd**。
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/cn/socket.png)


## 五大 IO 模型
典型的一次I/O操作，包含两个阶段
1. 数据就绪：操作系统IO操作的就绪状态
    - 阻塞：用户进程阻塞，直到内核数据准备好和内核将数据从内核态拷贝到用户缓冲区，读取数据
    - 非阻塞：用户进程不阻塞，不管内核数据是否准备好，都立刻返回，返回错误 or 正确；如果返回错误，则用户进程一直轮询内核。
2. 数据读写：根据应用程序和内核的交互方式
    - 当 **「内核数据准备好」和「内核将数据从内核态拷贝到用户缓冲区」** 后，如果内核没有通知用户程序，用户程序需要自己读取用户程序缓冲区，这里有一个先后顺序，即用户程序缓冲区有数据才能读，内核执行和用户程序执行有先后顺序，因此是**同步的**；如果内核通知用户程序，那么用户程序就可以不用等内核执行完操作，表现的就没有先后顺序，因此是**异步的**。


### 阻塞I/O(同步)
**阻塞 I/O**：当用户进程(如read/write)会被阻塞，一直等到**内核数据准备好**，并**把数据从内核缓冲区拷贝到应用程序的缓冲区**中，当拷贝过程完成，用户进程(read/write) 才会返回。**阻塞等待的是「内核数据准备好」和「数据从内核态拷贝到用户态」这两个过程**

当**没有 client 连接请求时，server accept 会阻塞，一直等待，直到客户端连接请求到来**。
当 client1 与 server 建立连接后，当没有处理完时，client1 占用IO, 此时 client2 连接请求会被阻塞
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/cn/synchronous_blocking_IO.jpg)

这是一个**单线程过程，当某个 socket 阻塞，会影响到其他 socket 处理**。如果使用多线程，为每一个客户端分配一个线程，客户端较多时，会造成资源浪费，全部 socket 中可能每个时刻只有几个就绪。同时，线程的调度、上下文切换乃至它们占用的内存，可能都会成为瓶颈。


### 非阻塞I/O(同步)

**非阻塞I/O**：用户进程(如read/write)在数据未准备好的情况下立即返回，用户进程**不断轮询内核**，直到内核数据准备好。然后内核将数据拷贝到应用程序缓冲区，用户进程(如read/write)可以获取到结果。

同步非阻塞I/O**从操作系统层面解决了阻塞问题**。
非阻塞I/O不断轮询文件描述符集，进行系统调用，拷贝当前 fd 到内核。如果没有客户端请求（客户端阻塞），server 内核马上返回 accept_fd 为 -1，跳过该客户端，而不是阻塞等待。如果有客户端请求，server 内核返回 accept_fd > 0, 后续处理客户端请求：
- 方式1：主线程直接进行客户端请求的后续处理 
- 方式2：开辟新的子线程进行客户端请求的后续处理，主线程继续遍历 fd 集合
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/cn/synchronous_non-blocking_IO.jpg)

同步非阻塞I/O的优缺点：
- 优点：
  - 单个 socket 阻塞，不会影响到其他 socket 
- 缺点：
  - 需要不断的遍历 fd 集合，不断进行系统调用，有一定开销

### IO多路复用(同步)
轮询的过程中，应用程序啥也做不了。如果你的服务器需要和多个客户端保持连接，处理客户端的请求，属于多进程的并发问题，如果创建很多个进程来处理这些IO流，会导致CPU占有率很高。所以人们提出了I/O多路复用模型：**一个线程，通过记录I/O流的状态来同时管理多个I/O**。

#### select
select()函数允许程序监听多个文件描述符，内核拷贝文件描述符集合到内核，不断遍历内核中文件描述符集合。等待所监听的一个或者多个文件描述符变为就绪的状态。所谓的就绪状态是指：文件描述符不再是阻塞状态，可以用于某类IO操作了，包括可读，可写，发生异常三种。如果他们没有活动，则正确地将线程置于休眠状态，这时 CPU 会切换其他线程执行任务，等待客户端连接唤醒。

```c
/* According to POSIX.1-2001, POSIX.1-2008 */
#include <sys/select.h>
/* According to earlier standards */
#include <sys/time.h>
#include <sys/types.h>
#include <unistd.h>

/**
 * @param nfd 最大文件描述符 + 1：maxfd + 1
 * @param readfds 要监听的可读文件描述符集合
 * @param writefds 要监听的可写文件描述符集合
 * @param exceptfds 要监听的异常文件描述符集合
 * @param timeout 最多等待时间，0是不等待直接返回，-1表示一直等待。一般设置 timeout=0 
 * @param return 大于0：已就绪文件描述符数量（但没说明哪些文件描述符已经就绪）；等于0：超时； -1：出错，并设置errno
 *     EBADF：集合中包含无效的文件描述符。（文件描述符已经关闭了，或者文件描述符上已经有错误了）。
 *     EINTR：捕获到一个信号。
 *     EINVAL：nfds是负的或者timeout中包含的值无效。
 *     ENOMEM：无法为内部表分配内存。
 */ 
int select(int nfds, fd_set *readfds, fd_set *writefds,
           fd_set *exceptfds, struct timeval *timeout);
int pselect(int nfds, fd_set *readfds, fd_set *writefds,
            fd_set *exceptfds, const struct timespec *timeout,
            const sigset_t *sigmask);

void FD_CLR(int fd, fd_set *set);
int  FD_ISSET(int fd, fd_set *set);
void FD_SET(int fd, fd_set *set);
void FD_ZERO(fd_set *set);
```
select 复用fd_set(内核中通过位图实现)
入参时：*readfds-->监听了哪些fd
回参时：*readfds--> 哪些fd就绪了

select的优缺点：
- 优点：
  - 不需要每个 fd 都进行一次系统调用，解决了同步非阻塞I/O频繁地用户态和内核态切换问题
- 缺点:
  - 单进程监听的 fd 数量存在限制，默认1024
  - 每次调用需要将 fd 集合从用户态拷贝到内核态
  - 返回的是数量，不知道具体是哪个文件描述符就绪，需要遍历全部文件描述符（O(n)）
  - 入参的3个 fd_set 集合每次调用都需要重置


#### poll
**poll本质上和select没有区别，它将用户传入的数组拷贝到内核空间**，然后查询每个fd对应的设备状态，如果设备就绪则在设备等待队列中加入一项并继续遍历，如果遍历完所有fd后没有发现就绪设备，则挂起当前进程，直到设备就绪或者主动超时，被唤醒后它又要再次遍历fd。这个过程经历了多次无谓的遍历。

```cpp
 #include <poll.h>
/**
 * @param pollfd 要监听的文件描述符集合
 * @param nfds 文件描述符数量
 * @param timeout 大于0表示最多等待时间，0是不等待直接返回，-1表示一直等待。一般设置 timeout=0 
 * @param return 大于0：已就绪文件描述符数量（但没说明哪些文件描述符已经就绪）；等于0：超时； -1：出错，并设置errno
 */ 
int poll(struct pollfd *fds, nfds_t nfds, int timeout);

/**
 * fds：指向一个结构体数组的第0个元素的指针，每个数组元素都是一个struct pollfd结构，用于指定测试某个给定的fd的条件。
   如果 fd 是负数，poll 会忽略 events，并且返回的revents是0，可以在单次poll调用中，忽略某一个fd
 * events 是入参，输入对fd上感兴趣的事件，是个bitmask，0表示对所有事件都不感兴趣。
 * revents是出参，表示哪些事件发生了，也是个bitmask，可能会比 events 多出来三个值 POLLER, POLLHUP, POLLNVAL。
 * 
 */
 struct pollfd {
   int   fd;         /* file descriptor */
   short events;     /* requested events */
   short revents;    /* returned events */
};
```
poll的优缺点：
- 优点：
  - 不需要每个 fd 都进行一次系统调用，解决了同步非阻塞I/O频繁地用户态和内核态切换问题
- 缺点(优化了select两个缺点):
  - ~~<font color=Red>单进程监听的 fd 数量存在限制，默认1024</font>~~(原因是它是在内核中基于链表来存储的)
  - 每次调用需要将 fd 集合从用户态拷贝到内核态
  - 返回的是数量，不知道具体是哪个文件描述符就绪，需要遍历全部文件描述符（O(n)）
  - ~~<font color=Red>入参的3个 fd_set 集合每次调用都需要重置</font>~~(通过pollfd 结构体中events(入参)和revents(出参))


#### epoll
select 和 poll 就用一个函数与内核交互，而 epoll 多个功能分开，分别与内核交互。epoll维护基于回调的事件模型。**并发量和并发效率要求高时使用epoll**。

```cpp
#include <sys/epoll.h>

/**
 * 创建 epoll 事件驱动 
 * 
 * @param size: epoll 要监听的文件描述符数量
 * @param return epoll 返回的文件描述符 epfd
 */
int epoll_create(int size)

/**
 * 事件注册
 * 
 * @param epfd  epoll的文件描述符，epoll_create 创建时返回
 * @param op  操作类型：新增(1); 删除(2); 更新(3)
 * @param fd  本次操作的文件描述符
 * @param event 需要监听的事件：读事件、写事件等
 * @param 如果成功返回 0， 不成功返回 -1
 */
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);

typedef union epoll_data {
   void  *ptr;
   int   fd;
   uint32_t  u32;
   uint64_t  u64;
} epoll_data_t;
struct epoll_event {
   uint32_t events; /* Epoll events */
   epoll_data_t data; /* User data variable */
};

/**
 * 获取就绪事件，类似 select, poll 函数
 * 
 * @param epfd  epoll的文件描述符，epoll_create 创建时返回
 * @param events  用于回传就绪的事件
 * @param maxevents  每次能处理的最大事件数
 * @param timeout 大于0表示最多等待时间，0是不等待直接返回，-1表示一直等待(阻塞)。一般设置 timeout=0
 * @param 大于0：已就绪文件描述符数量；等于0：超时；-1：出错，并设置errno
 */
 int epoll_wait(int epfd, struct epoll_event *events,
                int maxevents, int timeout);
```

epoll 在内核中整体数据结构如下：
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/cn/epoll-data-structure.png)

**epoll_create 对应的底层数据结构是 eventpoll**
<div align=center>
   <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/cn/epoll_create.jpg" width = "80%" height = "80%">
</div>

epoll_ctl 为每一个 fd 创建 epitem   ，即在红黑树根 rbr 上添加节点

<div align=center>
   <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/cn/epoll_ctl.jpg" width = "80%" height = "80%">
</div>

epoll_wait 检测epoll红黑数上的就绪文件描述符集合 rdllist，返回就绪的描述符集合
<div align=center>
   <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/cn/epoll_wait.jpg" width = "80%" height = "80%">
</div>
 
```c
struct eventpoll {
	/* Protect the access to this structure */
	spinlock_t lock; //自旋锁,提供更细粒度的锁定,同时保护对这个结构的访问,比如向rdllist中添加数据等
	struct mutex mtx; 
	//最重要的作用就是确保文件描述符不会在使用时被删除,
	//在遍历rdllist,ctl操作中都会使用,这也告诉我们epoll是线程安全的

	/* Wait queue used by sys_epoll_wait() */
	wait_queue_head_t wq; //在epoll_wait中使用的等待队列,把进行epoll_wait的进程都加进去

	/* Wait queue used by file->poll() */
	wait_queue_head_t poll_wait;  //用于epollfd本身被poll的时候

	/* List of ready file descriptors */
	struct list_head rdllist; //就绪的文件描述符链表

	/* RB tree root used to store monitored fd structs */
	struct rb_root rbr; //存储被监听的fd结构的红黑树根

	/*
	 * This is a single linked list that chains all the "struct epitem" that
	 * happened while transferring ready events to userspace w/out
	 * holding ->lock.
	 */
	struct epitem *ovflist; 
	//我们在epoll_wait被唤醒后要把rdllist中的数据发往用户空间,
	//但可能这是也来了被触发的fd结构,这个结构的作用就是在使用rdllist期间把到来的fd结构加到ovflist中,这样可以不必加锁. epoll_ctl 新增 fd 会创建新的 epitem

	/* The user that created the eventpoll descriptor */
	struct user_struct *user;
};
```

```cpp
/*
 * Each file descriptor added to the eventpoll interface will
 * have an entry of this type linked to the "rbr" RB tree.
 */
 //Epoll每次被加入一个fd的时候就会创建一个epitem结构,表示一个被监听的fd.
struct epitem {
	/* RB tree node used to link this structure to the eventpoll RB tree */
	struct rb_node rbn; 
	//对应的红黑树节点,在epoll中已红黑树为主要结构管理fd,而每个fd对应一个epitem,其root保存在eventpoll,上面提到了,即rbr

	/* List header used to link this structure to the eventpoll ready list */
	struct list_head rdllink; //事件的就绪队列,已就绪的epitem会连接在rdllist中,

	/*
	 * Works together "struct eventpoll"->ovflist in keeping the
	 * single linked chain of items.
	 */
	struct epitem *next;

	/* The file descriptor information this item refers to */
	struct epoll_filefd ffd; //此epitem对应的fd和文件指针,用在红黑树中的比较操作,就相当于正常排序的小于号

	/* Number of active wait queue attached to poll operations */
	int nwait; //记录了poll触发回调的次数 epoll_ctl中有提及
	/* List containing poll wait queues */
	struct list_head pwqlist; //保存着被监视fd的等待队列

	/* The "container" of this item */
	struct eventpoll *ep;//该项属于哪个主结构体（多个epitm从属于一个eventpoll)

	/* List header used to link this item to the "struct file" items list */
	//这个实在没搞懂什么意思,参考别的博主的,
	struct list_head fllink; //file中有f_ep_link,用作连接所有监听file的epitem,链表加入的成员为fllink

	/* The structure that describe the interested events and the source fd */
	struct epoll_event event; //所注册的事件,和我们用户空间中见到的差不多
}
```

LT VS ET:
- LT：Level-triggered，水平（条件）触发，默认。epoll_wait 检测到事件后，如果该事件没被处理完毕，后续每次 epoll_wait 调用都会返回该事件(不会漏掉事件，但损耗性能)。
- ET：Edge-triggered，边缘触发。epoll_wait 检测到事件后，只会在第一次返回该事件，不管该事件是否被处理完毕(后续不再返回对应事件，效率较高，但不够安全)。


epoll的优缺点：
- 优点：
  - 不需要每个 fd 都进行一次系统调用，解决了同步非阻塞I/O频繁地用户态和内核态切换问题
- 缺点:
  - ~~<font color=Red>单进程监听的 fd 数量存在限制，默认1024</font>~~(poll中解决了这个问题)
  - ~~<font color=Red>每次调用需要将 fd 集合从用户态拷贝到内核态</font>~~(**epoll在内核中维护一棵文件描述符红黑树，避免了将用户态的fd集合拷贝到内核态**)
  - ~~<font color=Red>返回的是数量，不知道具体是哪个文件描述符就绪，需要遍历全部文件描述符（O(n)）</font>~~(**epoll维护一个就绪队列，不用遍历所有fd集合**)
  - ~~<font color=Red>入参的3个 fd_set 集合每次调用都需要重置</font>~~(poll中解决了这个问题)
  - 跨平台性不够好，只支持 linux，macOS 等操作系统不支持
  - 相较于 epoll，select 更轻量可移植性更强
  - 在监听连接数和事件较少的场景下，select 可能更优


### 异步IO
目前linux下异步IO还不是很完善，主要使用epoll。
**异步 I/O** 是**内核数据准备好**和**数据从内核态拷贝到用户态**这两个过程都不用等待。当我们发起 aio_read 之后，就立即返回，内核自动将数据从内核空间拷贝到应用程序空间，这个拷贝过程同样是异步的，内核自动完成的，和前面的同步操作不一样，应用程序并不需要主动发起拷贝动作。

### 信号驱动
当内核数据没有就绪好，用户程序不需要阻塞等待，也不需要轮询。当内核数据准备好，通过信号机制通知用户进程，这个过程是**异步的**。然后内核把数据从内核态拷贝到用户态，用户进程主动读取用户程序缓冲区的数据，这个过程是**同步的**。**信号驱动不阻塞在数据准备过程，但阻塞在数据拷贝**。

IO多路复用和信号驱动区别：
- IO多路复用：阻塞某个函数(Select)，遍历文件描述符集合，内核数据准备不需要阻塞，但是处理某个IO操作(内核将数据从内核态拷贝到用户态)需要阻塞。
- 信号驱动：内核数据准备是异步的，内核数据拷贝到用户态是同步的(阻塞)

**参考：**
- [IO多路复用之select、poll、epoll详解](https://www.cnblogs.com/jeakeven/p/5435916.html)
- [一文看懂IO多路复用](https://zhuanlan.zhihu.com/p/115220699)
- [IO多路复用之poll总结](https://www.cnblogs.com/Anker/p/3261006.html)
- [select、poll、epoll之间的区别](https://www.cnblogs.com/aspirant/p/9166944.html)
- [epoll源码分析(基于linux-5.1.4)](https://icoty.github.io/2019/06/03/epoll-source/)



## 两种高效的事件处理模型



## 多线程编程与线程池实现

