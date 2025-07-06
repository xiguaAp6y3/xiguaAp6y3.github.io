---
title: 数据结构 - 堆栈
published: 2024-07-06
description: "堆栈的抽象数据类型描述及其C语言实现"
tags: ["数据结构", "堆栈", "Stack"]
category: 数据结构
draft: false
---

# 数据结构 2.2 堆栈

## 堆栈的抽象数据类型描述

**堆栈（Stack）**：具有一定操作约束的线性表
- 只在一端（栈顶，Top）做插入、删除
- 插入数据：入栈（Push）
- 删除数据：出栈（Pop）
- 后入先出：Last In First Out（LIFO）

> 例如：放在桌子上的碗，后放上去的碗要先拿走

### 数据对象集
一个有0个或多个元素的有穷线性表。

### 操作集
长度为 `MaxSize` 的堆栈 $S \in Stack$，堆栈元素 $item \in ElementType$

1. `Stack CreateStack( int MaxSize )`：生成空堆栈，其最大长度为 `MaxSize`
2. `int IsFull( Stack S, int MaxSize )`：判断堆栈$S$是否已满
3. `void Push( Stack S, ElementType item )`：将元素$item$压入堆栈
4. `int IsEmpty ( Stack S )`：判断堆栈$S$ 是否为空
5. `ElementType Pop( Stack S )`：删除并返回栈顶元素

---

## 1. 数组实现的堆栈

### 1.1 基本结构定义

```c
#include <stdio.h>
#include <malloc.h>
const int N = 1e5 + 10;

typedef struct Snode *Stack;
struct Snode {
    int data[N];
    int top;
};
```

### 1.2 堆栈操作实现

```c
Stack CreateStack() {
    Stack st = (Stack)malloc(sizeof (struct Snode));
    if(st == NULL) {
        printf("创建失败\n");
        return NULL;
    }
    st->top = -1;
    return st;
}

bool isEmpty(Stack ptr) {
    if(ptr->top == -1) return 1;
    else return 0;
}

bool isFull(Stack ptr) {
    if(ptr->top == N-1) return 1;
    else return 0;
}

void push(int x, Stack ptr) {
    if(ptr->top == N-1) {
        printf("堆栈满了\n");
        return;
    }
    else {
        ptr->data[++(ptr->top)] = x;
        return;
    }
}

int pop(Stack ptr) {
    // 0x3f3f代表错误
    if(ptr->top < 0 || ptr->top >= N) {
        printf("非法弹出\n");
        return 0x3f3f3f3f;
    }
    else {
        return ptr->data[(ptr->top)--];
    }
}
```

### 1.3 测试代码

```c
int main() {
    Stack pt = CreateStack();
    push(1, pt);
    push(22, pt);
    push(333, pt);
    for(int i = 0; i < 3; i++)
        printf("%d", pop(pt));
    return 0;
}
```

---

## 2. 一个数组实现两个堆栈

### 2.1 设计思路

```
一个数组 ||||->  <-|||| 这样可以最大化利用空间
```

两个堆栈分别从数组的两端开始，向中间增长。

### 2.2 结构定义

```c
#include <stdio.h>
#include <malloc.h>
const int MAXSIZE = 5000;

typedef struct DSnode* DStack;
struct DSnode {
    int data[MAXSIZE];
    int top1, top2;
};
```

### 2.3 双栈操作实现

```c
DStack CreateDStack() {
    DStack tmp = (DStack)malloc(sizeof(struct DSnode));
    if(tmp == NULL) {
        printf("创建失败");
        return NULL;
    }
    tmp->top1 = -1;
    tmp->top2 = MAXSIZE;
    return tmp;
}

bool isFULL(DStack ptr) {
    if(ptr->top1 == (ptr->top2) - 1) return 1;
    else return 0;
}

bool isEmpty(DStack ptr, int lr) {
    if(lr == 1) {
        if(ptr->top1 == -1) return 1;
        else return 0;
    }
    else if(lr == 2) {
        if(ptr->top2 == MAXSIZE) return 1;
        else return 0;
    }
    else {
        printf("未知的栈操作");
        return 0;
    }
}

void push(int x, DStack ptr, int lr) {
    if(isFULL(ptr)) {
        printf("满");
        return;
    }
    else {
        if(lr == 1) ptr->data[++(ptr->top1)] = x;
        else ptr->data[--(ptr->top2)] = x;
    }
}

int pop(DStack ptr, int lr) {
    if(isEmpty(ptr, lr)) {
        printf("空");
        return 0x3f3f3f3f;
    }
    else {
        if(lr == 1) {
            return ptr->data[(ptr->top1)--];
        }
        else if(lr == 2) {
            return ptr->data[(ptr->top2)++];
        }
        else {
            printf("未知的栈操作");
            return 0x3f3f3f3f;
        }
    }
}
```

### 2.4 测试代码

```c
int main() {
    DStack pt = CreateDStack();
    push(3, pt, 1);
    push(4, pt, 1);
    push(2, pt, 2);
    push(1, pt, 2);
    
    for(int i = 0; i < 2; i++)
        printf("%d ", pop(pt, 2));
    for(int i = 0; i < 2; i++)
        printf("%d ", pop(pt, 1));
    return 0;
}
```

---

## 3. 堆栈的链式存储

### 3.1 结构图示

```
head -> [] -> [] -> [] -> NULL
```

单链表实现，显然应该在左边插入删除，这样方便操作。

### 3.2 结构定义

```c
#include <stdio.h>
#include <malloc.h>
#include <stdlib.h>

typedef struct SLnode* Stack;
struct SLnode {
    int data;
    Stack next;
};
```

### 3.3 链式堆栈操作实现

```c
Stack CreateStack() {
    Stack tmp = (Stack)malloc(sizeof(struct SLnode));
    if(tmp == NULL) {
        printf("创建失败");
        return NULL;
    }
    else {
        tmp->next = NULL;
        return tmp;
    }
}

bool isEmpty(Stack ptr) {
    if(ptr->next == NULL) return 1;
    else return 0;
}

void push(int x, Stack ptr) {
    Stack tmp = (Stack)(malloc(sizeof(struct SLnode)));
    if(tmp == NULL) {
        printf("创建失败");
        return;
    }
    tmp->next = ptr->next;
    ptr->next = tmp;
    tmp->data = x;
}

int pop(Stack ptr) {
    if(isEmpty(ptr)) {
        printf("空的");
        return 0x3f3f3f3f;
    }
    else {
        Stack tmp = ptr->next;
        int tmp2 = tmp->data;
        ptr->next = tmp->next;
        free(tmp);
        return tmp2;
    }
}
```

### 3.4 测试代码

```c
int main() {
    Stack pt = CreateStack();
    push(1, pt);
    push(233, pt);
    push(3, pt);
    pop(pt);
    push(4, pt);
    
    for(int i = 0; i < 3; i++)
        printf("%d ", pop(pt));
    return 0;
}
```

---

## 4. 堆栈的应用

### 4.1 表达式求值

在堆栈内计算时有些符号优先级小，如左括号。

堆栈常用于：
- 表达式求值
- 函数调用
- 括号匹配
- 中缀表达式转后缀表达式

---

*最后更新：2024年7月6日*
