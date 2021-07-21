# Monotone_queue

### 新知识

#### 单调队列：

1.应用：在移动窗口找到最大（小）值

2.使用方法：单调递增（减）的队列，如果进去的时候，破坏了单调性的从后面出，被窗口排出来的从前面出队，最后进入，此时队列第一个即是窗口最小的元素。

3.特点：队列为单向进双向出

```c
#include<stdio.h>
int queue[1000000],store[1000000],head=0,tail=0;
int main()
{
    int n,k,i;
    scanf("%d%d",&n,&k);
    for(i=0;i<n;i++) scanf("%d",store+i);
    for(i=0;i<k;i++){
        while(head!=tail&&store[queue[tail-1]]>=store[i]) tail--;
        queue[tail++]=i;
    }
    printf("%d ",store[queue[head]]);
    for(;i<n;i++){
        if(queue[head]==i-k) head++;
        while(head!=tail&&store[queue[tail-1]]>=store[i]) tail--;
        queue[tail++]=i;
        printf("%d ",store[queue[head]]);
    }
    printf("\n");
    head=tail=0;
    for(i=0;i<k;i++){
        while(head!=tail&&store[queue[tail-1]]<=store[i]) tail--;
        queue[tail++]=i;
    }
    printf("%d ",store[queue[head]]);
    for(;i<n;i++){
        if(queue[head]==i-k) head++;
        while(head!=tail&&store[queue[tail-1]]<=store[i]) tail--;
        queue[tail++]=i;
        printf("%d ",store[queue[head]]);
    }
    return 0;
}
```

