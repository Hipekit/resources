# 1、什么是Steam流

`Stream`流是一种针对集合对象更加便利、高效的内聚操作或是大批量数据操作。Stream流分为中间操作和终止操作。中间操作返回的是一个新的Stream，终止操作返回的是我们需要的数据。

Stream流接口关系继承图：

![Stream流接口继承关系图](E:\学习资料\个人技术成长\Java8\Stream流\图片\Stream流接口继承关系图.png)

# 2、Stream流方法

1. `Collection.stream()`

2. `Stream.of()`
3. `foreach()`：循环调用
4. `filter()`：选取出匹配符合条件的元素
5. `map()`：将流中的数据映射到另一个流中（类型转换 s-> t）
6. `count`：返回流中元素个数（这是一个终结方法）
7. limit（n）：截取前n个元素
8. skip（n）：跳过前n个元素，如果n大于流中元素的个数，则会获得一个长度为0的流
9. concat(Stream a, Stream b)：合并两个流

注意：stream流属于管道流，只能使用一次，第一次调用完之后，数据会流转到下一个Stream上