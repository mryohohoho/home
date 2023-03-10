---
title: "Datastruct"
description: "datastruct"
keywords: "datastruct"

date: 2023-03-10T22:39:41+08:00
lastmod: 2023-03-10T22:39:41+08:00

categories:
  -
tags:
  -
  -

# 原文作者
# Post's origin author name
#author:
# 原文链接
# Post's origin link URL
#link:
# 图片链接，用在open graph和twitter卡片上
# Image source link that will use in open graph and twitter card
#imgs:
# 在首页展开内容
# Expand content on the home page
#expand: true
# 外部链接地址，访问时直接跳转
# It's means that will redirecting to external links
#extlink:
# 在当前页面关闭评论功能
# Disabled comment plugins in this post
#comment:
#  enable: false
# 关闭文章目录功能
# Disable table of content
#toc: false
# 绝对访问路径
# Absolute link for visit
#url: "datastruct.html"
# 开启文章置顶，数字越小越靠前
# Sticky post set-top in home page and the smaller nubmer will more forward.
weight: 1
# 开启数学公式渲染，可选值： mathjax, katex
# Support Math Formulas render, options: mathjax, katex
#math: mathjax
# 开启各种图渲染，如流程图、时序图、类图等
# Enable chart render, such as: flow, sequence, classes etc
#mermaid: true
---

# 数据结构笔记

---

二叉树已更新完毕

<!--more-->
<toc>

# 基础算法汇总

## part1 线性表、栈、队列

### 1.1线性表 
1. **顺序表插入元素操作**
//初始条件：顺序线性表 L 已存在， 1 ≤ i ≤ ListLength(L)
//操作结果：在 L 中第 i 个位置之前插入新的数据元素 e,L 的长度加 1
```c
int ListInsert(SeqList *L,int i,ElemType *e){
    int k;
    if(L->length == MAXSIZE) //顺序线性表已经满
        return ERROR;
    if (i<1 || i>L->length+ 1) //当 i 不在范围内时 
        return ERROR;
    if( i<=L->length) {//若插入数据位置不在表尾
        //将要插入位置后 的 数据元素向后动一位
        for(k=L->length + 1; k >= i - 1; k--) 
            L ->data[k+1]=L->data[k];
    }
    L ->data[i+1]=e;//将新元素插入
    L->length ++;
    return OK;
}
```
在上述代码中，核心语句为for 循环里面的判断，对顺序表进行插入操作，应该先进
行位置移动，然后才能进行插入操 作。**所以顺序线性表的插入，时间复杂度为 O(n)**。


