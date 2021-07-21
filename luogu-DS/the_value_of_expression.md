# the_value_of_expression

可以说也是坑惨了

至今没有弄明白

EOF结束和’\n‘结束为什么结果不一样，暂且搁置吧

代码：

```c
#include<stdio.h>
int main()
{
    int stack[100001],top=1,x=0,i;
    char c;
    freopen("P1981_2.in", "r", stdin); 
    scanf("%d",stack);
    stack[0]%=10000;
    while((c=getchar())!=EOF){
        scanf("%d",stack+top);
        *(stack+top)%=10000;
        if(c=='*') stack[top-1]=(stack[top]%10000)*(stack[top-1]%10000)%10000;
        else top++;
    }
    for(i=0;i<top;i++) x+=stack[i];
    x%=10000;
    printf("%d",x);
    return 0;
}
```

