# Algorithm
算法总结

链表
- 链表是内存不连续,单向链表头结点记录基地，尾部next指向null
![image-20210804191853470](https://github.com/zuoming12135/Algorithm/blob/main/Snip20211029_9.png)
- 链表常见算法题
#### 1.反转链表

遍历链表，先存储前一个结点和保存下一个结点，将当前结点的next指针指向前一个结点。

```c
struct ListNode* reverseList(struct ListNode* head){
    struct ListNode *prev = NULL; 
    struct ListNode *curr = head; // 定义curr指向当前列表的头结点
    while (curr) {
        struct ListNode *next = curr -> next; // 保留下一个结点
        curr -> next = prev; // 当前结点指向前一个结点
        prev = curr; // curr 指向前一个结点
        curr = next; // curr指针指向下一个结点
    }
    return prev; // 返回上一个结点
}

-----
递归
struct ListNode* reverseList(struct ListNode* head){
   if (head == NULL || head-> next == NULL) {
       return head;
   }
   struct ListNode *newnode = reverseList(head-> next);

	head->next->next = head; // 下个结点的next指针指向自己
   head ->next = NULL; // n1的next指针指向null，防止产生环
    return newnode;
}
```



#### 2.环形链表球环的入口

分解为2个子问题1.链表是否有环2.有环的话求入口

```c
struct ListNode * detectCycle(struct ListNode *head){
struct ListNode *fast =head;
struct ListNode *slow = head;
while(fast!=NULL){
slow = slow -> next;
if (fast->next == NULL){
	return NULL;
}
fast = fast-> next ->next;
if (fast == slow){
// 有环
	struct ListNode *prt = head;
	while (slow != prt)  {
	// slow指针和head指针必定在环的入口相遇
	slow = slow -> next;
	prt = prt -> next;
	}
	return prt;
}
}
return NULL;
}
```

3.链表的中间值

快慢指针，快指针2步，慢指针1步，快指针到了链表尾部，慢指针就在中间

```
struct ListNode* middleNode(struct ListNode* head){
    struct ListNode *slow = head;
    struct ListNode *fast = head;
    while(fast != NULL && fast -> next != NULL) {
        fast = fast-> next ->next;
        slow = slow -> next;
    }
return slow;
}
```

4.合并2个有序链表

因为2个链表都是有序的，可以从两个链表开头一个一个连接到新链表上，当一个链表链完之后，直接把另一个链表链到后面就行了

```
struct ListNode* mergeTwoLists(struct ListNode* l1, struct ListNode* l2){
  struct ListNode *tmp = (struct ListNode *)malloc(sizeof(struct ListNode)); // 创建一个新链表的移动指针
  tmp -> next = NULL; // 头结点指向NULL
  struct ListNode *p = tmp; // p就是相当于新链表的头指针
  while (l1 && l2) {
      if (l1 -> val < l2 -> val) { // 如果l1的头结点值比较小
          tmp -> next = l1; // 移动到l1上
          l1 = l1 -> next; // l1想下移动一个结点
          tmp = tmp -> next; // tmp向下移动一个结点
      } else {
          tmp -> next = l2;
          l2 = l2 -> next;
          tmp = tmp -> next;
      }
  }
  tmp -> next = l1 ? l1:l2; // 判断tmp 的下一个结点指向哪个链表
  return p-> next; // 返回头结点
}
```

5.删除链表倒数第 n 个结点

思想：双指针法

First,sec 2个指针，first比sec超前n个结点，2个指针同步走遍历链表，当fir遍历到结点末尾时，sec正好处于倒数第n个结点，sec->next = sec->next->next即可删除指针。

利用哨兵对象可以减少边界情况处理

```
struct ListNode* removeNthFromEnd(struct ListNode* head, int n) {
    struct ListNode* dummy = malloc(sizeof(struct ListNode));
    dummy->val = 0, dummy->next = head;
    struct ListNode* first = head;
    struct ListNode* second = dummy;
    for (int i = 0; i < n; ++i) {
        first = first->next;
    }
    while (first) {
        first = first->next;
        second = second->next;
    }
    second->next = second->next->next;
    struct ListNode* ans = dummy->next;
    free(dummy);
    return ans;
}
```



