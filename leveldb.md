# LevelDB Notes
<h2 align="left">目录</h2>
[toc]



## LSM Tree

存储是数据库的重要模块，存储引擎是存储的核心。存储引擎是建立在**存储结构**上的。存储结构的共同**特性**：
1. **存储结构要适合磁盘存储**：顺序IO性能是要远远优于随机IO的。所以存储结构的IO次数越少越好，并且应该尽量一次读取一块连续的区域。从这个角度上看，存储结构的单元越大越好。
2. **存储结构要允许并发操作**: 存储结构需要支持增删改的高并发，增删改对存储结构的影响尽量下。从这个角度上看，存储结构的单元应该越小越好。

只适合磁盘存储，但不适合并发操作的数据结构有：**大文件、数组**。它们都是连续的一大块存储区域，但如果要修改，影响面很大，基本要锁住整个数据结构。适合并发操作，但不适合磁盘存储的数据结构有：**链表、各种二叉树、跳跃表、红黑树、哈希表**这些。它们的修改、写入操作影响小，但数据结构的粒度也非常小，一次一般只操作几个字节，不适合磁盘IO。

同时具备这两个特性的是：**B树类** 和 **LSM树**。B树类 和 LSM树分别代表两种不同的**存储结构**：
- **In-place update**：**就地更新结构**，因为只会存储每个记录的最新版本，所以往往**读性能更优，但写入的代价会变大**，因为更新会导致随机IO。
- **Out-of-place update**：**异位更新结构**，因为这种设计是顺序写，所以**写入性能显然更高，但读性能**显然就被牺牲掉了。可能要扫描多个位置，才能读到我们想要的结果。此外，这种数据结构还需要一个数据Compaction过程，防止数据无限膨胀。
<div align=center>
   <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/leveldb_lsm_storage_structure.jpg" width = "80%" height = "80%">
</div>

<u>B+树，就地更新结构，按页存储，支持连续IO, 并且IO次数(读/写)少，比较符合特性1；B+树的SMO(Structure Modification Operation)，造成页分裂或合并，这种影响会向上传递，可能造成根节点的分裂或合并，此外还需要层层加锁实现其并发控制，这些使得其多核并发操作性能较差。LSM树异位更新结构，正常插入数据完全不会改变历史数据的结构，也不需要线程锁实现并发控制，并发能力强，在特性2上表现好，此外追加写，写性能好(类比GFS)；LSM 每层数据多，读数据可能要读很多次，特性1表现不好，需要靠 Compaction 操作来优化。</u>

**总结**：B树和LSM树分别是in-place update和out-of-place update模式的集大成之作，而分布式(追加写好)、固态硬盘（读性能提升）、多核CPU(并发要求)三种技术的发展使存储结构从in-place update向out-of-place update方向发展，LSM树也因此得到了越来越多的使用，而以Bw树为代表的新型B树融合了两种模式的特性，可能是未来的一个发展方向


<div align=center>
   <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/leveldb-lsm.png" width = "75%" height = "75%">
</div>

下面详细介绍 LSM(The log-structured merge-tree, LSM-tree)树。发展历程：1996 --> bigtable --> levelDB --> RocksDB(facebook)

### LSM Tree 结构

LSM Tree 的结构如下：
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/leveldb-lsm_structure.png)

**写入流程：**
1. 写入的数据首先要记录WAL，用来做实时落盘，以实现持久性及崩溃后能够从WAL恢复
2. 然后数据有序的写入Active Memtable中，Active Memtable也是这里唯一可变的结构。在一个Active Memtable写满后，就把它转换为Immutable Memtable。**两类Memtable都在内存中，使用的数据结构基本上是跳跃表**
   - <font color=Red>这里为什么使用跳表数据结构，而不是常用其他内存数据结构，如 AVL 树和红黑树？跳表通过空间换时间，将查找的时间复杂度优化到 O(logn)(和AVL, 红黑树一样)，每层索引中插入元素的时间复杂度 O(1)，所以整个插入的时间复杂度是 O(logn)(和AVL, 红黑树一样)。但跳表天然有序，在 flush 的时候效率很高，且插入代价小，因此对于LSM Tree 结构在内存中使用跳表</font>
   - **适合内存的数据结构：AVL 树(旋转多)，红黑树(改进AVL 树，旋转少，操作粒度小，epoll使用)，跳表(添加方便，能够保证元素有序，方便落盘，LSM Tree 内存中和 Redis 使用之)**
   - **适合磁盘存储的数据结构，B+ 树，B树，LSM树(追加写)**
