---
sticker: emoji//2615
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


