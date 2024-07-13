---
aliases: []
tags:
  - java
  - base
created: 2023-08-18 19:44:52
modified: 2024-07-13 10:59:53
---

# `i++` 和 `++i`

---

## int 类型前后 ++

### int 无赋值 ++

#### 例子 1

```java
public class InCreament_1{
	public static void main(String[] args) {
		
		int i=0;
		int j=0;

		i++;
		++j;

		System.out.println("i: "+i);
		System.out.println("j: "+j);

	}
}
```

运行结果:

![image-20191102214024405](./i++和++i分析.assets/image-20191102214024405.png)

​	使用 javap 分析:

```
public class InCreament_1 {
  public InCreament_1();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return

  public static void main(java.lang.String[]);
    Code:
       0: iconst_0
       1: istore_1
       2: iconst_0
       3: istore_2
       4: iinc          1, 1
       7: iinc          2, 1
      10: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
      13: new           #3                  // class java/lang/StringBuilder
      16: dup
      17: invokespecial #4                  // Method java/lang/StringBuilder."<init>":()V
      20: ldc           #5                  // String i:
      22: invokevirtual #6                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      25: iload_1
      26: invokevirtual #7                  // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
      29: invokevirtual #8                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
      32: invokevirtual #9                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      35: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
      38: new           #3                  // class java/lang/StringBuilder
      41: dup
      42: invokespecial #4                  // Method java/lang/StringBuilder."<init>":()V
      45: ldc           #10                 // String j:
      47: invokevirtual #6                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      50: iload_2
      51: invokevirtual #7                  // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
      54: invokevirtual #8                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
      57: invokevirtual #9                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      60: return
```

> [!info] 分析结果
> 
> 从第**4**行和第**7**行的指令，可以看到无论是 `++` 在前还是在后，都是调了 `iinc`，此指令是 jvm 专门针对 int 类型「优化」的自增指令。
> 
> 此自增是直接对「局部变量表」 (有的地方也翻译为 「本地变量表」) 中数据自增，这就是所谓的 「**先自增后赋值**」 或 「**先赋值后自增**」中的自增，自增是局部变量表中的数据自增。
> 
> 第 **25** 行和第 **50** 行中 `iload_1` 和 `iload_2` 两个指令,是从局部变量表将数据加载进操作数栈中，这样才能让 `println()` 方法调用将数据显示出来,而因为上面 iinc 指令直接对局部变量表自增了，所以无论 `i++` 还是 `++i`，最后都是显示结果为**1**。

----------------------------------------------------------------------------------------------

### int 赋值 ++

#### 例子 2

```java
public class InCreament_2{
	public static void main(String[] args) {
		
		int i=0;
		int j=0;


		i=i++;
		j=++j;

		System.out.println("i: "+i);
		System.out.println("j: "+j);

	}
}
```

运行结果:

![image-20191102213934082](./i++和++i分析.assets/image-20191102213934082.png)

javap 分析：

```
public class InCreament_2 {
  public InCreament_2();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return

  public static void main(java.lang.String[]);
    Code:
       0: iconst_0
       1: istore_1
       2: iconst_0
       3: istore_2
       4: iload_1
       5: iinc          1, 1
       8: istore_1
       9: iinc          2, 1
      12: iload_2
      13: istore_2
      14: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
      17: new           #3                  // class java/lang/StringBuilder
      20: dup
      21: invokespecial #4                  // Method java/lang/StringBuilder."<init>":()V
      24: ldc           #5                  // String i:
      26: invokevirtual #6                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      29: iload_1
      30: invokevirtual #7                  // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
      33: invokevirtual #8                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
      36: invokevirtual #9                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      39: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
      42: new           #3                  // class java/lang/StringBuilder
      45: dup
      46: invokespecial #4                  // Method java/lang/StringBuilder."<init>":()V
      49: ldc           #10                 // String j:
      51: invokevirtual #6                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      54: iload_2
      55: invokevirtual #7                  // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
      58: invokevirtual #8                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
      61: invokevirtual #9                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      64: return
}
```

`i=i++` 对应的指令是：

```
4: iload_1
5: iinc          1, 1
8: istore_1
```

第**4**行指令：从**局部变量表**中 load 变量 i 的数据到**操作数栈**中，因为还没对**局部变量表** 中 i 的变量进行自增，所以 load 到**操作数栈** i 的值为 **0**。

第**5**行指令： 对**局部变量表** i 变量自增，这时局部变量表中 i 的值变为 1。

第 **8** 行指令：将**操作数栈** 中 i 变量出栈保存到**局部变量表**中 i 变量中。而这时操作数栈的 i 值是 0，即将 0 保存进局部变量表，所以之前自增后的 i 又被数据 「覆盖」变成 0。所以最后在第 **29** 行 `iload_1` 指令，将局部变量表中 i 值加载进栈顶，让 `println()` 方法调用显示，最后就如上面的结果一样，`i=i++` 的结果是 0。

