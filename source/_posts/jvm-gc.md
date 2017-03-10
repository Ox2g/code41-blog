title: GC概述
date: 2017-03-09 09:37:23
tags:
- java
- JVM
- 基础
- GC
---
### GC概述
#### 1. 什么是GC
- Garbage Collection
- GC就是对垃圾进行回收
- GC是为了使开发人员不需要显式分配内存和回收内存，从而减少开发的难度
- 但是，GC的实际效果却是...
- ![](http://om6u6x9f9.bkt.clouddn.com/markdown/1488970912818.gif)

#### 2. 哪些地方可以回收？哪些东西要回收？
- JVM基础-内存空间
    + JVM规范下的结构示意图
        * ![](http://om6u6x9f9.bkt.clouddn.com/markdown/1488944811319.png)
    + JVM内存示意图[JDK8持久代已经由元空间替换]
        * ![](http://om6u6x9f9.bkt.clouddn.com/markdown/1488946190664.png)
- 回收哪些地方？
    + 方法区[回收条件苛刻很难回收，既回收的效果不佳]
        * 对类型卸载
        * 常量池回收
            - 废弃的常量[常量对象未被引用]
            - 无用的类
                + 该类的所有实例已被回收，不存在实例
                + 加载该类的ClassLoader已经被回收
                + 该类对应的Class对象已经回收，无法通过反射引用
    + Java堆[主要的GC处理区域，怎么回收中详细介绍]
        * 可回收70%~95%的空间
- 哪些东西要回收？[如何定义垃圾]
    + 定义算法
        * [反例]引用计数算法
            - 被引用一次则计数+1
        * 根搜索算法[GC Roots Tracing]
            - 通过一系列GCRoots的对象为起始点，通过引用的向下搜索，可以与GCRoots的对象建立联系的都标记为可用对象，反之为不可用对象。
            - GC Roots的对象
                + 虚拟机栈中的引用对象
                + 方法区中类静态属性引用的对象
                + 方法区中常量引用对象
                + 本地方法栈中的引用对象
        * 示例图
            - ![](http://om6u6x9f9.bkt.clouddn.com/markdown/1488958141212.png)
    + 定义引用[Reference]
        * Strong Reference > Soft Reference > Weak Reference > Phantom Reference

#### 3. 怎么回收？
- 通过收集器回收
- 收集器基于回收算法产生
    + 收集算法
        * Mark-Sweep
            - 内存中标记需要回收的内存空间，在标记完成后统一回收掉全部标记的内存空间，从而使得对象回收完成。
            - 缺点
                + 效率问题：标记和清除的操作过程，效率都比较低。
                + 空间问题：清除后会产生大量不连续的内存碎片，导致大对象无可用的连续内存。
            - ![](http://om6u6x9f9.bkt.clouddn.com/markdown/1488958759088.png)
        * Copying
            - 可用内存划分为大小相等的两部分，当内存用完后，将存活对象所占用的内存空间复制到另外一半中，然后回收掉用完的一半。
            - 优点
                + 只需要移动堆顶的指针就可以完成内存的切换，运行高效
                + 无内存碎片问题
            - 缺点
                + 内存只使用一半，浪费较大
            - ![](http://om6u6x9f9.bkt.clouddn.com/markdown/1488958705168.png)
        * Mark-Compact
            - 针对老年代的特点，让所有存活的对象向一端移动，清理边界以外的全部内存空间。
            - ![](http://om6u6x9f9.bkt.clouddn.com/markdown/1488958734183.png)
        * Generational Collection[分代收集]
            - 根据存活周期不同将内存划分多块，然后针对不同不同周期（年代）对象使用不同的，最应当的垃圾回收算法。
            - java堆中划分为新生代和老年代，针对新生代(每次垃圾回收，有大批对象死亡，)使用Copying算法，而针对老年代使用Mark-Compact算法。
- 收集器的分类和细节
    + 评判标准[有言在先]
        * 吞吐量优先
        * 响应时间优先
    + Serial收集器
        * 新生代收集使用
        * 复制算法
        * 单线程，适合于单核
        * ![](http://om6u6x9f9.bkt.clouddn.com/markdown/1488967461931.png)
    + ParNew收集器
        * 新生代收集使用
        * 复制算法
        * Server模式首选收集器
        * 新生代回收采用多线程处理
        * ![](http://om6u6x9f9.bkt.clouddn.com/markdown/1488967494306.png)
    + Parallel Scavenge收集器
        * 新生代收集使用
        * 复制算法
        * 新生代回收采用多线程处理
        * 可控制吞吐量--与ParNew收集器的唯一区别[吞吐量=运行用户代码时间/(运行用户代码时间 + GC回收停顿时间)]
        * 无法与CMS收集器搭配使用
    + Serial Old收集器
        * 老生代收集使用
        * 标记整理算法
        * 单线程
        * 作为CMS收集器出错后的备选方案
        * ![](http://om6u6x9f9.bkt.clouddn.com/markdown/1488967461931.png)
    + Parallel Old收集器
        * 老生代收集使用
        * 标记整理算法
        * 多线程整理
        * 搭配Parallel Scavenge收集器的老生代收集使用
        * ![](http://om6u6x9f9.bkt.clouddn.com/markdown/1488967670110.png)
    + CMS(Concurrent Mark Sweep)收集器
        * 老生代收集使用
        * 与用户线程共同并发标记和并发清理
        * 标记清除算法
        * 优点
            - 并发收集
            - 低停顿
        * 缺点
            - 产生碎片
            - 无法处理浮动垃圾(并发标记过程中用户产生的垃圾)
            - 占用用户线程的CPU资源
        * ![](http://om6u6x9f9.bkt.clouddn.com/markdown/1488969530314.png)
    + G1[Garbage First]
        * 基本要求
            - 多核CPU
            - 大内存
        * 基于Mark-Compact算法[不会产生碎片]
        * 垃圾收集的消耗时间在毫秒级别
        * 划分多个大小固定的区域(Region)
            - 大小范围为1M~32M
            - Region的类型
                + Eden
                + Survivor
                + Old
            - 年代类型
                + 新生代
                + 老生代
            - GC
                + YoungGC
                    * 新对象进入Eden区 -> 存活对象拷贝到Survivor区 -> 存活时间达到年龄阈值时，对象晋升到Old区
                + FullGC
                    * 初始并行阶段 -> Root区扫描 -> 并行标记 -> 重标记 -> 清除 -> 复制
                    * 初始并行阶段（Initial Marking Phase） 
                        - 属于Young GC范畴，是stop-the-world活动。对持有老年代对象引用的Survivor区（Root区）进行标记。
                    * Root区扫描（Root Region Scanning） 
                        - 扫描Survivor区中的老年代对象引用，该阶段发生在应用运行时，必须在Young GC前完成。
                    * 并行标记（Concurrent Marking） 
                        - 找出整个堆中存活的对象，对于空区标记为“X”。该阶段发生在应用运行时，同时该阶段活动会被Young GC打断。
                    * 重标记（Remark） 
                        - 清除空区，重计算所有区的存活状态（liveness），是stop-the-world活动。
                    * 清除（Cleanup）
                        - 选择出存活状态低的区进行收集。
                        - 计算存活对象和空区，是stop-the-world活动。
                        - 更新记录表，是stop-the-world活动。
                        - 重置空区，将其加入空闲列表，是并行活动。
                    * 复制（Copying）
                        - 该阶段是stop-the-world活动，负责将存活对象复制到新的未使用的区。
                        - 可以发生在年轻代区，日志记录为[GC pause (young)]。
                        - 也可以同时发生在年轻代区和老年代区，日志记录为[GC Pause (mixed)]。
    + 不同收集器的搭配
        * -XX:+UseSerialGC|新生代使用Serial，老生代使用Serial Old
        * -XX:+UseParNewGC|新生代使用ParNew，老生代使用Serial Old
        * -XX:+UseConcMarkSweepGC|新生代使用ParNew，老生代使用CMS，老生代后备使用Serial Old
        * -XX:+UseParallelGC|新生代使用Parallel Scavenge，老生代使用Serial Old
        * -XX:+UseParallelOldGC|新生代使用Parallel Scavenge，老生代使用Parallel Old
        * -XX:+UseG1GC|用G1

#### 4. 内存分配与回收策略
- GC触发条件&实例验证
    + 基础
        * -xx:+PrintGCdetails 可以详细了解GC中的变化。
        * -XX:+PrintGCTimeStamps 可以了解这些垃圾收集发生的时间，自JVM启动以后以秒计量。
        * -XX:+PrintGCApplicationStoppedTime 输出GC造成应用暂停的时间
        * -XX:+PrintGCDateStamps GC发生的时间信息
        * -xx:+PrintHeapAtGC 了解堆的更详细的信息。
        * -Xloggc:/tmp/logs/gc.log gc日志产生的路径
    + 分析[内部在线分析工具（包含常用命令）](http://mprofiler.stable.meili-inc.com/)
    + GC触发条件
        * YoungGC/MinorGC
            - Eden空间不足以分配内存给新的对象
        * FullGC
            - 老生代空间不足，新生代对象或者大对象无法转入老生代
            - Perm Gen被占满
                + 采用CMS
                + 加大Perm Gen
            - CMS GC出现promotion failed和concurrent mode failure
                + 增大survivor空间
                + 增大老生代空间
                + 调低触发GC的比率
            - 统计到的YoungGC晋升的平均大小大于老生代剩余空间大小
* 汇总
    + 对象优先在Eden区分配
    + 大对象直接进入老老年代
        * 大对象直接进入老年代的目的是避免Eden和Survivor的互相拷贝大对象
    + 长期存活对象将进入老年代
        * 避免短存活的大对象
            - 大对象一般需要大的连续空间，如果没有会触发GC操作，因此避免短存活期的大对象存在
    + 动态对象年龄判定
    + 空间分配担保
- 启发
    + 避免短存活的大对象
        * 大对象一般需要大的连续空间，如果没有会触发GC操作，因此避免短存活期的大对象存在

#### 5. 扩展
- Real-Time JDK
    + 新的内存管理机制
        *  Immortal内存区域
            - Immortal内存区域用于保留永久的对象，这些对象仅在应用结束运行时才会释放内存，这个最典型的需求场景莫过于缓存了 
        *  Scoped内存区域
            - Scoped内存区域用于保留临时的对象，位于scope中的对象在scope退出时，这些对象所占用的内存会被直接回收
        * Immortal内存区域和Scoped内存区域均不受GC管理，因此基于这两个内存区域来编写的应用完全不用担心GC会造成暂停的现象
- JVMs[参考1](https://www.zhihu.com/question/29265430?sort=created)[参考2](https://en.wikipedia.org/wiki/Comparison_of_Java_virtual_machines)
    + HotSpot
    + IBM J9
    + Zing JVM

#### 6. 总结
- 这些回收技术都是越来越好用，但这些并不能成为大家不优化现有代码的原因，我们需要为将来留有余地

#### 7. 流程图文件
[流程图](http://om6u6x9f9.bkt.clouddn.com/fileGC4JVM.key)
