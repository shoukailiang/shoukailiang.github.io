---
title: javascript数据结构与算法-链表
date: 2018-11-20 12:39:36
tags: 
  - javascript
  - typescript
  - 前端
  - 数据结构
categories:
  - 数据结构
---

# 定义
链表是一种物理存储单元上非连续、非顺序的存储结构，数据元素的逻辑顺序是通过链表中的指针链接次序实现的。链表由一系列结点（链表中每一个元素称为结点）组成，结点可以在运行时动态生成。每个结点包括两个部分：一个是存储数据元素的数据域，另一个是存储下一个结点地址的指针域。 相比于线性表顺序结构，操作复杂。由于不必须按顺序存储，链表在插入的时候可以达到O(1)的复杂度，比另一种线性表顺序表快得多，但是查找一个节点或者访问特定编号的节点则需要O(n)的时间，而线性表和顺序表相应的时间复杂度分别是O(logn)和O(1)。
# 方法和属性
- count属性 数量
- head属性  指向head对象
- push(element)
- getElementAt(index) 根据索引获取,返回node对象
- insert(element, index) 向链表中插入对象
- removeAt(index) 根据索引移除
- remove(element)
- indexOf(element) 作用类似于数组的indexOf 返回索引
- isEmpty() 判断是否为空
- size() 长度
- getHead() 获得链表头
- clear()  清空链表
- toString() 由于链表中使用了Node 类，就需要重写继承自javascript对象默认的toString方法


# 代码实现链表
先创建一个Node 类，即为辅助类，构建出这样的一个对象,他包含一个element属性，即要添加到列表的值，以及一个next属性，即指向下一个节点的指针
```javascript
class Node {
  constructor(element, next) {
    this.element = element;
    this.next = next;
  }
}
```
辅助方法,用来判断两个值是否相等
```javascript
function defaultEquals(a, b) {
  return a === b;
}
```
LinkedList类,链表的关键就是他不像是数组一样，可以直接拿到当前索引的值，他必须从head开始找，不停的找next。
#### push
push的时候，判断是否是第一个，或是，将头变成当前插入的node,若不是，循环找到最后一项，将他的next执行当前插入的node，循环的时候，若找到有个node的next为空，即把这个node的next指向当前这个node
#### getElementAt
根据索引返回node对象,注意循环的终止条件
#### insert 
插入，也要判断是否是第一个
#### removeAt
若是第一个，把头改成下一个
```javascript
class LinkedList {
	constructor(equalsFn = defaultEquals) {
		this.equalsFn = equalsFn;
		this.count = 0;
		this.head = undefined;
	}
	push(element) {
		const node = new Node(element);
		let current;
		if (this.head == null) {
			this.head = node;
		} else {
			current = this.head;
			while (current.next != null) {
				current = current.next;
			}
			current.next = node;
		}
		this.count++;
	}
	getElementAt(index) {
		if (index >= 0 && index <= this.count) {
			let node = this.head;
			for (let i = 0; i < index && node != null; i++) {
				node = node.next;
			}
			return node;
		}
		return undefined;
	}
	insert(element, index) {
		if (index >= 0 && index <= this.count) {
			const node = new Node(element);
			if (index === 0) {
				const current = this.head;
				node.next = current;
				this.head = node;
			} else {
				const previous = this.getElementAt(index - 1);
				node.next = previous.next;
				previous.next = node;
			}
			this.count++;
			return true;
		}
		return false;
	}
	removeAt(index) {
		if (index >= 0 && index < this.count) {
			let current = this.head;
			if (index === 0) {
				this.head = current.next;
			} else {
				const previous = this.getElementAt(index - 1);
				current = previous.next;
				previous.next = current.next;
			}
			this.count--;
			return current.element;
		}
		return undefined;
	}
	remove(element) {
		const index = this.indexOf(element);
		return this.removeAt(index);
	}
	indexOf(element) {
		let current = this.head;
		for (let i = 0; i < this.size() && current != null; i++) {
			if (this.equalsFn(element, current.element)) {
				return i;
			}
			current = current.next;
		}
		return -1;
	}
	isEmpty() {
		return this.size() === 0;
	}
	size() {
		return this.count;
	}
	getHead() {
		return this.head;
	}
	clear() {
		this.head = undefined;
		this.count = 0;
	}
	toString() {
		if (this.head == null) {
			return '';
		}
		let objString = `${this.head.element}`;
		let current = this.head.next;
		for (let i = 1; i < this.size() && current != null; i++) {
			objString = `${objString},${current.element}`;
			current = current.next;
		}
		return objString;
	}
}
```