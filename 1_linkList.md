# 链表

## 例一：实现一种算法，找出单向链表中倒数第k个结点，返回该结点的值

- 方法一：首先找出单链表的长度，再定位到所要找到的结点位置	

```c++
/*
 * 无头结点链表
 * 结点结构如下
 * struct node
 * {
 *     int val;
 *     node* next;
 *     node(int x) : val(x), next(nullptr) {}
 * }
 */
int kthToLast(node* head, int k)
{
    int count = 0;
    node* ptr = head;
    while (ptr != nullptr) {
        ++count;
        ptr = ptr->next;
    }
    int num = count - k + 1;
    ptr = head;
    for (int i = 1; i < num; ++i) {
        ptr = ptr->next;
    }
    return ptr->val;
}
```

- 方法二：快慢指针，快指针提前k个位置先走，当快指针指向最后的结点，慢指针真好指向目标结点

```c++
int kthToLast(node* head, int k)
{
    int count = 0;
    node* quick = head; // 定义快指针
    while (count != k) {
        ++count;
        quick = quick->next;
    }
    node* slow = head; // 慢指针比快指针慢k个结点
    while (quick != nullptr) {
        // 当快指针指向最后的节点时，慢指针指向目标结点
        quick = quick->next;
        slow = slow->next;
    }
    return slow->val;
}
```

- 方法三：使用栈解决

```c++
int kthToLast(node* head, int k)
{
    stack<node*> S;
    while (head != nullptr) {
        S.push(head);
        head = head->next;
    }
    node* ptr;
    while (k != 0) {
        ptr = S.top();
        S.pop();
        --k;
    }
    return ptr->val;
}
```

- 方法四：使用递归

```c++
int size = 0; // 全局变量，记录递归往回走时候访问的结点数量
int kthToLast(node* head, int k)
{
    /* 终止条件 */
    if (head == nullptr) {
        return 0;
    }
    /* 递归调用 */
    int val = kthToLast(head->next, k);
    /* 逻辑处理 */
    ++size;
    if (size < k) {
        // 说明还没有回归到倒数第k个
        return 0;
    } else if (size == k) {
        // 到达倒数第k个，递归调用阶段的val值确定好了
        return head->val;
    } else {
        // 越过倒数第k个，即返回之前的val即可
        return val;
    }
}
```

## 例二：删除中间结点的另一种方法

```c++
// 要求：只能访问要删除的结点
/* node定义如例一 */
int deleteNode(node* mid)
{
    // 狸猫换太子
    // 把该结点的数据由后面的结点的数据覆盖，并修改其结构
    mid->val = mid->next->val;
    node* ptr = mid->next;
    mid->next = ptr->next;
    delete ptr;
}
```

