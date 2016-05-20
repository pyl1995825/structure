#树

###双亲表示法
>假设一组连续的空间存储数的结点，同时在每个结点中，附设一个指示器指示双亲结点到链表中的位置

	// 双亲表示法结构定义
	#define MAX_TREE_SIZE 100
	typedef int TElemType;
	typedef struct PTNode {
		TElemType data;5/18/2016 1:28:32 PM 
		int parent; //双亲位置
	}PTNode;
	typedef struct {
		PTNode ndoes[MAX_TREE_SIZE];
		int r,n;  // 根的位置和结点数
	}PTree;

这样的存储结构我们很容易的找到双亲结点，所用时间复杂度O(1)，知道parent为-1，如果要找结点的孩子是什么，需要遍历整个结构才行

###孩子表示法
>由于树种每个结点可能有多个子树，可以考虑多重链表，即每个结点有多个指针域，其中每个指针指向一个子树的根结点<br><br>
>具体办法，把每个结点的孩子结点排列起来，以单链表作存储结构，则n个结点有n个孩子链表，如果是叶子结点则此单链表为空，然后n个头指针有组成一个线性表，采用顺序存储<br><br>
>需要设计两种结点结构，一个是孩子链表的孩子结点，child-->数据域，用来存储某个节点在表头数组中的下标，next是指针域，用来存储指向某节点下一个孩子结点指针。另一个是表头数组，data-->是数据域，存储某节点数据，firstchild-->是头指针域，存储该结点的孩子链表的头指针

	//定义结构	
	#define MAX_TREE_SIZE 100
	typedef struct CTNode {
		int child;
		struct CTNode *next;
	}*ChildPtr;
	typedef struct {
		TElemType data;
		ChildPtr firstchild;
	}CTBox;
	typedef struct {
		CTBox nodes[MAX_TREE_SIZE];
		int r, n;
	}CTree;

这种方法存在问题，就是知道某个节点的双亲是谁比较麻烦，需要遍历整棵树，所以在CTBox中添加一个parent属性就可以了，这种方法被称为双亲孩子表示法

###孩子兄弟表示法
>任意一棵树，它的结点的第一个孩子如果存在就是唯一的，它的右兄弟存在也是唯一的，因此，设置两个指针，分别指向该结点的第一个孩子和此结点的右兄弟 data-->数据域，firstchild-->指针域，存储该结点的第一个孩子结点的存储地址，rightsib-->指针域，存储该结点的右兄弟结点的存储地址

	// 孩子兄弟结构表示法
	typedef struct CSNode {
		TElemType data;
		struct CSNode *firstchild, *rightsib;
	}CSNode, *CSTree;


##二叉树
###二叉树特点：

1. 每个结点最多有两棵子树，所以二叉树中不存在度大于2的结点。没有子树或者一棵子树也可以<br>
2. 左子树和右子树是有顺序的，次序不能颠倒<br>
3. 即使树中某节点只有一棵子树，也要区分他是左子树还是右子树

###特殊的二叉树
1.满二叉树：所有分支结点都存在左子树和右子树，并且所有叶子都在同一个层上<br>
>特点：（1）叶子只能出现在最下一层 （2）非叶子结点的度一定是2 （3)在同样深度的二叉树，满二叉树结点个数最多

2.完全二叉树：对一棵具有n个结点的二叉树按层序编号，如果编号为i的结点与同样深度的满二叉树编号为i的结点在二叉树中位置完全相同
>特点：（1）叶子结点只能出现在最下两层 （2）最下层的叶子一定集中在左部连续的位置 （3）倒数二层，若有叶子结点，一定都在右部连续位置 （4）如果结点度为1，则该结点只有左孩子 （5）同样的节点数的二叉树，完全二叉树深度最小

###二叉树的存储结构
####顺序存储
顺序存储：通过用一维数组存储二叉树的结点，并结点的存储位置，也就是数组下标体现结点之间的逻辑关系（一般只用于存完全二叉树）
####二叉链表
二叉树每个结点最多有两个孩子，所以设计一个数据域和两个指针域，称这样的链表叫二叉链表
	
	typedef struct BiTNode {
		TElemType data;
		struct BiTNode *lchild, *rchild;
	}BiTNode, *BiTree;

###遍历二叉树
二叉树的遍历是指从根结点出发，按照某种次序依次访问二叉树中所有结点，使得每个结点被访问一次且仅被访问一次<br>
#####遍历方式：
>1. 前序遍历：先访问根结点，然后前序遍历左子树，在前序遍历右子树
>2. 中序遍历：从根结点开始（并不是先访问根结点），中序遍历根结点的左子树，然后访问根结点，最后中序遍历右子树
>3. 后序遍历：从左到右先叶子后结点遍历左右子树，最后访问根结点
>4. 层序遍历：从根结点开始访问，从上而下逐层遍历，在同一层中，按从左到右顺序访问

####遍历算法（递归）
	
	// 前序遍历算法
	void PreOrderTraverse(BiTree T) {
		if(T == null) {
			return ;
		}
		printf("%c", T->data);
		PreOrderTraverse(T->lchild);
		PreOrderTraverse(T->rchild);
	}
	//中序遍历算法
	void InOrderTraverse(BiTree T) {
		if(T == null) {
			return ;
		}
		InOrderTraverse(T->lchild); // 递归找到左子树的叶子
		printf("%c", T->data);  
		InOrderTraverse(T->rchild);
	}
	//后序遍历
	void PostOrderTraverse(BiTree T) {
		if(T == null) {
			return ;
		}
		PostOrderTraverse(T->lchild); // 先后序遍历左子树
		PostOrderTraverse(T->rchild); // 再后序遍历右子树
		printf("%c", T->data);
	}


