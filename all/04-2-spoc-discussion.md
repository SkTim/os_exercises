#lec9 虚存置换算法spoc练习

## 个人思考题
1. 置换算法的功能？

2. 全局和局部置换算法的不同？

3. 最优算法、先进先出算法和LRU算法的思路？

4. 时钟置换算法的思路？

5. LFU算法的思路？

6. 什么是Belady现象？

7. 几种局部置换算法的相关性：什么地方是相似的？什么地方是不同的？为什么有这种相似或不同？

8. 什么是工作集？

9. 什么是常驻集？

10. 工作集算法的思路？

11. 缺页率算法的思路？

12. 什么是虚拟内存管理的抖动现象？

13. 操作系统负载控制的最佳状态是什么状态？

## 小组思考题目

----
(1)（spoc）请证明为何LRU算法不会出现belady现象
```
访问页的序列为b(t)，物理页帧的集合为S(t)，更大的物理页帧的集合为S'(t)
讨论以下四种情况：
1. b(t)属于S(t)，b(t)属于S'(t)
2. b(t)不属于S(t)，b(t)属于S'(t)
3. b(t)不属于S(t)，b(t)不属于S'(t)
4. b(t)属于S(t)，b(t)不属于S'(t)
前三种情况下，在物理页帧为S'(t)时，缺页率不会高于S(t)
若第四种情况发生，则：
b(t)在S(t)中，则知b(t)最近被使用过且在S(t)中并没有被换出，然而其在S'(t)中被换出，与LRU算法矛盾
```


(2)（spoc）根据你的`学号 mod 4`的结果值，确定选择四种替换算法（0：LRU置换算法，1:改进的clock 页置换算法，2：工作集页置换算法，3：缺页率置换算法）中的一种来设计一个应用程序（可基于python, ruby, C, C++，LISP等）模拟实现，并给出测试。请参考如python代码或独自实现。
 - [页置换算法实现的参考实例](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab3/page-replacement-policy.py)
```
def test(timeSize,memSize,readList,length):
    memory = [0] * memSize
    in_memory = {}
    page_fault = 0
    point = 1
    recent_used = {}
    for i in range(len(readList)):
        recent_used[readList[i]] = 1
        in_memory[readList[i]] = 1
        if readList[i] not in in_memory:
            if point - page_fault > timeSize:
                for key in in_memory.items():
                    if key not in recent_used:
                        del(in_memory[key])
            recent_used = {readList[i] : 1}
    point += 1
    return in_memory
```

## 扩展思考题
（1）了解LIRS页置换算法的设计思路，尝试用高级语言实现其基本思路。此算法是江松博士（导师：张晓东博士）设计完成的，非常不错！

参考信息：

 - [LIRS conf paper](http://www.ece.eng.wayne.edu/~sjiang/pubs/papers/jiang02_LIRS.pdf)
 - [LIRS journal paper](http://www.ece.eng.wayne.edu/~sjiang/pubs/papers/jiang05_LIRS.pdf)
 - [LIRS-replacement ppt1](http://dragonstar.ict.ac.cn/course_09/XD_Zhang/(6)-LIRS-replacement.pdf)
 - [LIRS-replacement ppt2](http://www.ece.eng.wayne.edu/~sjiang/Projects/LIRS/sig02.ppt)
