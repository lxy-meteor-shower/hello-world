# Monotonic_Stack

很直接的单调栈了

运用性质出栈元素可以知道右边第一个比自己大（小）的数

常犯错误：下标和元素值不要混了

直接粘代码：

```c
#include<stdio.h>
int stack[3000000],store[3000001][2],top=0;
int main()
{
    int i,n;
    scanf("%d",&n);
    for(i=1;i<=n;i++){
        scanf("%d",store[i]);
        while(top>0&&store[stack[top-1]][0]<store[i][0]){
            store[stack[top-1]][1]=i;
            top--;
        }
        stack[top++]=i;
    }
    while(top>0){
        store[stack[--top]][1]=0;
    }
    for(i=1;i<n;i++) printf("%d ",store[i][1]);
    printf("%d",store[i][1]);
    return 0;
}
```

