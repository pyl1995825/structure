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

#####栈的结构定义

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

#####循环队列
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

#####链式队列
由于链式结构的特殊性，不用考虑队列是否满，因为链表没有固定大小，循环队列用的是线性表，使用数组必须要设置大小

	// 入队
	Status EnQueue(LinkQueue *Q, QElemType e) {
		QueuePtr s = (QueuePtr)malloc(sizeof(QNode));
		s->data = e;
		s->next = null;
		Q->rear->next = s;
		Q->rear = s;
	}
	// 出队
	Status DeQueue(LinkQueue *Q, QElemType *e) {
		QueuePtr p;
		if(Q->front == Q->rear) 
			return -1;
		p = Q->front->next;
		*e = p->data;
		Q->front->next = p->next;
		if(Q->rear == p) 
			Q->rear = Q->front;
		free(p);
	}


##[树](https://github.com/pyl1995825/structure/blob/master/Tree/READEME.md "树")

##查找
><strong>查找表</strong>是由同一类型的数据元素或记录构成的集合。<br>
><strong>关键字</strong>是数据元素中某个数据项的值<br>
>对于那些识别多个数据元素的关键字，称为<strong>此关键字</strong><br>
>查找就是根据给定的某个值，在查找表中确定一个其关键字等于给定值的数据元素

###顺序表查找算法
顺序表查找，是最基本的查找技术，它的查找过程是：从表中第一个记录开始，逐个进行记录的关键字和给定值比较，若某个记录的关键字和给定值相等，则查找成功，找到所查记录，若果直到最后一个记录，其关键字和给定值比较不相等，则表中没有所查记录，查找不成功

	// 顺序表查找
	int Sequential_Search(int *a, int n, int key) {
		int i;
		for(i = 1; i <= n; i++) {
			if(a[i] == key) 
				return i
		}
		return 0;
	}
	
	// 顺序表查找优化
	int Sequential_Search(int *a, int n, int key) {
		int i;
		a[0] = key;
		i = n;
		while(a[i] != key) {
			i--;
		}
		return i;
	}

###折半查找
在有序表中，取中间记录作为比较对象，若给定值与中间记录的关键字相等，则查找成功；若给定值小于中间记录，则在中间记录的左半区继续查找；若给定值大于中间记录的关键字，则在中间记录的右半区继续查找

	int Binary_Search(int *a, int n, int key) {
		int low, high,mid;
		low = 1;
		high = n;
		while(low <= high) {
			mid = (low+high)/2;  // 折半
			if(key < a[mid]) {   
				high = mid - 1;  // 最高下标调整到中位下标小一位
			} else if(key > a[mid]) {
				low = mid + 1;  //  最低下标调整到中位下标大一位
			} else {
				return mid;
			}		
		
		}
		return 0; 
	}

###插值查找
插值查找是根据要查找的关键字key与查找表中最大最小记录的关键字比较后的查找方法，其核心就在于插值计算公式(key-a[low])/(a[high]-a[low])

斐波那契查找法核心在于：
1. 当key=a[mid],查找就成功
2. 当key<a[mid],新范围是第low个到第mid-1个，此时范围个数为F[k-1]-1个
3. 当key>a[mid],新范围是第m+1个到第high个，此时范围个数是F[k-2]-1个


###线性索引查找
#####稠密索引
稠密索引是指在线性索引中，将数据集中的每个记录对应一个索引项<br>
对于稠密索引来说，索引项一定是按照关键字码有序的排列

#####分块索引
分块有序，是把数据集的记录分成了若干块，并且这些块需要满足两个条件：<br>
1. 块内无序，每一块内记录不要求有序<br>
2. 快间有序，比如第i块所有记录的关键字均要大于第i-1块所有记录的关键字

数据项：<br>
1. 最大关键码，它存储每一块中的最大关键字<br>
2. 存储了块中的记录个数，以便于循环时使用<br>
3. 用于指向块首数据元素的指针，便于开始对这一块中记录进行遍历

