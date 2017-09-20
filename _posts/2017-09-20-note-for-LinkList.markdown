---
layout:		post
title:		"链表的基本用法"
subtitle:		"java语言"
date:		2017-09-20 12:00:00
author:		"Droodsunny"	
header-img: ""
---

>可以说学了两年的编程语言了，但是对指针和链表的操作依然很糊涂，好在Java中没有了指针，最近看到有一位大神学姐说如果面试的时候连链表的倒置与合并都不会就太尴尬了，所以在这里，第一次用Java完成链表的操作，可能认识很短浅，温故而知新。

>首先新建一个链表的结点类,包含两个字段
```java
public class Node {
	public int data;
	public Node next;
}
```
>然后新建一个单链表的类,包含四个字段
```java
    public Node head, tail; // 都声明为公开成员
	public int size = 0; // 这是一个派生数据，冗余数据，
```

# 在单链表类中封装操作
## 判断节点数量
```java
public int getSize() { // 判断元素的数量，如果没有size数据的话，
		return size; // 需要遍历这个链表，然后计数
	}
```
## 清空链表
```java
public void clear() { // 清空单链表，回到单链表的初始状态
		head = tail = null;
		size = 0;
	}
```
## 头插法
```java
public void addhead(int data){
		Node n= new Node();
		n.data=data;
		if (size == 0) { // 如果链表为空，没有一个元素
			head = n; // head，tail都指向这个元素就可以了
			tail = n;
		} else {
		n.next=head;
		head=n;
		}
		size++;
	}

```
## 尾插法
```java

public void add(int data) { // 向链表末尾追加元素，append操作
		Node n = new Node();
		n.data = data;
		if (size == 0) { // 如果链表为空，没有一个元素
			head = n; // head，tail都指向这个元素就可以了
			tail = n;
		} else { // 如果链表不为空
			/*
			 * n.next = head; head = n; //头插法，在最前面插入一个元素
			 */
			tail.next = n; // 尾插法，在链表末尾添加新节点
			tail = n;
		}
		size++; // 链表元素数量+1
	}
```
## 在指定位置插入元素
```java
public void add(int index, int data) { // 在指定位置插入元素，index从0开始
		/*
		 * 如果位置index非法，应该抛出IndexOutOfBoundsException异常， 异常处理将在后续章节介绍
		 */
		if (index > size || index < 0) {
			return;
		}
		Node n1 = new Node();
		n1.data = data;
		if (size == 0) { // 链表为空的条件下，
			head = n1;
			tail = n1;
		} else { // 如果链表不为空
			Node n = head; // 当前节点
			Node p = head; // 当前节点的前一个节点
			for (int i = 0; i < index; i++) { // 开始遍历，直到到达插入位置
				p = n;
				n = n.next;
			}
			if (n == p) { // 插入位置就是0，头插法。
				n1.next = head;
				head = n1;
			} else if (n == null) { // 插入位置是末尾，尾插法。
				tail.next = n1;
				tail = n1;
			} else { // 中间位置插入一个元素
				p.next = n1;
				n1.next = n;
			}
		}
		size++; // 将元素个数+1
	}
```
## 删除指定位结点
```java

public void remove(int index) { // 删除指定位置的元素
		// 注意，不是删除某个数
		if (index >= size || index < 0) {// 非法的位置参数，应该抛出异常，
			return; // 在此处简单的返回处理，什么也不做
		}
		if (size == 1) { // 单链表中只有一个元素，删除
			head = tail = null;
		} else if (index == 0) { // 不止有一个元素，如果删除第一个元素
			head = head.next; // 进行第一个元素的删除操作
		} else {
			Node n = head; // 如果不是删除第一个元素，则遍历
			Node p = head; // 到该位置
			for (int i = 0; i < index; i++) {
				p = n;
				n = n.next;
			}
			p.next = n.next; // 进行该指定位置的删除操作
			if (p.next == null) { // 如果删除的是末尾节点，那么p.next为空
				tail = p; // 还需要设置tail引用
			}
		}
		size--; // 元素的个数-1
	}
```
## 返回指定下标位置的数据
```java
public int get(int index) {
		Node n = head;
		for(int i=0;i<index;i++) {
			n = n.next;
		}
		return n.data;
	}
```
## 遍历单链表
```java
public void printElements() { // 输出链表中所有数据，遍历单链表
		if (size == 0) {
			System.out.println("empty single linkedlist!");
		} else {
			Node n = head;
			System.out.print("elements: ");
			while (n != null) { // 循环遍历程序
				if (n == tail) { // println函数输出后自动添加回车换行
					System.out.println(n.data); // 最后的元素输出后，回车换行
				} else { // print函数输出后不自动添加回车换行
					System.out.print(n.data + ", ");
				}
				n = n.next; // 单链表的遍历，后续章节的异常链也是单链表
			}
		}
	}
```
## 输出头结点元素信息
```java
public void printFirst() { // 输出头节点元素信息
		if (head != null) {
			System.out.println("first = " + head.data);
		} else {
			System.out.println("no first element.");
		}
	}
```
## 反转链表
``` java
public void Reverse(Node n){
		Node next = null;
		Node prev=null;
		while(n!=null){
			next=n.next;
			n.next=prev;
			prev=n;
			n=next;
		}
		head=prev;
	}
```