####遍历算法（非递归）
######结构初始化

	typedef struct seqstack {
		BiTree data[MAXSIZE];
		int tag[MAXSIZE];  // 为后续遍历准备
		int top;	// top为数组下标
	}seqstack;
	void push(seqstack *s, bintree t) {
		if(s->top == MAXSIZE) {
			return -1;
		}else {
			s->top++;
			s->data[s->top] = t;
	}
	void pop(seqstack *s) {
		if(s->top == -1) 
			return null;
		else 
			s->top--;
			return s->data[s->top+1];
	}
		 
######非递归前序遍历

	void PreOrder(BiTree t) {
		seqstack s;
		s.top = -1;
		if(!t) return -1;
		else {
			while(t || s.top != -1) {
				while(t) {
					printf("%c", t->data);
					push(&s,t);			
					t = t->lchild;
				}
				t = pop(&s);
				t = t->rchild;
			}
		}
	}	
######非递归中序遍历

	void InOrder(BiTree t) {
		seqstack s;
		s.top = -1;
		if(!t) return -1;
		else {
			while(t || s.top = -1) {
				while(t) {
					push(&s, t);
					t = t->lchild;
				}
				t = pop(&s);	
				printf("%c", t->data);
				t = t->rchild;
			}
		}
	} 
######非递归后序遍历

	void PostOrder(BiTree t) {
		seqstack s;
		s.top = -1;
		if(!t) return -1;
		else {
			while(t || s.top != -1) {
				while(t) {
					push(&s, t);
					s.tag[s.top] = 0;
					t = t->lchild;
				}		
				if(s.tag[s.top] == 0) {
					t = s.data[s.top];
					s.tag[s.top] = 1;
					t = t->rchild;
				}else {
					while(s.tag[s.top] == 1) {
						t = pop(&s);
						printf("%c", t->data);
					}
					t = null;
				}
			}
		}
	}

######非递归层次遍历
层次遍历存储有先进先出，所以选择队列存储。队列定义:
	
	#define MAX 1000  
	typedef struct seqqueue{  
	    bintree data[MAX];  
	    int front;  
	    int rear;  
	}seqqueue;  
	  
	  
	void enter(seqqueue *q,bintree t){  
	    if(q->rear == MAX){  
	        printf("the queue is full!\n");  
	    }else{  
	        q->data[q->rear] = t;  
	        q->rear++;  
	    }  
	}  
	  
	bintree del(seqqueue *q){  
	    if(q->front == q->rear){  
	        return NULL;  
	    }else{  
	        q->front++;  
	        return q->data[q->front-1];  
	    }  
	} 
	
	void level_tree(bintree t){  
	    seqqueue q;  
	    bintree temp;  
	    q.front = q.rear = 0;  
	    if(!t){  
	        printf("the tree is empty\n");  
	        return ;  
	    }  
	    enter(&q,t);  // 放入头结点
	    while(q.front != q.rear){  
	        t=del(&q);  
	        printf("%c ",t->data);  
	        if(t->lchild){  
	            enter(&q,t->lchild); // 从左子树开始遍历  
	        }  
	        if(t->rchild){  
	            enter(&q,t->rchild);  // 之后遍历右子树
	        }  
	    }  
	} 
######统计结点个数
	
	int countTree(bintree t) {
		if(t) {
			return (countTree(t->lchild) + countTree(t->rchild) + 1);
		}
		return 0;
	}

######求二叉树深度
	
	int depth(bintree t) {
		int h, right,left;
		if(!t) {
			return 0;
		}
		left = depth(t->lchild);
		right = depth(t->rchild);
		h = right > left ? right : left;
		return h+1;
	}
注： <br>
1. 已知前序遍历序列和中序遍历序列，可以唯一确定一棵二叉树<br>
2. 已知后序遍历序列和中序遍历序列，可以唯一确定一棵二叉树<br>
3. 已知前序和后序遍历，不能唯一确定一棵二叉树，无法知道左子树和右子树


###二叉树的建立

	void CreateBiTree(BiTree *T) {
		TElemType ch;
		scanf("%c", &ch);
		if(ch == "#") {
			*T = null;
		}else {
			*T = (BiTree)malloc(sizeof(BiTNode));
			if(!*T) {
				exit(OVERFLOW);
				(*T)->data = ch;
				CreateBiTree(&(*T)->lchild);
				CreateBiTree(&(*T)->rchild);
			}
		}
	}

###树，森林与二叉树的转换
#####树转化为二叉树
1. 加线，在所有兄弟结点之间加一条线
2. 去线，对数中每个结点，只保留它与第一个孩子结点的连线，删除它与其他孩子结点之间的连线
3. 层次调整，第一个孩子是二叉树结点的左子树，兄弟转化过来的是右子树


#####森林转化为二叉树
1. 把每个树转化为二叉树
2. 第一棵二叉树不动，从第二棵二叉树开始，依次把后一棵二叉树的根结点作为前一棵二叉树的根结点的右子树，然后之后的依次连起来


#####二叉树转化为树
1. 加线。
2. 去线
3. 调整结构


#####二叉树转化为森林
1. 从根结点开始，若右子树存在，把与右子树结点的连线删除，再查看分离后的二叉树，若右子树存在，则连线删除，知道所有右子树连线都删除为止
2. 再将每棵树分离后的二叉树转化为树即可


###赫夫曼树