---
aliases: []
tags:
  - dotnet
  - wpf
created: 2024-05-02 11:20:09
modified: 2024-05-03 22:01:23
---

# WPF 笔记

---

## XAML

---

## 布局

### 布局容器

 #Panel

WPF 布局容器均派生自 `System.Windows.Controls.Panel` 抽象类的面板。也就是说所有的容器都是一个 `Panel`。

#### Grid

#### StackPanel

顾名思义，`StackPanel` 中的子元素都以「堆栈」形式放置。

`StackPanel` 可以垂直放置，也可以水平放置，默认是自上而下地垂直排列。通过方向属性设置：`Orientation="Horizontal"`，面板容器中的元素就可以水平排列放置了。

##### 对齐

* `VerticalAlignment`：垂直对齐
* `HorizontalAlignment`：水平对齐





#### WrapPanel

#### DockPanel

---

## 事件

控件的事件都是在控件中使用其 `Click` 属性来指定。`<Button Click="xxx_Click">`

## 路由事件

---

## 相关笔记

* [.Net视频清单](Dotnet_Videos.md)