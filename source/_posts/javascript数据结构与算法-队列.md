---
title: javascript数据结构与算法-队列
date: 2018-11-16 21:09:18
tags: 
  - javascript
  - 前端
  - 数据结构
categories:
 - 前端
 - 数据结构
---
# 定义
队列是一种特殊的线性表，特殊之处在于它只允许在表的前端（front）进行删除操作，而在表的后端（rear）进行插入操作，和栈一样，队列是一种操作受限制的线性表。进行插入操作的端称为队尾，进行删除操作的端称为队头。队列中没有元素时，称为空队列。
队列的数据元素又称为队列元素。在队列中插入一个队列元素称为入队，从队列中删除一个队列元素称为出队。因为队列只允许在一端插入，在另一端删除，所以只有最早进入队列的元素才能最先从队列中删除，故队列又称为先进先出（FIFO—first in first out）线性表。
# 方法和属性
- push方法 入队
- shift方法  删除队头的元素
- peek方法 查看当前队头的元素
- isEmpty方法 查看队列是否为空
- length属性 队列的元素个数
- list属性  存储队列  
# 代码实现
这里依然用js的数组模拟队列，毕竟高级语言（落泪），数组的shift时删除数组的第一个元素,这里list还是公用的
```javascript
class Queue{
	constructor(){
	    this.list=[];
	    this.length=0;
	}

	/* 入队 */
	enquequ(value){
	    this.length++;
	    this.list.push(value);
	}

	/* 出队 */
	dequeue() {
	    if (this.length === 0) return;
	    this.length--;
	    return this.list.shift();
	}
	/* 查看队头 */
	peek() {
	    return this.list[0];
  }
	/* 查看队头 */
	isEmpty(){
	    return this.length ===0;
	}
}
```
# 优先队列
简单来说，比方说我是会员，我就要排在你的前面，hhhhhh

实现一个优先队列，有两种选项：设置优先级，然后在正确位置添加元素；或者用入列操作添加元素，然后按照优先级移除他们。
```javascript
function PriorityQueue(){
	let items = [];
	// 辅助类
	function QueueElement(element,priority){
	    this.element = element;
	    this.priority = priority;
	}
	this.enquequ=function(element,priority){
	    let queueElement = new QueueElement(element,priority);
	    let added =false;
	    // 比较优先级
	    for (let i = 0; i < items.length; i++) {
	    // 如果说我的优先级比较大
	        if(queueElement.priority>items[i].priority){
	            // 数组切割，实现添加操作
	            items.splice(i,0,queueElement);
	            added=true;
	            break;
	        }
	    }
	    // 如果没有插入就是说我的优先级都比他们小，就放到队尾
	    if(!added){
	        items.push(queueElement);
	    }
	};
	// 打印
	this.print=function(){
	    for (let i = 0; i < items.length; i++) {
	        console.log(`${items[i].element}- ${items[i].priority}`);
	    }
	}
}
let priorityQueue = new PriorityQueue();
priorityQueue.enquequ("skl",2);
priorityQueue.enquequ("skl2",1);
priorityQueue.enquequ("skl3",3);
priorityQueue.print();
```
如果队列为空，就直接插入，如果不为空，就进行优先级比较操作。中间使用了一个辅助类，用来创建名字和优先级的对象
```javascript
{
  element:'skl',
  priority:'1',
}
```
# 实现击鼓传花
不懂得网上百度一下啥事击鼓传花
```javascript
function hotPotato(nameList,num){
	let queue = new Queue();
	// 先入队
	for (let i = 0; i < nameList.length; i++) {
	    queue.enquequ(nameList[i]);
	}
	// 被淘汰者
	let eliminated="";
	while(queue.length>1){
	    for (let i = 0; i < num.length; i++) {
	        // 出队后入队，就是从队头到队尾去
	        queue.enquequ(queue.dequeue());
	    }
	    // 等于num的那个被淘汰
	    eliminated=queue.dequeue();
	    console.log(eliminated+"在击鼓传花中淘汰");
	}
	// 循环结束后只有一个人的时候他就是获胜者了
	return queue.dequeue();
}
let names = ["John","Jack","Camilia","Ingrid","Carl"];
let winner = hotPotato(names,2);
console.log("获胜者是"+winner);
```
输出过程
```
John在击鼓传花中淘汰
Jack在击鼓传花中淘汰
Camilia在击鼓传花中淘汰
Ingrid在击鼓传花中淘汰
获胜者是Carl
```
下图模拟了这个过程
![](/image/javascript数据结构与算法之队列/javascript数据结构与算法之队列-1.png)