2. **顺序表按位删除元素操作**
初始条件： 顺序线性表 L 已存在， 1 ≤ i ≤ List.Length(L)
操作结果：删除 L 的第 i 个数据元素，并用 e 返回其值， L 的长度减 1
```c
int ListDelete(SqList *L,int i,ElemType *e){
    int k;
    if (L->length == 0)//线性表为空
        return ERROR;
    if(i<1|| i>L ->length) //删除位置不正确
        return ERROR;
    *e=L->data[i-1];
    if(i != L->length  +1){ //如果删除不是最后位置
        for (k=i;k< L->length;k++) 将删除位置后继元素前移
        L->data[k-1]=L->data[k];
    L ->length--:
    }
    return OK;
}
```
该删除算法关键步骤是：从删除元素位置开始遍历到最后一个元素位置，分别依次将它
们向前移动一个位置，然后对第 i 个元素进行删除。 因为**在进行删除之前还需要一次跟插入一样的移动操作，故删除的平均算法时间也为 O(n)**。
3. **单链表按位查找**
初始条件：顺序线性表已存在， 1≤i≤ListLength(L)
操作结果：用 e 返回表中第 i 个数据元素的值
```c
int GetElem(LinkList L,int i,ElemType *e){
    int j=1; //j 为计数器
    LinkList *p; //声明一 节点p
    p=L->next; //p 指向链表 L 的第一个节点
    while(p && j<i) { 当 p 不为空 并且 计数器 不 等于 i 时，循环继续
        p=p->next //p 指向下一个 节点
        j++;
    }
    if(!p || j > i)
        return ERROR;
    *e = p ->data;
    return OK;
}
```
**代码题中记得写上容错机制。**
4. **单链表插入操作**
初始条件：链表已存在， 1 ≤ i ≤ ListLength L
操作结果： 链表的在第 i 处插入新元素 e
```c
int ListInsert(LinkList *L,int i,ElemType e){
    int j=1；
    LinkList *p,*s;
    p=L->next;
    while (p && j<i) {//寻找第 i 个 节点
        p=p -> next;
        j++;
    }
    if (!p || j>i)
        return ERROR; //第 i 个元素不存在
    s=(LinkList *) malloc (sizeof(Node)) //生成新节点 (C 标准函数）
    s ->data= e;
    s ->next=p ->next;// 将 p 的后继节点赋值给 s 的后继
    p ->next = s;//将 s 赋值给 p 的后继
    return OK;
}
```
5. **链表逆置**
方法一：
算法思想：修改指针即可，head 指针遍历链表不断向前移动，用 p 记录 head的之前的一个节点，修改各个节点指针域 next，指向 p，用一个 temp指针指向（保存）head 的下一个节点。
```c
ListNode* reverseList(ListNode* head){
    ListNode *temp,*p = NULL;
    while(head != NULL) {
        temp = head ->next;
        head -> next =  p;
        p = head;
        head = temp;
    }
    return p;
}
```
方法二：递归(无头结点代码)
```c
ListNode* reverseList(ListNode* head){
    if(head == NULL || head -> next == NULL)//链表为空or只有一个结点
        return head;
    ListNode newhead = reverseList(head -> next);//递归到链尾
    head->next->next = head;//反转链表
    head -> next = NULL;
    return newhead;//newhead始终指向新链表的头
}
```
5. **逆置循环双链表（含两个以上节点）**
算法思想：只交换节点中的数据成员 data ，其他的前后指针不变。
```c
void Reverse () {
    swap(_head_ ->_data, _tail_ ->_data);
    LinkList * begin = _head->next;
    LinkList * end = _tail->prev;
    //以上为修正后，替换了以下内容
    // LinkList * begin = _head;
    // LinkList * end = _tail;
    while (begin != end && begin ->_prev != end)
    {
    swap(begin ->_data, end ->_data);
    begin = begin ->_next;
    end = end ->_prev;
    }
}
```

## part2 KMP

## part3 树

