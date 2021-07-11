# Queue Arrangement

### 数据结构

​		这次用到了数组链表，也是在应用层面，深刻地理解到了这个数据结构的优点，因为**本题的数据是1~N的序号，用数组存储是很好的，既可以根据名字随机访问，而且每个位置都能用到，也不算浪费空间**（不像有些序号是随机给的，可以很大可以很小，两个数据1，10000，你也开一个很大数组）。用单链表data域要一个个比较，虽说插入删除方便了，但是比较太麻烦了，每一次都要遍历搜索。

其他都是一些小细节了。

### 代码

不废话，直接上，找到合适的数据结构就很简单了。

```c
#include<stdio.h>

int queue[100001][2];

int main()
{
    queue[1][0]=1;
    queue[1][1]=1;
    int N,i,x,flag,M,Head=1;
    scanf("%d",&N);
    for(i=2;i<=N;i++){
        scanf("%d%d",&x,&flag);
        if(flag==0){
            if(x==Head) Head=i;
            queue[i][0]=queue[x][0];
            queue[i][1]=x;
            queue[queue[x][0]][1]=i;
            queue[x][0]=i;
        }
        else{
            queue[i][1]=queue[x][1];
            queue[i][0]=x;
            queue[queue[x][1]][0]=i;
            queue[x][1]=i;
        }
    }
    scanf("%d",&M);
    for(i=0;i<M;i++){
        scanf("%d",&x);
        if(queue[x][0]==0&&queue[x][1]==0) continue;
        if(x==Head) Head=queue[x][1];
        queue[queue[x][0]][1]=queue[x][1];
        queue[queue[x][1]][0]=queue[x][0];
        queue[x][0]=0;
        queue[x][1]=0;
    }
    x=Head;
    while(queue[x][1]!=Head){
        printf("%d ",x);
        x=queue[x][1];
    }
    printf("%d\n",x);
    return 0;
}
```

