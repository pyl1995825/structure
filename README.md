#数据结构

<br/>
##链表
<br/>
###单向链表

通过js实现链表的功能，包括链表的删除和增加，但是这其中有一定的问题
所以这里对这些问题进行一些补充：

js实现链表的<strong>初始化</strong>时，使用的是头插法<br/>

	p->next = (*L)->next;
	(*L)->next = p;

用的是插队的方法，这种方法始终让新节点在第一的位置，事实上，这并不符合正常的思维，所谓先来后到。我们把每次新节点都插在终端节点后面，这种方法被称为尾插法<br/>

	for(var i = 0; i < n; i++) {
		(*L)->next = p;
		r = p;
	}
	r->next = null;



还有在链表进行<strong>删除</strong>的时候，需要创建一个变量来保存被释放的值，以便后续可以进行二次使用<br/>

	var node, element;
	node = preNode.next;
	preNode.next = node.next;
	element = node.next;
	return node;


链表的整体删除：
	
	var q;
	p = this.head;
	while(p) {
		q = p.next;
		remove(p);
		p = q;
	}
	(*L)->next = null;
	
###双向链表

双向链表是在单链表中的每个节点中，在设置一个指向其前驱节点的指针域，所以一个节点中有两个指针域，一个指向直接后继，一个指向直接前驱

	typeof struct DulNode {					function  node(element) {
		ElementType data;		   js			this.element = element;
		struct DulNode *prior;   ------>  		this.next = null;
		struct DulNode *next;					this.previous = null;
	}DulNode, *DuLinkist;					}


删除和插入的操作<br/>
1.插入操作  s <br/>
	
	s.next = p.next;
	s.previous = p;
	p.next.previous = s;
	p.next = s;

2.删除操作 p<br/>

	p.previous.next = p.next;
	p.next.previous = p.previous;


###循环链表
循环链表是将单链表中终端节点指针端由空指针改为指向头结点，就使整个单链表形成一个环