### 3.1二叉树遍历
1.  **先序遍历**
```c
//递归算法
void PreOrder(Bitree T){
    if(T != NULL) {
        visit(T);
        PreOrder(T->lchild);
        PreOrder(T->rchild);
    }
}

//非递归算法
void PreOrder(Bitree T){
    InitStack(S);
    BiTree p = T;
    while(p || !IsEmpty(S)){
    if (p){
        visit(p);
        Push(S,p);
        p = p->rchild;
    }
    else{
        Pop{S,p};
        p = p->lchild;
    }
    }
}
```
2. **中序遍历**
```c
//递归算法
void InOrder(BiTree T){
    if (T != NULL){
        InOrder(T->lchild);
        visit(T);
        InOrder(T->rchild);
    }
}
```
```c
//非递归算法
void InOrder(BiTree T){
    InitStack(s);
    BiTree p = T;
    while(p || IsEmpty(S)){
        if(p){
            Push(S,p);
            p = p->lchild;
        }
        else {
            Pop(S,p);
            visit p;
            p =  p->rchild;
        }
    }
}
```
3. **后序遍历**
```c
//递归算法
void PostOrder(BiTree T){
    if(T != NULL){
        PostOrder(T->lchild);
        PostOrder(T->rchild);
        visit(T);
    }
}
```
```c
//非递归算法
void PostOrder(BiTree T){
    InitStack(S);
    BiTree p = T;
    BiTree r = NULL;
    while (p || IsEmpty(S)){
        if (p){
            Push(S,p);
            p = p->lchild;
        }
        else {
            GetTop(S,p);
            if(p->rchild && p->rchild != r) //p的右子树存在，且未被访问过
                p = p->rchild;
            else{
                Pop(S,p);
                visit(p);
                r = p; //记录最近访问过的结点
                p = NULL;//结点访问完后，重置p指针
            }

        }
    }
}
```
4. **层次遍历**
```c
void LevelOrder(BiTree T){
    InitQueue(S);
    BiTree p = T;
    EnQueue(S,p);
    while(!IsEmpty(S)){
        DeQueue(S,p);
        visit(p);
        if(p->lchild)
        EnQueue(S,p->lchild);
        if(p->rchild)
        EnQueue(S,p->rchild);
    }
}
```
### 3.2线索二叉树
1. **中序遍历对二叉树线索化（递归）**
```c
void InThread(ThreadTree &p,ThreadTree &pre){
    if(p!=NULL){
        InThread(p->lchild,pre); //递归，线索化左子树
        if(p->lchild == NULL){
            p->lchild = pre;  //左子树为空，建立前驱结点
            p->ltag = 1;
        }
        if(pre!=NULL&&pre->rchild  == NULL){
            pre->rchild = p; //建立前驱结点的后继结点
            pre->rtag = 1;
        }
        pre = p; //标记当前结点为刚刚访问过的结点
        InTread(p->rchild,pre); //递归，线索化右子树
    }
}

void CreateInThread(ThreadTree T){
    ThreadTree pre = NULL;
    if(T!=NULL){    //非空二叉树，线索化
        InThread(T,pre); //线索化二叉树
        pre->rchild=NULL; //处理最后一个结点
        pre->rtag = 1;
    }
}
```
2. **中序线索二叉树的遍历**
在对其遍历时，只需要先找到序列里的第一个结点，然后依次找结点的后继，直到后继为空。
中序线索二叉树后继的规律是：若其右标志为“1",则为线索，指示其后继，否则遍历右子树种第一个访问的结点（右子树最左下的结点）为其后继。
```c
//求中序线索二叉树的中序序列下第一个结点
ThreadNode *Firstnode(ThreadNode *p){
    while(p->ltag == 0)  p = p->lchild; //最左下结点（不一定是叶子结点）
    return p;
}
//求中序线索二叉树种结点p在中序序列下的后继
ThreadNode *Nextnode(ThreadNode *p){
    if(p->rtag == 0) return Firstnode(p->rchild);
    else return p->rchild;
}

void InOrder(ThreadNode *T){
    for(ThreadNode *p  = Firstnode(T);p!=NULL;p = Nextnode(p))
    visit(p);
}
```
### 3.3二叉排序树与平衡二叉树
1. **二叉排序树的插入**
```c
int BST_Insert(BiTree &T,KeyType k){
    if(T==NULL){
        T == (BiTree)malloc(sizeof(BSTNode));
        T -> key =k;
        T->lchild = T->rchilid = NULL;
        return 1;  //返回1，插入成功
    }
    else if(k == T->key)    //树种存在相同关键字结点，插入失败
        return 0;
    else if(k < T->key) //插入到左子树
        return BST_Insert(T->lchild,k);// BST_Insert(T->lchild,k);
    else                //插入到右子树
        return BST_Insert(T->rchild,k);
}
```
2. **二叉排序树的构造**
```c
void Creat_BST(BiTree &T,KeyType str[],int n){
    T=NULL;     //初始化T为空树
    int i = 0;
    while(i < n) {  //依次将每个关键词插入树中
        BST_Insert(T,str[i]);
        i++;
    }
}
```
3. **判断二叉树是否为平衡二叉树**
算法思想：设置二叉树平衡标记balance，标记返回二叉树bt是否为平衡二叉树，若为平衡二叉树则返回1，否则返回0；h为二叉树bt的高度。采用后序遍历递归算法：
    1. 若bt为空，则高度为0，balance=1；
    2. 若bt仅有根节点，则高度为1，balance=1；
    3. 否则，对bt的左、右边子树执行递归运算，返回左右子树的高度和平衡标记，bt的高度为最高子树的高度+1。若左右子树高度差大于1，则balance=0；若左右子树的高度差小于等于1，且左右子树都平衡时，balance=1，否则balance=0。
