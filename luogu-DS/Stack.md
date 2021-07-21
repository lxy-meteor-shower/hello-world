# Stack

### 一、超时

本来想偷个懒，直接递归，结果TLE了，意料之中

附上代码

```c
#include<stdio.h>
int n;
int stack(int pushnum,int popnum)
{
    if(pushnum==n) return 1;
    if(popnum==pushnum) return stack(pushnum+1,popnum);
    else return stack(pushnum+1,popnum)+stack(pushnum,popnum+1);
}

int main()
{
    scanf("%d",&n);
    printf("%d",stack(0,0));
    return 0;
}
```



于是联想到斐波拉契，有重复，那就开个数组储存，优化一下吧

优化代码：

```c
#include<stdio.h>
int stack[20]={0};
int stack1(int pushnum,int popnum,int n)
{
    if(pushnum>n) return 0;
    if(pushnum==n) return 1;
    if(popnum==pushnum){
        if(!stack[n-popnum]) stack[n-popnum]=stack1(1,0,n-popnum);;
        return stack[n-popnum];
    }
    else return stack1(pushnum+1,popnum,n)+stack1(pushnum,popnum+1,n);
}

int main()
{
    int n;
    stack[0]=0;
    scanf("%d",&n);
    stack[n]=stack1(1,0,n);
    printf("%d",stack[n]);
    return 0;
}
```



又失败了

优化强度不够大，继续

```c
#include<stdio.h>
int stack[20][20]={0};//前者还需退栈元素，后者为栈中元素
int stack1(int n,int stacknum)
{
    if(stacknum==0){
        if(!stack[n][1]) stack[n][1]=stack1(n,1);
        return stack[n][1];
    }
    if(stacknum>0){
        if(!stack[n][stacknum+1]) stack[n][stacknum+1]=stack1(n,stacknum+1);
        if(!stack[n-1][stacknum-1]) stack[n-1][stacknum-1]=stack1(n-1,stacknum-1);
        return stack[n][stacknum+1]+stack[n-1][stacknum-1];
    }
}

int main()
{
    stack[0][0]=0;
    int n,i;
    scanf("%d",&n);
    for(i=1;i<=n;i++) stack[i][i]=1;
    if(n) stack[n][0]=stack1(n,0);
    printf("%d",stack[n][0]);
    return 0;
}
```

可以说是很感动了

我觉得可能用到了一点差分的思想吧，算净量，就有效实现了递归的规模变化