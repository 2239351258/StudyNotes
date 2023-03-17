![image-20230226220817804](https://my-learning-data.oss-cn-hongkong.aliyuncs.com/imgs/image-20230226220817804.png)

#### 题解：

快慢指针，快指针到链表结尾时，慢指针到删除结点的前一个位置。需要注意在删除倒数最后一个（链表首部节点）的处理

```ts
function removeNthFromEnd(head: ListNode | null, n: number): ListNode | null {
    let [fast,slow]=[head,head]
    while(n--){
        fast=fast.next
    }
    while(fast!==null&&fast.next!==null){
        slow=slow.next
        fast=fast.next
    }
    if(fast===null){
        return head.next
    }
    else{
        slow.next=slow.next.next
        return head
    }
};
```

