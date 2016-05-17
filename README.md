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


##栈

####栈的结构定义

	typedef int SElemType;					function Stack() {
	typedef struct {				js			this.dataStore = [];
		SElemType data[MAXSIZE];   ---->		this.top = 0;	
		int top;								this.pop = pop;
	}SqStack;									this.peek = peek;
											}


栈的顺序存储结构----进栈操作

	Status Push(SqStack *S, SElementType e) {
		if(S->top == MAXSIZE - 1 ) {
			return 0;
		}
		S-top++;
		S->data[S->top] = e;
		return OK;	
	}
	
	//js
	this.dataStore[this.top++].push(element);
出栈操作

	Status Pop(SqStack *S, SElementType *e) {
		if(S->top == -1) {
			return 0;
		}
		*e = S->data[S->top];
		S->top--;
		return OK;
	}



#####两栈共享空间<br/>

> 在使用两栈共享的时候需要多加两个指针变量top1,top2来区分栈1或者栈2<br/><br>
在插入以及删除过程中，还需要有一个判断是栈1还是栈2的栈号元素参数stackNumber

	//入栈
	Status Push(SqStack *S, SElemType e, int stackNumber) {
		if(S->top+1==S->top2) 
			return 0;
		if(stackNumber == 1) 
			S->data[++S->top1] = e; //如果是stack1那么就从顶部开始 ++
		else if(stackNumber == 2) 
			S->data[--S->top2] = e; //如果是stack2那么就从顶部开始 --
		return OK;
	}
	// 出栈
	Status Pop(SqStack *S, SElemType *e, int stackNumber) {
		if(stackNumber == 1) {
			if(S->top1 == -1) 
				return -1;
			*e = S->data[S->top1--]; //栈1元素出栈
		}
		else if(stackNumber == 2) {
			if(S->top2 == MAXSIZE) 
				return -1;
			*e = S->data[S->top2++]; // 栈2元素出栈
		}
	}

#####栈的链式存储结构
链栈的结构代码

	typedef struct StackNode {
		SElemType data;
		struct StackNode *next;
	}StackNode, *LinkStackPtr;

	typedef struct LinkStack {
		LinkStackPtr top; // 定义一个top栈点
		int countl
	}LinkStack;

栈的链式操作
> 插入操作：把未插入前的栈顶元素取出，赋值给新元素的直接后继，这样就将新元素和之前的栈顶元素建立连接，之后再移动栈顶指针位置到新元素上，完成压栈<br/><br/>
> 删除操作：记录栈顶元素，移动栈顶指针，释放记录的栈顶指针

	// 入栈
	Status Push(LinkStack *S, SElemType e) {
		LinkStackPtr s = (LinkStackPtr)malloc(sizeof(StackNode));
		s->data =  e;
		s->next = S->top; //让添加进来的元素与链栈相关联
		S->top = s;
		S->count++;
	}
	//出栈
	Status Pop(LinkStack *S, SElemType *e) {
		LinkStackPtr p;
		if(StackEmpty(*S))
			retun  -1;
		*e = S->top->next;
		p = S->top;
		S->top = S->top->next;
		free(p);
		S->count--;
		return OK;
	}

#####栈的作用
1. 递归
2. 四则运算表达式的求值 <br/>
> 后缀表达式：所有的符号都是在要运算的数字后面出现的 比如 9+（3-1）*3+10/2； 后缀表达式为9 3 1 - 3 * + 10 2 / + <br/>
> 中缀表达式：标准的四则运算表达式
	
	
##队列

###循环队列
> 队列存在两种指针，front指针指向对头元素，rear指针指向队尾元素的下一个位置，当front==rear时，此队列不是还剩一个元素，而是空队列<br/><br>
> 假溢出：假设有长度为5的数组，入队4个元素，出队2个，front指向2位置，rear指向4的位置，此时在入队一个元素，front指针不变，数组末尾元素已经占用，rear向后移一个单位，造成'假溢出'

如何解决假溢出？ ----> 循环队列

我们把队列中头尾相接的顺序存储结构成为循环队列，像刚才的例子，rear改为指向下标为0的位置，这样就不会造成指针指向不明的问题了，那么问题又来了？

1. 如何判断队列是否是空队列还是满队列
>办法一：设置一个标志变量flag，当front==rear,且flag=0时为空，当front==rear,且flag=1时队列满<br>
>方法二：当队列空是，条件时front==rear,当队列满时，修改其条件，保留一个元素空间，就是说队列满的时候，其实还有一个空闲单元 

讨论下第二种方法，由于rear和front大小不确定，所以尽管只差一个位置是满的情况，但也可能是相差一圈。所以队列满的条件时（rear+1）% QueueSize == front,队列长度公式为（rear-front+QueueSize）%QueueSize

	// 定义队列
	typedef int QElemType;
	typedef struct {
		QElemType data[MAXSIZE];
		int front;
		int rear;
	}SqQueue;
	
	// 入队
	Status EnQueue(SqQueue *Q, QElemType e) {
		if((Q->rear+1) % MAXSIZE == Q->front) 
			return -1;
		Q->data[Q->rear] = e; // 将元素e赋值到队尾
		Q->rear = (Q->rear+1)%MAXSIZE	// 移动rear指针
	}

	// 出队
	Status DeQueue(SqQueue *Q, QElemType *e) {
		if((Q->rear+1) % MAXSIZE == Q->front) 
			return -1;
		*e = Q->data[Q->front];
		Q->front = (Q->front + 1) % MAXSIZE;
	}

###链式队列
由于链式结构的特殊性，不用考虑队列是否满，因为链表没有固定大小，循环队列用的是线性表，使用数组必须要设置大小

	// 入队
	Status EnQueue(LinkQueue *Q, QElemType e) {
		QueuePtr s = (QueuePtr)malloc(sizeof(QNode));
		