#####二叉排序树
- 若它的左子树不空，则左子树上所有的结点的值均小于它的根结构的值
- 若它的右子树不空，则右子树上所有的结点的值均大于它的根结点的值
- 它的左右子树也分别为二叉排序树

######二叉排序树的操作

	// 结构定义
	typedef struct {
		int data;
		struct BiTNode *lchild, *rchild;
	}BiTNode, *BiTree;

	// 递归查找二叉排序树是否存在key
	// f指向T的双亲，其初始值为null
	// 若查找成功，p指向该数据元素结点
	// 否则指针p指向查找路径上访问的最后一个节点并放回false
	Status SearchBST(BiTree T, int key, BiTree f, BiTree *p) {
			if(!T) {
				*p = f;
				return false;
			}else if(key == T->data) {
				*p = T;
				return true;
			} else if(key < T->data) {
				return SearchBST(T->lchild, key, T, p);
			} else {
				return SearchBST(T->rchild, key, T, p); 
			}
	}

	// 二叉排序树插入操作
	Status InsertBST(BiTree *T, int key) {
		BiTree p, s;
		if(!SearchBST(*T, key, null, &p)) {  // 找到合适的位置p 
			s = (BiTree)malloc(sizeof(BiTNode));
			s->data = key;
			s-lchild = s->rchild = null;
			if(!p) {
				*T = s;
			} else if(key < p->data) 
				p-lchild = s;
			else 
				p-rchild = s;
			return true;
		}
		else 
			return false;
	}

######二叉排序树删除操作
对删除结点情况的分析：
 
- 叶子结点
- 仅有左子树或者右子树的结点
- 左右子树都有的结点

	
		// 若二叉排序树中存在关键字等于key的数据元素时，删除该数据元素结点
		Status DeleteBST(BiTree *T, int key) {
			if(!*T) {
				return false;
			}
			else {
				if(key == (*T)->data)
					return Delete(T);
				else if(key < (*T)->data)
					return DeteleBST(&(*T)->lchild, key);
				else 
					return DeleteBST(&(*T)->rchild, key);
			}
		}
		
		// 二叉排序树中删除结点p，并重接它的左或右子树
		Status Delete(BiTree *p) {
			BiTree q,s;
			if((*p)->rchild == null) {
				q = *p; *p = (*p)->lchild; free(q);
			} else if((*p)->lchild == null) {
				q = *p; *p = (*p)->rchild; free(q);
			}
			else {
				q = *p ; 
				s = (*p)-lchild;
				while(s->rchild) {
					q = s;
					s = s->rchild;
				}
				(*p)->data = s->data;
				if(q != p) {
					q->rchild = s->lchild;
			
				}else {
					q->lchild = s->lchild;
				}
				free(s);
			
			}
			return true;
		}

