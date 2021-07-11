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



惨痛而又典型的我的错误



### 三、新知识

##### 1、分治RMQ

##### 2、单调栈

