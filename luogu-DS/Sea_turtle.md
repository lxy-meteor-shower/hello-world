# Sea_turtle

本题主要是在处理语句遇到了一点问题：最好不要在一个if分支里面乱吞空格或其他，直接通过外层循坏跳过，可以避免错过（在中间吞了，外层循环没捕捉到）

代码：

```c
#include<stdio.h>
int main()
{
    long long stack1[300],stack2[300],n=0,m,top1=0,top2=0;
    char c,a[20];
    while((c=getchar())!='\n'){
        if(c=='F'){
            getchar();
            scanf("%lld",&m);
            n+=m;
        }
        else if(c=='B'){
            getchar();
            scanf("%lld",&m);
            n-=m;
        }
        else if(c=='R'){
            scanf("%s",a);
            scanf("%lld",&m);
            stack1[top1++]=m;
            stack2[top2++]=n;
            n=0;
        }
        else if(c==']')
            n=n*stack1[--top1]+stack2[--top2];
    }
    if(n<0) printf("%d",-n);
    else printf("%d",n);
    return 0;
}
```