###平衡二叉树
平衡二叉树：是一种二叉排序树，其中每个结点的左子树和右子树的高度差至多等于1；我们将二叉树上结点的左子树深度减去右子树深度的值称为平衡因子BF,那么平衡二叉树所有节点的平衡因子只可能是-1,0,1。平衡二叉树的前提首先是一棵二叉排序树<br>
最小不平衡子树：距离插入结点最近的，且平衡因子的绝对值大于1的结点为根的子树<br>

	typedef struct BiTNode {
	int data;
	int bf;
	struct BiTNode, *lchild, *rchild;
	}BiTNode, *BiTree;
	void R_Rotate(BiTree *p) {
		BiTree L;
		L = (*p)->lchild;
		(*p)->lchild = L->rchild;
		L->rchild = (*p);
		*p = L;
	}
	void L_Rotate(BiTree *p) {
		BiTree R;
		R = (*p)->rchild;
		(*p)->rchild = R->lchild;
		R->lchild = (*p);
		*p = R;
	}

	#define LH +1 // 左高
	#define EH 0  // 等高
	#define Rh -1 // 右高
	void LeftBalance(BiTree *T) {
		BiTree L, Lr;
		L = (*T)->lchild;
		switch(L->bf) {
			case LH: 
				(*T)->bf = L->bf = EH;
				R_Rotate(T);
				break;
			case RH:
				Lr = L->rchild;
				switch(Lr->bf) {
					case LH:
				    	(*T)->bf = RH;
				    	L->bf = EH;
				    	break;
				    case EH: 
				    	(*T)->bf = L->bf = EH;
				    	break;
				    case RH: 
				    	(*T)->bf = EH;
				    	L->bf = LH;
				    	break;	
				}
				Lr->bf = EH;
				L_Rotate(&(*T)->lchild);
				R_Rotate(T);
		}
	}
	
	Status InsertAVL(BiTree *T, int e, Status *taller) {
		if(!*T) {
			*T = (BiTree)malloc(sizeof(BiTNode));
			(*T)->data = e;
			(*T)->lchild = (*T)->rchild = null;
			(*T)->bf = EH;
			*taller = true;
		} else {
			if(e == (*T)->data) {
				*taller = false;
				return false;
			}
			if(e < (*T)->data) {
				if(!InsertAVL(&(*T)->lchild, taller))
					return false;
				if(*taller) {
					switch((*T)->bf) {
						case LH:
							LeftBalance(T);
							*taller = false;
							break;
						case EH: 
							(*T)->bf = LH;
							*taller = true;
							break;
						case RH: 
							(*T)->bf = EH;
							*taller = true;
							break;
					}
				}
			}
			else {
				if(!InsertAVL(&(*T)->rchild, taller)) {
					return false;
				}
				if(*taller) {
					switch((*T)->bf) {
						case LH:
							(*T)->bf = EH;
							*taller = false;
							break;
						case EH: 
							(*T)->bf = RH;
							*taller = true;
							break;
						case RH: 
							R(T);
							*taller = false;
							break;
					}
				}
			}
		}
		return true;
	}


###多路查找树(B树)

####2-3树
2-3树：其中每一个结点都具有两个孩子（2结点）或者三个孩子（三结点）。<br>

> 一个2结点包含一个元素和两个孩子，左子树包含的元素小于该元素，右子树包含的元素大于该元素，这个2结点要么没有孩子，要么就有两个<br><br>
> 一个3结点包含一小一大两个元素和三个孩子,如果某个3结点有孩子的话，左子树包含小于较小元素的元素，右子树包含大于较大元素的元素，中间子树包含结余两元素之间的元素

2-3树插入

1. 对于空树，插入一个2结点即可
2. 插入到一个2结点的叶子上，只需要升级成3结点即可
3. 要往3结点插入一个新元素，由于3结点本身已经是2-3树的结点最大容量，因此就需要将其拆分，且将树中两元素或插入元素的三者中选择其一向上移动一层

2-3树删除

1. 所删除元素位于一个3结点的叶子上，这非常简单，只需要删除该元素即可，
2. 所删除元素位于一个2结点上，既要删除的是一个只有一个元素的结点。这样是不可以的，他不满足定义了,所以对于删除叶子是2结点的情况，分四种情形处理

> 此结点双亲也是2结点，且拥有一个3结点的右孩子，那么只需要左旋.<br><br>
> 此结点的双亲是2结点，右孩子也是2结点，如果直接左旋会造成没有右孩子，因此需要整颗树变形，并且保持二叉排序树的特点<br><br>
> 此结点双亲是一个3结点，于是将此结点拆分
> 如果当前是一个满二叉树的情况，此时删除任何一个叶子都会使得整棵树不能满足2-3树定义，删除叶子结点8时，就不得不考虑将2-3树层数减少

3.所删除结点位于非叶子的分支结点，此时通过将树按中序遍历后得到此元素的前驱或者后继元素


####B树
B树一种平衡的多路查找树。结点最大的孩子数目称为B树的阶<br>
一个m阶的B树具有如下属性：
	
