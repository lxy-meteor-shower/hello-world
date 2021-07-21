# Fill_Color

首先：找规律发现只要和边上有挨着，就说明在圈外

然后：写的时候有个问题，可能本来还不能说它是圈外的，后来才变的，导致判断失效，得另想办法

失败代码：

```c
#include<stdio.h>
int main()
{
    int i,n,square[30][30],j;
    scanf("%d",&n);
    for(i=0;i<n;i++)
        for(j=0;j<n;j++)
            scanf("%d",square[i]+j);
    for(i=0;i<n;i++){
        if(!square[0][i]) square[0][i]=-1;
        if(!square[i][0]) square[i][0]=-1;
        if(!square[n-1][i]) square[n-1][i]=-1;
        if(!square[i][n-1]) square[i][n-1]=-1;
    }
    for(i=1;i<n-1;i++)//mark
        for(j=1;j<n-1;j++){
            if(square[i][j]==1) continue;
            if(square[i][j-1]==-1||square[i][j+1]==-1||square[i-1][j]==-1||square[i+1][j]==-1)
                square[i][j]=-1;
        }
    for(i=0;i<n;i++){
        for(j=0;j<n;j++){
            if(square[i][j]==-1) printf("0 ");
            else if(square[i][j]==0) printf("2 ");
            else printf("1 ");
        }
        printf("\n");
    }
    return 0;
}
```

最简单的改进方法，就是在mark处多循坏几次，时间好像没怎么增加，那就不管了

如果复杂一点：应该就是深搜和广搜吧，这个可能要用到栈和队列，需要试试怎么应用，可是已经找到了偷懒的方法，也不想再看了，看以后有机会吗
