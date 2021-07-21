# Postfix_expression

第一眼，纠结，还是没忍住看了题解，（毕竟高精度不是那么好写）

然后开始，还是一些小错误：
1.优先级问题，先算等号后边，再算等号前边
2.瞎用函数，数组忘记清零(用库函数注意点)

代码：

```c
#include<stdio.h>
int atoi(char a[],int i)
{
    int j,x=0;
    for(j=0;j<i;j++){
        x=x*10+a[j]-'0';
    }
    return x;
}
int main()
{
    int stack[1000],top=0,i=0;
    char x,a[10];
    while((x=getchar())!='@'){
        if(x=='+') stack[top-1]=stack[top-2]+stack[--top];
        else if(x=='-') stack[top-1]=stack[top-2]-stack[--top];
        else if(x=='*') stack[top-1]=stack[top-2]*stack[--top];
        else if(x=='/') stack[top-1]=stack[top-2]/stack[--top];
        else if(x=='.'){
            stack[top]=atoi(a,i);
            i=0;
            top++;
        }
        else a[i++]=x;
    }
    printf("%d",stack[0]);
    return 0;
}
```

