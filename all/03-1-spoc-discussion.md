# lec5 SPOC思考题


NOTICE
- 有"w3l1"标记的题是助教要提交到学堂在线上的。
- 有"w3l1"和"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的git repo上。
- 有"hard"标记的题有一定难度，鼓励实现。
- 有"easy"标记的题很容易实现，鼓励实现。
- 有"midd"标记的题是一般水平，鼓励实现。


## 个人思考题
---

请简要分析最优匹配，最差匹配，最先匹配，buddy systemm分配算法的优势和劣势，并尝试提出一种更有效的连续内存分配算法 (w3l1)
```
  + 采分点：说明四种算法的优点和缺点
  - 答案没有涉及如下3点；（0分）
  - 正确描述了二种分配算法的优势和劣势（1分）
  - 正确描述了四种分配算法的优势和劣势（2分）
  - 除上述两点外，进一步描述了一种更有效的分配算法（3分）
 ```
- [x]  

```
优势：
    最优匹配：大部分分配较小时，效果很好，可避免大的空闲区被拆分，可减少外部碎片大小，相对简单
	最差匹配：中等大小分配较多时，效果最好
	最先匹配：简单;在高地址有较大的空闲分区
	buddy system：速度快，低外部碎片

劣势:
    最优匹配：外部碎片;释放分区较慢;容易产生很多无用小碎片
    最差匹配：释放分区较慢; 外部碎片; 容易破坏大的空闲分区，因此后续难以分配大的分区
	最先匹配：外部碎片; 分配大块时较慢
	buddy system：内部碎片

对最优匹配或最差匹配，再增加一个链表，按地址顺序存储每个分区，则在释放分区时速度加快。
```

## 小组思考题

请参考ucore lab2代码，采用`struct pmm_manager` 根据你的`学号 mod 4`的结果值，选择四种（0:最优匹配，1:最差匹配，2:最先匹配，3:buddy system）分配算法中的一种或多种，在应用程序层面(可以 用python,ruby,C++，C，LISP等高语言)来实现，给出你的设思路，并给出测试用例。 (spoc)

```python
  class Test:
    def __init__(self,size):
        self.space = [size]
        self.use_list = [0]
        self.use_dict = {}

    def malloc(self,name,size):
        flag = 0
        for i in range(len(self.space)):
            if self.space[i] >= size and self.use_list[i] == 0:
                if self.space[i] < 2 * size:
                    self.use_list[i] = 1
                    self.use_dict[name] = i
                    flag = 1
                    break
                else:
                    self.space.insert(i,self.space[i] / 2)
                    self.space[i + 1] = self.space[i]
                    self.use_list.insert(i,0)
                    self.malloc(name,size)
                    if name in self.use_dict:
                        return
        if flag == 0:
            print "malloc false"
        return self.space

    def demalloc(self,name):
        index = ""
        try:
            index = self.use_dict[name]
        except:
            return "no name"
        self.use_list[index] = 0
        if index == 0:
            if self.use_list[1] == 0:
                self.space[1] += self.space[0]
                del(self.space[0])
                del(self.use_list[0])
        else:
            if self.use_list[index - 1] == 0 and self.space[index - 1] == self.space[index]:
                self.space[index] += self.space[index - 1]
                del(self.space[index - 1])
                del(self.use_list[index - 1])
            if self.use_list[index + 1] == 0 and self.space[index + 1] == self.space[index]:
                self.space[index + 1] += self.space[index - 1]
                del(self.space[index])
                del(self.use_list[index])
        i = 0
        while i < len(self.space) - 1:
            if self.use_list[i] == 0:
                if self.use_list[i + 1] == 0 and self.space[i + 1] == self.space[i]:
                    self.space[i + 1] += self.space[i]
                    del(self.space[i])
                    del(self.use_list[i])
                    i -= 1
            i += 1
        return self.space
```
## 扩展思考题

阅读[slab分配算法](http://en.wikipedia.org/wiki/Slab_allocation)，尝试在应用程序中实现slab分配算法，给出设计方案和测试用例。

## “连续内存分配”与视频相关的课堂练习

### 5.1 计算机体系结构和内存层次
MMU的工作机理？

- [x]  

>  http://en.wikipedia.org/wiki/Memory_management_unit

L1和L2高速缓存有什么区别？

- [x]  

>  http://superuser.com/questions/196143/where-exactly-l1-l2-and-l3-caches-located-in-computer
>  Where exactly L1, L2 and L3 Caches located in computer?

>  http://en.wikipedia.org/wiki/CPU_cache
>  CPU cache

### 5.2 地址空间和地址生成
编译、链接和加载的过程了解？

- [x]  

>  

动态链接如何使用？

- [x]  

>  


### 5.3 连续内存分配
什么是内碎片、外碎片？

- [x]  

>  

为什么最先匹配会越用越慢？

- [x]  

>  

为什么最差匹配会的外碎片少？

- [x]  

>  

在几种算法中分区释放后的合并处理如何做？

- [x]  

>  

### 5.4 碎片整理
一个处于等待状态的进程被对换到外存（对换等待状态）后，等待事件出现了。操作系统需要如何响应？

- [x]  

>  

### 5.5 伙伴系统
伙伴系统的空闲块如何组织？

- [x]  

>  

伙伴系统的内存分配流程？

- [x]  

>  

伙伴系统的内存回收流程？

- [x]  

>  

struct list_entry是如何把数据元素组织成链表的？

- [x]  

>  