3. 在Immutable Memtable达到指定的数量后，就把Immutable Memtable落盘到磁盘中的L0层，我们把这步操作称为**minor merge**。
    - 为了追求性能，落盘操作的memtable不做整理，**内存到L0层是追加写**, L0层SST之间可能有**重叠**的范围数据。
    - $L_{i}$ 到 $L_{i+1}$(i>=1)**写入时排序**，可能要读取所有SST, **L1层之后的SSTable之间无范围重叠且有序**。 
4. L1和之后层次间的合并，随着 Compaction 操作一层层向下执行。**Compact，就是将低级别的多个SST文件中的数据通过不同策略放入高一级的SST文件中，并清理掉低级别中的SST文件的过程**。后面详细介绍Compact。

**读取流程：**
读取的过程就是从上至下层层扫描(内存->硬盘)，直至找到数据。在查找的过程中使用**布隆过滤器**加速对数据的筛选。下面详细介绍布隆过滤器。

### SSTable

**SSTable**(Sorted String Table): 有序字符串表，是LevelDB最初实现的一种数据格式。可以把SST理解成一个小型的聚簇索引结构，只是这个结构整体是不可变的。一个SST通常由两部分组成：索引文件和数据文件。索引文件可以是B树或哈希表。数据文件就是要存储的KV数据，KV对以多个block的形式组织读取, SSTable 查询过程：
1. 通过<u>范围</u>确定是否可能在本SSTable
2. 搜索SSTable读取键值block
3. 从block中读取KV

### 布隆过滤器
布隆过滤器 (Bloom Filter)是由 Burton Howard Bloom 于 1970 年提出，**它是一种 space efficient 的概率型数据结构，用于判断一个元素是否在集合中**。
当布隆过滤器说，**某个数据存在时，这个数据可能不存在；当布隆过滤器说，某个数据不存在时，那么这个数据一定不存在。**
哈希表也能用于判断元素是否在集合中，但是布隆过滤器只需要哈希表的 1/8 或 1/4 的空间复杂度就能完成同样的问题。
**布隆过滤器可以插入元素，但不可以删除已有元素。**（删除意味着需要将对应的 k 个 bits 位置设置为 0，其中有可能是其他元素对应的位。）
其中的元素越多，false positive rate(误报率)越大，但是 false negative (漏报)是不可能的。

**布隆过滤器原理**
BloomFilter 的算法是，首先分配一块内存空间做 bit 数组，数组的 bit 位初始值全部设为 0。加入元素时，采用 k 个相互独立的 Hash 函数计算，然后将元素 Hash 映射的 K 个位置全部设置为 1。检测 key 是否存在，仍然用这 k 个 Hash 函数计算出 k 个位置，如果位置全部为 1，则表明 key 存在，否则不存在。
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/leveldb_bloom_filters.png)
**哈希函数会出现碰撞，所以布隆过滤器会存在误判**。哈希函数越多，误报率越低，但哈希函数太多，布隆过滤器代价也多，内存也多。哈希函数使用多少个，需要好好协调，是一个重要参数。

### LSM Tree 放大问题

- **读放大**（Read Amplification）。LSM-Tree 的读操作需要从新到旧（从上到下）一层一层查找，直到找到想要的数据。这个过程可能需要不止一次 I/O，扫描多个SSTable。特别是 range query 的情况，影响很明显。Compact操作也会造成读放大。
- **空间放大**（Space Amplification）。因为所有的写入都是顺序写（append-only）的，不是 in-place update ，所以过期数据不会马上被清理掉。例如 SSTable 中存储的旧版数据都是无效的。
- **写放大**（Write amplification）原本指的是在SSD上的一种介质本身的现象，即实际写入介质的数据量大于请求本身的数据量。例如在 LSM 树中写入时可能触发Compact操作，导致实际写入的数据量远大于该key的数据量。

