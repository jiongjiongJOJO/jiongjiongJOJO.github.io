---
title: 数据结构实验五：二叉树的基本操作
author: 囧囧JOJO
top: false
cover: false
mathjax: false
tags: [数据结构, C++, 笔记, 入门, 教程]
date: 2021-12-10 00:07:16
img:
coverImg:
password:
summary:

---

# 二叉树的基本操作

## 1 实验目的

1. 掌握二叉树的存储实现；
2. 掌握二叉树的遍历思想；
3. 掌握二叉树的常见算法的程序实现。

## 2 实验内容

1. 建立一棵含有n个结点的二叉树，采用二叉链表存储。按照教材p247中图7.33进行创建；
2. 输出二叉树；
3. 输出’H’结点的左右孩子结点值；
4. 输出二叉树b的高度；
5. 释放二叉树。

## 3 软件程序

btree.cpp：

```c++
#include <malloc.h>  
#include <stdio.h>  
#define MaxSize 100  
  
typedef int ElemType;  
typedef struct node  
{  
    ElemType data;  
    struct node* lchild;  
    struct node* rchild;  
} BTNode;  
  
void CreateBTree(BTNode * &b,char * str) // 创建二叉树  
{  
    BTNode* St[MaxSize], * p=NULL;  
    int top = -1, k, j = 0;  
    char ch;  
    b = NULL;  
    ch = str[j];  
    while (ch != '\0') {  
        switch (ch)   
        {  
            case '(':top++; St[top]=p; k = 1; break;  
            case ')':top--; break;  
            case ',':k = 2; break;  
            default:p = (BTNode*)malloc(sizeof(BTNode));  
                p->data = ch;  
                p->lchild = p->rchild = NULL;  
                if (b == NULL)  
                    b = p;  
                else   
                {  
                    switch (k)  
                    {  
                    case 1:St[top]->lchild = p; break;  
                    case 2:St[top]->rchild = p; break;  
                    }  
                }  
        }  
        j++;  
        ch = str[j];  
    }  
}  
  
void DestroyBTree(BTNode*& b)// 销毁二叉树  
{  
    if (b != NULL)   
    {  
        DestroyBTree(b->lchild);  
        DestroyBTree(b->rchild);  
        free(b);  
    }  
}  
  
BTNode * FindNode(BTNode * b, ElemType x)  
{  
    BTNode * p;  
    if (b == NULL)  
        return NULL;  
    else if (b->data == x)  
        return b;  
    else  
    {  
        p = FindNode(b->lchild, x);  
        if (p != NULL)  
            return p;  
        else  
            return FindNode(b->rchild, x);  
    }  
}  
  
BTNode* LchildNode(BTNode* p)// 找孩子节点（左）  
{  
    return p->lchild;  
}  
BTNode* RchildNode(BTNode* p) // 找孩子节点（右）  
{  
    return p->rchild;  
}  
  
int BTHeight(BTNode* b)// 求高度  
{  
    int lchildh, rchildh;  
    if (b == NULL)return(0);  
    else {  
        lchildh = BTHeight(b->lchild);  
        rchildh = BTHeight(b->rchild);  
        return (lchildh > rchildh) ? (lchildh + 1) : (rchildh + 1);  
    }  
}  
  
void DispBTree(BTNode* b) // 输出二叉树  
{  
    if (b != NULL)  
    {  
        printf("%c", b->data);  
        if (b->lchild != NULL || b->rchild != NULL)  
        {  
            printf("(");  
	        DispBTree(b->lchild);  
	        if (b->rchild != NULL)printf(",");  
	        DispBTree(b->rchild);  
	        printf(")");  
	    }  
	}  
}  

```

main.cpp：

```c++
#include<stdio.h>  
#include<stdlib.h>  
#include<malloc.h>  
/*将二叉链表结构定义和其基本运算函数定义放到这里*/  
#include "btree.cpp"  
/*                                              */  
int main()  
{  
    BTNode* b, * p, * lp, * rp;;  
    printf("二叉树的基本运算如下:\n");  
    printf("  (1)创建二叉树\n");  
    CreateBTree(b, "A(B(D,E(H(J,K(L,M(,N))))),C(F,G(,I)))");  
    printf("  (2)输出二叉树:"); DispBTree(b); printf("\n");  
    printf("  (3)H结点:");  
    p = FindNode(b, 'H');  
    if (p != NULL)  
    {  
        lp = LchildNode(p);  
        if (lp != NULL)  
            printf("左孩子为%c ", lp->data);  
        else  
            printf("无左孩子 ");  
        rp = RchildNode(p);  
        if (rp != NULL)  
            printf("右孩子为%c", rp->data);  
        else  
            printf("无右孩子 ");  
    }  
    printf("\n");  
    printf("  (4)二叉树b的高度:%d\n", BTHeight(b));  
    printf("  (5)释放二叉树b\n");  
    DestroyBTree(b);  
    return 1;  
}  

```

## 4 实验结果

![](/assets/images/myVeTcHRy/1639066065159.png)
