# cow

### 一、失败的教训

第一次提交失败了，回忆起寒假也是这样，用我使劲想出来的方法去模拟，最后只能实现部分测试集，有些问题有些方面没有想到，感觉不是很肯定的思路还是应该先搁置，并不是不鼓励思考自己的想法，而是真的不够全面，容易出错，要养成一些算法思维，像分治啥的，在解决编程问题时这些想法才应该是自然的，只是靠脑子去模拟不太行得通。



比如：如果采用题解所说的分治，情况很简单，一种两种三种，很显然。但是我采用分块再merge的方式，事实上两块之间的关系跟中间可能联系不大，不一定需要它作为中介，这种方法缺少一种全局观，本题显然是要考虑全局的，以部分代替全部的关系，让思路产生了错误，寒假也总是这个问题，得要改改这个坏毛病。

### 二、失败的代码

```c
#include<stdio.h>

int cow[100000][2]={0};

int main(){
    int N,i,left=0,max,mid;
    scanf("%d",&N);
    scanf("%d",cow[0]);
    for(i=1;i<N;i++){
        scanf("%d",cow[i]);
        if(cow[i][0]<=cow[i-1][0]){
            cow[left][1]=i-1;
            cow[i-1][1]=left;
            left=i;
        }
    }
    cow[N-1][1]=left;
    cow[left][1]=N-1;
    left=cow[0][1]+1;
    while(left<N){
        while(left!=0)
        {
            if(cow[cow[left-1][1]][0]<cow[left][0]&&cow[cow[left][1]][0]>cow[left-1][0]){
                cow[cow[left-1][1]][1]=cow[left][1];
                cow[cow[left][1]][1]=cow[left-1][1];
                cow[left][1]=0;
                left=cow[left-1][1];
                cow[cow[left][1]][1]=0;
            }
            else break;
        }
        left=cow[left][1]+1;
    }
    max=0;
    for(left=0;left<N;left=cow[left][1]+1){
        mid=cow[left][1]-left+1;
        if(mid>max&&mid!=1) max=mid;
    }
    printf("%d",max);
    return 0;
}
```



惨痛而又典型的我的错。



### 三、新知识

#### 1、分治+RMQ

首先给概念：

**RMQ** (Range Minimum/Maximum Query)：对于长度为n的数组A，回答若干询问RMQ(A,i,j)(i,j<=n-1)，返回数组A中下标在i,j范围内的最小（大）值，也就是说，RMQ问题是指求区间最值的问题。



关于这个问题的求解又引入了ST（Sparse Table）算法，要基于动态规划，暂时先放一放

啊，本来想偷个懒，把ST放一放，但是遍历超时了，没办法，但还是想放一放，晚点改了吧。



先放上超时的代码：

```c
#include<stdio.h>

int cow[100000];

int Cow(int down,int up)
{
    int i,max=down,min=down,a=0,x;
    if(down>=up) return 0;
    for(i=down+1;i<=up;i++){
        if(cow[i]<=cow[min]) min=i;
        if(cow[i]>cow[max]) max=i;
    }
    if(max<=min){
        x=Cow(down,max);
        if(x>a) a=x;
        x=Cow(min,up);
        if(x>a) a=x;
        x=Cow(max+1,min-1);
        if(x>a) a=x;
    }
    else{
        x=max-min+1;
        if(x>a) a=x;
        x=Cow(max+1,up);
        if(x>a) a=x;
        x=Cow(down,min-1);
        if(x>a) a=x;
    }
    return a;
}

int main()
{
    int i,N;
    scanf("%d",&N);
    for(i=0;i<N;i++) scanf("%d",cow+i);
    printf("%d",Cow(0,N-1));
    return 0;
}
```



#### 2、单调栈

**单调栈的概念**：栈中元素单调递增或单调递减。



**实现方法**：元素依次进栈，在进栈是如果自己进去的时候破坏了队形，那就把别人踢出去，直到自己进去的时候队形是OK的。



**单调栈的性质**：
1.单调递增栈：进栈元素可以知道左边第一个比自己小的元素。元素依次进栈，就所有元素都可以知道。出栈元素可以知道右边第一个比自己小的元素。
2.单调递减栈：进栈元素可以知道左边第一个比自己大的元素。元素依次进栈，就所有元素都可以知道。出栈元素可以知道右边第一个比自己大的元素。



**结果的直观体现：**
1.单调递增栈：        ~~12 3 43 23 45~~ **1** ~~65 67 34 65 34~~ **21** **54**

2.单调递减栈：        ~~12 3 43 23 45 1 65~~ **67** ~~34~~ **65** ~~34 21~~ **54**

结果出现方式：找到所有的当中最小（大）的那个，再在右边找最小（大）的，直到找到最后一个数



**关于本题：**

傻瓜错误说一句，毕竟不是一回两回了：up用大的，down等于0。

分析了半天，到底还是没有理解单调栈的应用吧。

本题此方法采用定一找一，定后面的找前面的，到底还是入栈的多，出栈自己加的。用到性质左边第一个比自己大的元素，找到左界，再用结果的性质栈中元素的右边一定没有比它小（大）的元素，因为它就是从右边一堆选出来的最小（大）的，两个栈配合使用，实现结果。

粘上代码：

```c
#include<stdio.h>

int upstack[100000],downstack[100000],cow[100000];
int uptop=0,downtop=0;
int search(int);

int main()
{
    int N,i,x,max=0;
    scanf("%d",&N);
    for(i=0;i<N;i++){
        scanf("%d",cow+i);
        downtop--,uptop--;
        while(downtop>=0&&cow[downstack[downtop]]<cow[i]) downtop--;
        downstack[downtop+1]=i;
        downtop+=2;
        while(uptop>=0&&cow[upstack[uptop]]>=cow[i]) uptop--;
        upstack[uptop+1]=i;
        uptop+=2;
        if(downtop==1) x=upstack[0];
        else x=search(downstack[downtop-2]);
        if(x==i) x=0;
        else x=i-x+1;
        max=(x>max?x:max);
    } 
    printf("%d",max);
    return 0;
}

int search(int x)
{
    int top,down,mid;
    top=uptop-1,down=0;
    while(top>=down){
        mid=(down+top)/2;
        if(upstack[mid]<x) down=mid+1;
        else top=mid-1;
    }
    return upstack[down];
}
```