层数越多, 读放大增加，写放大增加，空间放大增加。高性能写和高性能读是矛盾的：越追求写，写放大越小，读放大越大，越追求读，读放大越小，写放大越大。



**LSM-Tree的写放大问题由来**:

1. <u>追加写入，而不是覆盖原有数据，产生重复写</u>
2. <u>Compaction操作(主要)，涉及到将数据从一个位置复制到另一个位置，并且涉及到磁盘的读写操作</u>

这些额外的写入操作增加了实际写入的数据量，从而导致写放大问题的产生。

**放大问题的优化**:
1. **批量写入**（Batching Writes）：将多个写操作合并为一个批量写入操作。
2. **写缓冲区冲区的优化**：通过优化写缓的大小和管理策略，可以减少写放大问题。
3. **压缩策略的优化**：通过调整压缩策略，可以减少压缩的频率和成本，从而减少写放大问题。
4. **硬件优化**：使用高性能的存储设备，如固态硬盘（SSD），可以提高写入性能并减少写放大问题。

### LSM Tree Compact及优化

Compaction 合并策略有两种，如下：
- **Leveling Merge Policy**：把相联两层的数据做合并，然后一起写入下面那层。**LevelDB一系列LSM树使用该策略**。
- **Tiering Merge Policy**：合并时也并不是把相联两层合并，而是把一层的所有组件进行合并并放入下一层。这导致每层都有重叠的区域。

<div align=center>
   <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/leveldb_lsm_merge.jpg" width = "75%" height = "75%">
</div>

> 相比于Leveling合并策略，Tiering合并策略显而易见的对写入更友好，但读取的性能会进一步降低，因为每一层也有多个重叠的区域

然后**写入时排序**。

Compaction一直是LSM树的瓶颈所在，Compaction过程中占用大量资源并调整数据位置，会引发缓冲池中数据的大量丢失。compaction 优化方案如下：
1. 使用**Tiering合并策略**是提升综合写性能，减少写放大的一个重要手段。
2. 还有另外一些手段来优化Compaction。可以采用流水线技术，把读取、合并、写入三个操作以流水线的形式执行，以增强合并操作的资源利用率，减少耗时；
3. 可以**复用组件**，在合并的过程中识别出不变的部分并保留；
4. Compaction操作会造成Cache丢失，为了优化这一点，可以在**Compaction结束后主动更新Cache**，或采用机器学习的方式预测回填；
5. 还可以**利用额外硬件执行Compaction**，把Compaction操作offload到FPGA等额外的硬件上执行，使Compaction不占用本地的CPU和内存，减少Compaction对读写操作的影响。


**参考：**
- https://mp.weixin.qq.com/s/NW3lGMejsDHNs0ml3vQRwg
- https://mp.weixin.qq.com/s/NW3lGMejsDHNs0ml3vQRwg
- [Redis 布隆（Bloom Filter）过滤器原理与实战](https://cloud.tencent.com/developer/beta/article/1975700)


## LevelDB

LevelDB 的选择牺牲部分 Get 性能，换取强悍 Put 性能，再极力优化 Get(LSM 树)。

LevelDB 存储结构如下：
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/leveldb_structure2.webp)
内存中：最多 4MB，然后写入级别 0 的 SST 文件。其他见图，和 LSM Tree 类似。

实现 LevelDB 的软件架构如下： 
![](https://images--hosting.oss-cn-chengdu.aliyuncs.com/ds/leveldb_structure.png)

> levelDB 小端存储

**参考：**
https://juejin.cn/post/6901257330524946445

https://izualzhy.cn/leveldb-sstable

https://bean-li.github.io/leveldb-sstable/

https://www.cnblogs.com/Jack47/p/sstable-1.html