`j=++j` 对应指令是：

```
9: iinc          2, 1
12: iload_2
13: istore_2
```

第 **9** 行 `iinc` 指令：先对局部变量表 j 的数据自增

第 **12** 行 `iload_2` 指令： 是将**局部变量表**的数据加载进**操作数栈**，这时局部变量表 j 的数据是 1，加载进栈顶，这时操作数栈 j 的数据是 1

第 **13** 行指令 `istore_2` 指令： 将**操作数栈**的数据保存进**局部变量表**，这时局部变量表 j 的数据仍为 1

所以最后在第 **54** 行的 `iload_2` 指令，从局部变量表将 j 数据加载进操作数栈栈顶，让 `println()` 方法调用时，显示 j 的结果是 1。

`i=i++` 与 `j=++j` 两条语句在指令分析上，最大区别，就是**iinc** 这个指令是先执行还是后执行。**++**在前，那就先对局部变量表进行自增，**++** 在后，就先从局部变量表加载数据到操作数栈后再对局部变量表中的变量进行自增。而因为局部变量表自增的时机不同，就出现 `i=i++` 与 `j=++j` 两条语句执行结果差异。而常见的说法：「先自增再赋值，先赋值后自增」，原理就在这里！

对比无赋值的 [例子 1](#例子%201)，本例子指令中多了 `istore` 指令，`store` 指令是将操作数栈数据出栈保存到**局部变量表**中，也就是在函数中说「赋值」实际是「赋」到「局部变量表」。

----------------------------------------------------------------

### 在别的方法中调用

#### 例子 3

```java
public class InCreament_1_1{
	public static void main(String[] args) {
			
		int i=0;
		int j=0;

		System.out.println("i: "+(i++));
		System.out.println("j: "+(++j));


	}
}
```

运行结果:

![image-20191102213443042](./i++和++i分析.assets/image-20191102213443042.png)

javap 分析:

```
public class InCreament_1_1 {
  public InCreament_1_1();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return

  public static void main(java.lang.String[]);
    Code:
       0: iconst_0
       1: istore_1
       2: iconst_0
       3: istore_2
       4: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
       7: new           #3                  // class java/lang/StringBuilder
      10: dup
      11: invokespecial #4                  // Method java/lang/StringBuilder."<init>":()V
      14: ldc           #5                  // String i:
      16: invokevirtual #6                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      19: iload_1
      20: iinc          1, 1
      23: invokevirtual #7                  // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
      26: invokevirtual #8                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
      29: invokevirtual #9                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      32: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
      35: new           #3                  // class java/lang/StringBuilder
      38: dup
      39: invokespecial #4                  // Method java/lang/StringBuilder."<init>":()V
      42: ldc           #10                 // String j:
      44: invokevirtual #6                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      47: iinc          2, 1
      50: iload_2
      51: invokevirtual #7                  // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
      54: invokevirtual #8                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
      57: invokevirtual #9                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      60: return
}
```

println() 方法中的 i++ 的对应的指令:

```
19: iload_1
20: iinc          1, 1
```

而另一个 println() 方法中对应的指令:

```
47: iinc          2, 1
50: iload_2
```

这时 " 先自增 " 或 " 后自增 " 的特性才显现出来。

i++，先是从局部变量表 load 数据到操作数栈，然后才将局部变量表中的 i 的数据自增为 1，而 println() 方法调的是操作数栈的数据，而这时的操作数栈 i 的数据为 0，所以结果显示 i 为 0。

++j，第**47**、**50**行指令可以看出先对局部变量表中 j 的数据自增为 1，然后才将局部变量表的数据 load 进操作数栈，最后 println() 方法调操作数栈数据，所以 j 的结果显示为 1。

这与例 2 的赋值语句情况是一样的。

-------

### 总结

上面三个例子，可以看出 " 先自增后赋值 " 或 " 先赋值后自增 " 的说法是具有迷惑和误导性的。++ 无论在前还是在后，单独存在是没有意义的，必须有赋值或与其他语句一起使用，" 先自增 " 或 " 后自增 " 的特性才会显现出来。

----

## long 类型的前后 ++

### long 无赋值 ++

#### 例子 4

```java
public class InCreament_3{
	public static void main(String[] args) {
		long i=0L;
		long j=0L;

		i++;

		++j;

		System.out.println("i: "+i);
		System.out.println("j: "+j);


	}
}
```

运行结果:

![image-20191103082103712](./i++和++i分析.assets/image-20191103082103712.png)

跟例子 1 结果完全一样。

javap 分析:

```
Compiled from "InCreament_3.java"
public class InCreament_3 {
  public InCreament_3();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return

  public static void main(java.lang.String[]);
    Code:
       0: lconst_0
       1: lstore_1
       2: lconst_0
       3: lstore_3
       4: lload_1
       5: lconst_1
       6: ladd
       7: lstore_1
       8: lload_3
       9: lconst_1
      10: ladd
      11: lstore_3
      12: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
      15: new           #3                  // class java/lang/StringBuilder
      18: dup
      19: invokespecial #4                  // Method java/lang/StringBuilder."<init>":()V
      22: ldc           #5                  // String i:
      24: invokevirtual #6                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      27: lload_1
      28: invokevirtual #7                  // Method java/lang/StringBuilder.append:(J)Ljava/lang/StringBuilder;
      31: invokevirtual #8                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
      34: invokevirtual #9                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      37: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
      40: new           #3                  // class java/lang/StringBuilder
      43: dup
      44: invokespecial #4                  // Method java/lang/StringBuilder."<init>":()V
      47: ldc           #10                 // String j:
      49: invokevirtual #6                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      52: lload_3
      53: invokevirtual #7                  // Method java/lang/StringBuilder.append:(J)Ljava/lang/StringBuilder;
      56: invokevirtual #8                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
      59: invokevirtual #9                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      62: return
}
```

其中 i++ 对应的指令如下:

```
4: lload_1
5: lconst_1
6: ladd
7: lstore_1
```

因为 long 类型没有编译器对 int 类型那样的 " 特殊照顾 "，所以只能老老实实 add。

大致步骤

1.lload_1-- 从局部变量表位置 1 中取出 i 的数据加载进操作数栈

2.将一个 long 类型的常量压入操作数栈栈顶

3.使用 ladd 指令将前两个元素出栈并相加，最后将相加的结果重新入栈

4.使用 lstore 指令将栈顶的相加结果保存入局部变量表 i 变量对应的位置

而 ++j 对应的是第**8**~第**11**行指令，与上面的 i++ 完全一样。

这就是 long 类型的 ++。

---

### long 赋值 ++

#### 例子 5

```java
public class InCreament_3_1{
	public static void main(String[] args) {
		long i=0L;
		long j=0L;

		i=i++;
		j=++j;

		System.out.println("i: "+i);
		System.out.println("j: "+j);

	}
}
```

运行结果:

![image-20191103084315564](./i++和++i分析.assets/image-20191103084315564.png)

与例子 2 结果完全一样

javap 分析:

```
Compiled from "InCreament_3_1.java"
public class InCreament_3_1 {
  public InCreament_3_1();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return

  public static void main(java.lang.String[]);
    Code:
       0: lconst_0
       1: lstore_1
       2: lconst_0
       3: lstore_3
       4: lload_1
       5: dup2
       6: lconst_1
       7: ladd
       8: lstore_1
       9: lstore_1
      10: lload_3
      11: lconst_1
      12: ladd
      13: dup2
      14: lstore_3
      15: lstore_3
      16: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
      19: new           #3                  // class java/lang/StringBuilder
      22: dup
      23: invokespecial #4                  // Method java/lang/StringBuilder."<init>":()V
      26: ldc           #5                  // String i:
      28: invokevirtual #6                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      31: lload_1
      32: invokevirtual #7                  // Method java/lang/StringBuilder.append:(J)Ljava/lang/StringBuilder;
      35: invokevirtual #8                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
      38: invokevirtual #9                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      41: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
      44: new           #3                  // class java/lang/StringBuilder
      47: dup
      48: invokespecial #4                  // Method java/lang/StringBuilder."<init>":()V
      51: ldc           #10                 // String j:
      53: invokevirtual #6                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      56: lload_3
      57: invokevirtual #7                  // Method java/lang/StringBuilder.append:(J)Ljava/lang/StringBuilder;
      60: invokevirtual #8                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
      63: invokevirtual #9                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      66: return
}
```

i=i++ 对应指令如下:

```
    4: lload_1
    5: dup2
    6: lconst_1
    7: ladd
    8: lstore_1
    9: lstore_1
```
步骤:
    1. 从局部变量表加载 i 数据进操作数栈
    2. 复制栈顶数据并入栈，栈顶数据为 0
    3. 常量 0 入栈，栈顶数据为 0
    4. add 栈顶二数并将结果压栈，栈顶数据为 1
    5. 将栈顶数据出栈保存进局部变量表中，栈顶数据为 0，局部变量表中 i 的值为 1
    6. 再将栈顶数据出栈保存进局部变量表，因此时栈顶数据为 0，所以此时局部变量表中 i 的值变为 0

j=++j; 对应指令如下:

```
 lload_3
      10: lload_3
      11: lconst_1
      12: ladd
      13: dup2
      14: lstore_3
      15: lstore_3
```

---

## 直接在 print 方法中调用

运行结果:

![image-20191104030012270](./i++和++i分析.assets/image-20191104030012270.png)

---

## 相关笔记

* [Java 笔记](Java_Note.md)
* [Java 基础笔记](Java_Base_Note.md)
* [Java IO 笔记](Java_IO_Note.md)