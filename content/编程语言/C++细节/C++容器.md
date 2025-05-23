# C++容器
## 顺序容器🥣

在C++中，顺序容器（Sequential Containers）是用于管理元素线性排列的数据结构，其核心特点是通过位置（存储顺序）访问元素

#### 顺序容器的选择——该选哪个
| **容器**         | **内存结构** | **随机访问** | **插入/删除效率**       | **迭代器失效风险**  | **额外空间开销** |             |
| -------------- | -------- | -------- | ----------------- | ------------ | ---------- | ----------- |
| `vector`       | 单块连续内存   | O(1)     | 尾部O(1)，中间/头部O(n)  | 扩容后全部失效      | 预留容量       | [[deque容器]] |
| `deque`        | 分块连续内存   | O(1)     | 头尾O(1)，中间O(n)     | 插入中间可能失效     | 块管理开销      |             |
| `list`         | 双向链表     | 不支持      | 任意位置O(1)          | 永不失效（除非元素删除） | 前后指针       |             |
| `forward_list` | 单向链表     | 不支持      | 插入后继O(1)，删除后继O(1) | 同list        | 前向指针       |             |
| `array`        | 固定连续内存   | O(1)     | 不支持               | 永不失效         | 无          |             |

一般来说，每个容器都定义在单独的一个头文件中，文件名与类型名相同

#### 操作——该怎么用
| 类型别名            | 说明                                  |
| --------------- | ----------------------------------- |
| iterator        | 此容器类型的迭代器类型                         |
| const_iterator  | 可以读取元素，但不能修改元素的迭代器类型                |
| size_type       | 无符号整数类型，足够保存此种容器类型最大可能容器的大小         |
| difference_type | 带符号整数类型，足够保存两个迭代器之间的距离              |
| value_type      | 元素类型                                |
| reference       | 元素的左值类型；与 value_type& 含义相同          |
| const_reference | 元素的 const 左值类型（即，const value_type&） |

| 构造函数          | 说明                                                                 |
|-------------------|----------------------------------------------------------------------|
| C;                | 默认构造函数，构造空容器（array，参见第301页）                       |
| C c1(c2);         | 构造 c2 的拷贝 c1                                                    |
| C c(b, e);        | 构造 c，将迭代器 b 和 e 指定的范围内的元素拷贝到 c（array 不支持）   |
| C{a, b, c...};    | 列表初始化 c                                                         |

| 赋值与 swap       | 说明                                                                 |
|-------------------|----------------------------------------------------------------------|
| c1 = c2            | 将 c1 中的元素替换为 c2 中元素                                       |
| c1 = {a, b, c...}  | 将 c1 中的元素替换为列表中元素（不适用于 array）                     |
| a.swap(b)         | 交换 a 和 b 的元素                                                   |
| swap(a, b)        | 与 a.swap(b) 等价                                                    |

| 大小              | 说明                                                                 |
|-------------------|----------------------------------------------------------------------|
| c.size()          | c 中元素的数目（不支持 forward_list）                                 |
| c.max_size()      | c 可保存的最大元素数目                                               |
| c.empty()         | 若 c 中存储了元素，返回 false，否则返回 true                          |

| 添加/删除元素    | 注：在不同容器中，这些操作的接口都不同                              |
|-------------------|----------------------------------------------------------------------|
| c.insert(args)    | 将 args 中的元素拷贝进 c                                             |
| c.emplace(inits)  | 使用 inits 构造 c 中的一个元素                                       |
| c.erase(args)     | 删除 args 指定的元素                                                 |
| c.clear()         | 删除 c 中的所有元素，返回 void                                       |

| 关系运算符        | 说明                                                                 |
|-------------------|----------------------------------------------------------------------|
| ==, !=, <, <=, >, >= | 所有容器都支持相等（不等）运算符                                     |
|                    | 关系运算符（无序关联容器不支持）                                     |

| 获取迭代器                | 说明                      |
| -------------------- | ----------------------- |
| c.begin(), c.end()   | 返回指向 c 的首元素和尾元素之后位置的迭代器 |
| c.cbegin(), c.cend() | 返回 const_iterator       |
|                      |                         |

| 反向容器的额外成员（不支持 forward_list） |                           |
| --------------------------- | ------------------------- |
| reverse_iterator            | 按逆序寻址元素的迭代器               |
| const_reverse_iterator      | 不能修改元素的逆序迭代器              |
| c.rbegin(), c.rend()        | 返回指向 c 的尾元素和首元素之前位置的迭代器   |
| c.crbegin(), c.crend()      | 返回 const_reverse_iterator |

#### 迭代器——🤯
##### 使用迭代器

有的类的迭代器定义了比较操作，但并非所有的类都如此，所以判断迭代器是否到达了末尾最好是判断是否相等
```cpp

classname::iterator it;//普通迭代器

classname::const_iterator it;//常量迭代器

s.begin() s.end()

s.cbegin() s.cend()
```

| it++            |              |
| --------------- | ------------ |
| (*it).something | it→something |
| *it             |              |

**任何改变容器容量的操作都会使容器失效**

