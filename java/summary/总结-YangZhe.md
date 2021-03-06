## 创建一个类的时候，什么时候选择类、内部类、静态内部类、匿名内部类？设计框架时该如何选择。

开始我谈一下对内部类的理解，我认为内部类的出现，万物皆对象的思路，人作为一个对象，脑袋是一个成员变量，但是脑袋不足以相容他的时候，这时候我开始把脑袋也看成一个对象，内部类便是形容这种场景下的情况。

1. 什么时候选择类、内部类、静态内部类、匿名内部类？

   选择类这个是面向对象的思想，内部类就是一个成员变量不足以形容的时候选择内部类，如果有个优先级加载的时候我们可以选择静态内部类，还有为了外部写法上方便我们便采用匿名内部类。

2. 设计框架时该如何选择？

   同上。。。

## 什么时候会创建一个抽象类，为什么不用接口？设计框架时该如何选择？

通过阅读抽象类和接口的文章，刚开始觉得老生常谈，但是细读下去发现这是一篇用心的文章，读起来有种诙谐不是风范的调调。读后有所得，这里我mark一下。

正经回答问题：

1. 什么时候创建抽象类，什么时候用接口？

   抽象类确实需要**成员变量**或者需要**向上转型**的时候才采用抽象类，否则选用接口。因为接口可以**多继承**功能，而且抽象类一旦继承就会有很多不需要的方法。

2. 设计框架的时候如何选择？

   设计框架的时候让我想到了，为了提高扩张性，更具不同的情况要灵活设计框架。

   - 实现接口是优先考虑的，因为扩展性高，而且代码规范

   比如我们做一个数据存储功能的时候，我们可以做shareprefrence、文件存储、数据库存储，我们可以设计一个saveData的一个接口SaveListener，然后三者实现这个接口，然后在调用的时候参数传这个接口，便有可扩展性，这样可以方便切换，其实也就是策略模式的一种实现了。

   - 考虑过接口的后，发现还有成员变量的需求的时候，这时候选择抽象类

     例如我们封装的MVP 设计模式中的时候，Presenter 就需要传到BaseActivity 中，方便调用 Presenter 中的方法，这时候便需要将BaseAcitivity 写为抽象类了。


   - 抽象类和接口不是互斥的，两者可以同时实现，这便实现了多继承而且也可以定义成员变量。

   > tips  不是接口不能写成员变量，变量如果有的话都是 public static final 形式的

   ​

## 常见的类、成员变量修饰符

1. 常见的类：

    集合类：HashMap、ArrayList、HashSet  

   线程类： Thread 、Runnable

2. 成员变量修饰符

   public

   private

   default

   protected

   static

   final

   volatate 

## 自动装箱拆箱

看完麦田兄写的自动拆装箱，发现条理清晰，讲到了点上，读后有所得

1. jdk 1.5 之后自动拆装箱，注意在频繁操作拆装箱的时候，需要注意性能问题
2. 注意集合中因为泛型要求必须是Object 类型，所以设计到拆装箱，这里可以好好看看如何处理的
3. 拆装箱设计到的引用类型注意空指针问题，基本类型都有默认值，如果不注意会报错的

## 注解

注解我接触的不多，但是想到的大都是@Overite @Ignore...等注解，但是还有自定义注解，因为在Retrofit 中就是用到了注解，拿到参数然后进行参数的拼装，就行网络请求的。

## 反射

一般用于过期的方法，例如4.4 版本的的短信拦截功能等，需要反射拿到废弃的方法，来实现功能

## 泛型

泛型用于集合中，或者方法代替某类型的值，里边只能是Object类型不能为基本类型，设计到的知识有泛型擦除、自动拆装箱等知识

## 异常

我感觉异常遇到的面试问题，各位挺好了，我要装逼了。。。

