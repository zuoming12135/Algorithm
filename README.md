# Algorithm
算法总结

## 链表
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

## 数组
数组是一种连续的数据结构，用一组连续的内存空间，来存储一组相同类型的数据。具有**<u>根据下标随机访问</u>**的特性，时间复杂度为O(1)，缺点：数据的删除和插入操作比较耗时

疑问1.为什么数组下标从0开始？

因为性能优化方面考虑的，以访问下标为k为例a[0]标识base_address，数组的下标随机访问，a[k]address = base_address + k * data_type_size;，如果下标从1开始，a[k]address = base_address + (k-1) * data_type_size;，会多一次减法指令运算。数组作为一种非常基础的数据结构要做到性能极致优化。

## 栈

栈是一种先入后出的数据结构。从操作上看是一种操作受限的线性表，只允许从一端插入和删除数据。
栈的应用
1.函数调用栈
每进入一个函数就会将临时变量作为一个栈帧入栈，调用函数完成返回之后，将这个函数的栈帧出栈。
2.表达式求和
3.括号匹配中的应用


## 排序
###### 1.冒泡排序
  冒泡排序：比较相邻的元素，如果第一个比第二个大，那么就交换，然后第在比较相邻的一对，重复。直到遍历到最后一对，那么最后一个就是最大的元素。
在重复上面的步骤，遍历到倒数第2个元素。
```
 void bolb_sort(int a[], int n){
    if (n<=1) return;
    for (int i = 0;i<n;i++){
        bool flag = false;// 提前退出冒泡排序的标志
        for (int j = 0;j < n - i - 1;j++){
            if(a[j]>a[j+1]) {
                flag = true;
                int tmp = a[j];
                a[j]=a[j+1];
                a[j+1]=tmp;
            }
        }
        if(flag == false){
            break;
        }

    }
}
```
###### 2.插入排序
  将第一排第一个序列元素看做有序序列，把第二个元素到最后一个元素看做未排序序列。从头到尾依次扫描未排序序列，将扫描到的元素依次插入到合适位置
  ```
void insert_sort (int a[],int len) {
    int i ,j ,key ;
    for (i = 1; i < len ; i ++) {
    	key = a[i];
    	j = i - 1;
    	while (j >= 0 && a[j] > key) {
    	a[j + 1] = a[j];
    	j --;
    	}
    a[j + 1] = key;
    }
    
}
```

###### 2.快速排序
&emsp;快速排序的思想是分而治之，将一个序列分为2个子序列然后递归排序2个子序列。
1.选择一个基准值
2.将小于基准值得移动到左边，大于基准值的移动到右边。
3.递归排序子序列。将小于基准值的子序列和大于小于基准值的子序列递归排序。
```
void quick_sort(int arr[], int left, int right)
{
        int i,j,mid,tmp;
        i=left;
        j = right;
        mid=arr[(i+j)/2];
        while(i<=j) {
            while (i<mid){
                i++; // 找到第一个大于基准值的值
            }
            while (j>mid){
                j--;// 找到第一个小于于基准值的值
            }
            if(i<=j) {
	    // 交换
                tmp = arr[j];
                arr[j]=arr[i];
                arr[i]=tmp;
                i++;
                j--;
            }  
        }
        if(i<right){ // 递归子序列
          quick_sort(arr,i,right);  
        }
        if(j>left){
            quick_sort(arr,j,left);  
        }
}

/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* sortArray(int* nums, int numsSize, int* returnSize){
    quick_sort(nums, 0, numsSize-1);
    *returnSize = numsSize;
    return nums;
}
```