> 如果根结点不是叶节点，则至少有两课子树<br>
> 每一个非根分支结点都有k-1个元素和k个孩子，每一个叶子结点n都有k-1个元素<br>
> 所有叶子结点都位于同一个层次<br>
> 所有分支结点包含下列信息(n, A0,K1,A1,K2,....,Kn,An)，其中Ki为关键字，指向子树根结点指针，且指针Ai-1所指子树中所有结点的关键字均小于Ki<br>


####B+树

一棵m阶的B+树和m阶的B树的差异在于：

- 有n棵子树的结点中包含n个关键字
- 所有叶子结点包含全部关键信息，及指向包含这些关键字记录的指针，叶子结点本身依关键字的大小自小而大的顺序连接
- 所有分支结点可以看成是索引，结点中仅含其子树中的最大关键字

###散列表查找（哈希表）
散列技术是在记录的存储位置和他的关键字之间建立一个确定的对应关系f，使得每个关键字key对应一个存储位置f。查找时，根据这个确定的关系找到给定值key的映射f中，若查找集合中存在这个记录，则必定在f的位置上<br>
这里我们把这种对应关系f称为散列函数，又称为哈希函数，采用散列技术记录存储一块连续的存储空间中，这款连续的存储空间称为散列表或者哈希表

<strong>注</strong>：哈希表经常会遇到一个问题，两个关键字key1≠key2，但是却有f(key1) = f(key2)，这种现象我们称为冲突，并把key1和key2称为这个散列函数的同义词


#####散列表的构造方法
1. 直接定址法
2. 数字分析法
3. 平方取中法
4. 折叠法
5. 除留余数法
6. 随机数法

#####处理冲突的方法
>开放地址法：一旦发生冲突，就去寻找下一个空的散列地址，只要散列表足够大，空的散列地址总能找到，并将记录存入。公式： fi(key) = (f(key)+di) mod m  ---> 线性探测法 <br>
	1. 随机探测法：在冲突时，对于位移量di采用随机函数计算得到

>再列散列函数法：事先准备多个散列函数<br>
>链地址法<br>
>公共溢出区法

#####散列表查找实现

	#define SUCCESS 1
	#define UNSUCCESS 0
	#define HASHSIZE 12
	typedef struct {
		int *elem;
		int count;
	}HashTable;
	int m = 0;
	Status InitHashTable(HashTable *H) {
		int i;
		m = HASHSIZE;
		H->count = m;
		H->elem = (int *)malloc(m*sizeof(int));
		for(i = 0; i < m; i++) {
			H-elem[i] = NULLKEY;
		return OK
	}
	int Hash(int key) {
		return key % m;
	}
	int InsertHash(HashTable *H, int key) {
		int addr = Hash(key);
		while(H->elem[addr] != NULLKEY) {
			H->elem[addr] = key;
		}
	}


##排序

####排序的稳定性
假设ki = kj,且在排序前的序列中ri领先于rj。如果排序后ri仍领先于rj，则称所用的排序的方法是最
稳定的；反之，若可能使得排序后序列中rj领先于ri，则称所用的排序方法是不稳定的
####内排序与外排序
内排序是在排序过程中，待排序的所有记录全部被放置在内存中。<br>
外排序是由于排序的记录个数太多，不能同时放置在内存中，整个排序过程需要在内外村之间多次交换数据才能进行。<br>
对于内排序来说，排序算法的性能主要受3各方面影响: 


1. 时间性能：在内排序中，主要进行两个操作是：比较和移动，尽可能少的关键字比较次数和尽可能少的记录移动次数
2. 辅助空间：执行算法所需要的辅助存储空间
3. 算法的复杂性 ----> 内排序分为：插入排序，交换排序，选择排序，归并排序


####冒泡排序
冒泡排序是一种交换排序：两两比较相邻记录的关键字，如果反序则交换，知道没有反序记录为止