1. 你平时在项目中最常见的异常有哪些？

   我平时异常数据参数类型不对异常，由于服务器同学写的代码经常类型没约定好，然后导致的错误。（这锅甩的可以不？但是这显得咱们不是老司机，我一般都是把接口都用postman 写一个单元测试队列，然后使用接口前都会进行进行一次校验，血和泪的教训，多少次研究半天发现是服务器的返回参数问题才总结出来的）

2. 你项目中遇到你认为最难的一个问题是什么？

   这个还真遇到过，最好是解决问题越底层显得你越有深度，所以这个问题，我感觉应该以下方面解答：

   （1）用的某个框架，发现现有功能有问题，我追踪源码，然后找到问题所在，顺便提交一个pr

   （2）容我想想。。。

## 集合框架

集合中如果面试的话，这是一个难学的地方，因为如果要解释很清楚需要数据结构的知识扎实，这里我以问答模式来总结我对集合框架的总结



1. ArrayList 和数组的区别？（线性表）

   数组的长度是固定的，Ar'rayList 的长度是可变化的，因为ArrayList都有扩容的机制，每次添加都会检查长度是否大于等于上一次约定的默认值，如果大于等于变会扩容。

   ​

2. fast-fail 和 fast-safe 机制

   fast-fail 问题对于多线程处理 Iteriter 遍历数组的时候 ，修改的modCount和另一个线程expectCount 不相等会抛出的错误。

   ```java
    final void checkForComodification() {
         if (expectedModCount != ArrayList.this.modCount)
             throw new ConcurrentModificationException();
   }
   ```

   fast-safe 机制：

   fail-safe任何对集合结构的修改都会在一个复制的集合上进行修改，因此不会抛出ConcurrentModificationException

   fail-safe机制有两个问题

   （1）需要复制集合，产生大量的无效对象，开销大

   （2）无法保证读取的数据是目前原始数据结构中的数据

   ​

3. PropertyQueue 的数据结构和应用场景（队列）

   数据结构特点： 只能从前后边插入

   数据结构： 完全二叉树、最小树

   应用场景： 比如医院看病一样，分普通病人、急症病人、马上需要动手术的，而且是动态插入数据，不是稳定的一个数组，这时候插入后需要排序分出优先级，然后处理。

   ​

4. Stack 的使用和使用场景（堆 FIFO）

   数据结构特点： 先进后出（FIFO）

   应用场景： 对于Acitivity的管理，可以将每一个启动的Acitivity技术堆中，然后符合我们页面特点，先进入的页面后出来。

   ​

5. LinkedList (双向链表)

   链表：插入、删除 迅速，查询难    

   数组： 查询简单 迅速 ，插入、删除难

6. HashMap 的实现原理  （散列表 俗称哈希表）

   数据结构： 散列表 

7. HashMap 和 HashTable 、CorrentHashTable 的区别和使用场景（多线程、并发）

   HashTable是线程安全的，HashMap是线程不安全的

   CorrentHashTable 处理并发情况比 HashTable 更优秀

##IO

IO 的操作有不少，大都是对于字节流读取和字符流读取两种方式，注意捕获异常，或者用其中的BufferWriter做缓冲等操作的类，大概就这么多。。。

## 字符串

字符串这个知识点，我总结的如下:

1. equals 和 == 的区别（这个很基础）

    一个比较的是value值，一个比较对象的hash值

2. String 是final 类型，为什么设计为 final类型？

   final的出现就是为了为了不想改变，而不想改变的理由有两点：设计(安全)或者效率。

3. StringBuffer 和StringBuilder的区别？

   StringBuffer 是线程安全的，在没有多编程的考虑下，推荐使用StringBuilder 效率更高

## 枚举

考到这个点的时候，我想到会被问到的问题：

1.  单例模式最简单的实现方式
2.  定义一些事物的状态码，比如后台的对应状态码和返回信息的规范约定中用到

## 工具类 Collections、Arrays

这个涉及到与数据结构相关的算法了，比如 Arrays.sort() 遍用的快速排序O(logn)的方法 、Collections的乱序等方法，就是封装了一系列对集合进行操作的静态函数，方便直接调用。这里想到的就这么多，在多就没用过了。

