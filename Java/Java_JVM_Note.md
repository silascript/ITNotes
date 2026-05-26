---
aliases: []
tags:
  - PL
  - java
  - jvm
created: 2026-05-27 02:16:10
modified: 2026-05-27 02:45:08
---

# JVM 笔记

---

## 字节码

---

## 类加载过程

---

## GC

---

## 内存布局

### 堆

 #JVM/Heap

Heap 也称「堆」，它存储几乎所有的实例对象，它由 [GC](#GC)（垃圾回收器）自动回收，堆区由各子线程共享使用。

#### 新生代

 #JVM/Heap/Young

#### 老年代

 #JVM/Heap/Old

#### 永久代

 #JVM/Heap/Perm

永久代，也称为 「**Perm 区**」。

在 [JDK8](Java_Note.md#JDK8) 中，此块已经被淘汰。

### 元空间

 #JVM/Metaspace

Metaspace，元空间，是从 [JDK8](Java_Note.md#JDK8) 之前的版本，的 [堆](#堆) 中的 `Perm` 区，即 [永久代](#永久代)，演化而来。即从 JDK8 后，永久代从堆中「独立」出来。

Metaspace 与「永久代」区别是，「永久代」是堆中的一块，Java 这个堆是 JVM 的堆；而 Metasapce 的空间是本地（Native）内存中分配的，与堆区「平级」。

### 虚拟机栈

 #JVM/Stack

JVM Stack，即「虚拟机栈」

---

## 相关笔记

* [Java 笔记](Java_Note.md)