```c
void Judge_AVL(BiTree bt,int &balance,int &h){
    int bl = 0, br = 0, hl = 0, hr = 0; //左右子树平衡标记和高度
    if(T == NULL){
        h = 0;
        balance = 1;
    }
    else if(T->lchild == NULL && T->rchild == NULL){
        h = 1;
        balance = 1;
    }
    else{
        Judge_AVL(T->lchild,bl,hl);//递归判断左子树
        Judge_AVL(T->rchild,br,hr);//递归判断右子树
        h = (hl>hr?hl:hr) +1;
        if (abs(hl - hr)<2>)//若字数高度差的绝对值<2，则看左右子树是否都平衡
            balance = bl&&br;//左右子树都平衡时，二叉树平衡
        else
            balance = 0;

    }
}
```
4. **判断给定二叉树是否二叉排序树**
对二叉排序树来说，其中序遍历为一个递增有序序列。因此对给定的二叉排序树进行遍历，始终能保持前一个值比后一个值小，则说明改二叉树时一棵二叉排序树。
```c
KeyType predt = -32767;   //predt为全局变量，保村当前结点中序前驱的值，初始值为负无穷
int JudgeBST(BiTree bt){
    int bl,br;
    if (bt == NULL) //空树
        return 1;
    else {
        bl = JudgeBST(bt -> lchild);
        if(bl == 0 || predt >= bt->date)
            return 0;
        predt  = bt->data;
        br = JudgeBST(bt -> rchild);
        return br;
    }
}
```
   - 思路2：用先序遍历，每处理一个一个结点，对比其根节点与左右节点的值。

## part4 图

### 4.1图的遍历
1. **深度优先遍历DFS**
基本思想：首先从图中某个顶点 v0 出发，访问此顶点，然后依次从 v0 相邻的顶点出发
深度优先遍历，直至图中所有与 v0 路径相通的顶点都被访问了；若此时尚有顶点未被访问，
则从中选一个顶点作为起始点，重复上述过程，直到所有的顶点都被访问。可以看出深度优
先遍历是一个递归的过程。这种遍历过程类似树的先序遍历，均是先访问 节点 ，再从该 节点
出发继续向下遍历。
```c
//伪代码
bool visited[Max_Vex];// 定义访问标记数组，为了防止重复访问
void DFSTraverse(Graph G){
    for(v=0;v <G.vexnum;v++)
        visited[v]=false;// 初始化标记数组
    for (v=0;v<G.vexnum;++
    if(!visited[v])
    如果 v 未被访问，那么从 v 起，开始做深度优先遍历
    DFS(G,v);
}
void DFS(Graph G,int v){
    visit(v);
    visited[v]=true; //定义为已访问
    for (w=FirstNeighbor(G,v);w>=0;w=NextNeighbor(G,v,w))
        if(! visited[w])
            DFS(G,w);
}
```
2. **广度优先遍历**
```c
bool visited[Max_Vex];// 定义访问 标记数组
void BFSTraverse(Graph G){
    for( i =0;i<G.vexnum;++i)
    visited[v]=false;// 初始化标记数组
    InitQueue(Q);//初始化队列
    for (v=0;v<G.vexnum;++v)
    if(!visited[v])//如果 v 未被访问，那么从 v 起，开始做广度优先遍历
    BFS(G,v);
}
void BFS(Graph G,int v){
    visit(v);
    visited[v]=true;// 定义为已访问
    while(!isEmpty(Q)){
    DeQueue(Q,v);//顶点v出队列
    for (w=FirstNeighbor(G,v);w>=0;w=NextNeighbor(G,v,w))
    if(!visited[w]){
        visit(w);
        visited[w]=true;
        EnQueue(Q,w);//顶点w入队
}//if
}//while
}
```
## part5 查找、排序