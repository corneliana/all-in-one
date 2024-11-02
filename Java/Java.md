---
sticker: emoji//2615
_links: []
---
Question:
1. 闭包和对象共享的区别？
	1. 闭包提供了一种强大的方式来创建有状态的函数，而对象共享则是理解 Java 内存模型和并发编程的基础。
2. `Function` 是 Java 8 引入的一个函数式接口，它是 `java.util.function` 包中的一部分。这个接口代表了一个接受一个参数并产生一个结果的函数。将函数作为对象来传递和操作，是函数式编程的一个核心概念，使得代码更加灵活和可组合。
3. 
4. Interface：定义对象行为的方式，不关心对象的具体实现


Data Type
- 基本类型（Primitive Types）
	- 包括 byte, short, int, long, float, double, boolean, 和 char
	- 直接存储在栈内存(Stack Memory)
	- 存储的是实际的数据值

- 引用类型（Reference Types）
	- 包括所有的对象类型，如 String, 数组, 自定义类等
	- 存储在堆内存(Heap Memory)，而变量本身（引用）存储在栈内存中
	- 变量存储的是对象在堆内存中的地址


https://liaoxuefeng.com/books/java/oop/basic/method/index.html#0
- Case 1: fullName[0]被重新赋值之后，数组的第一个元素从指向Homer，改为指向Bart。但p.name对整个数组fullName的指向没有改变。相当于p.name在更高一层，这个指向没有变，只是它里面元素的指向变了，所以p.name也随之变为"Bart Simpson"。
- Case 2: bob被重新赋值之后，bob从指向Bob，改为指向Alice。但p.name在第一次setName时已经独立指向了Bob，所以bob的指向更改和p.name没有关系。

所以，
- 不可变对象（如String）的不可变性：值永不会变，只有指向会变；
- 可变对象（如String数组）的可变性：在更高层次上保持指向不变，通过改变其内部不可变对象的指向，体现出了整体的可变性。