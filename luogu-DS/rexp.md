# rexp

这是一道表达式相关的问题，用栈存数字和运算符是基础，此题最关键的是弄懂几个“运算符”的运算规律：

遇到a：数量加1

遇到（：数量进栈，运算符进栈

遇到|：同上

遇到）：出栈，在（出栈前，运算|并存入

（出栈时：加上（前的数量

得到的结果给数量



要认真分析示例，把所有情况考虑完整



代码：

```c
#include<stdio.h>
int stack1[100000],top1=0,top2=0;
char stack2[100000];
int max(int a,int b)
{
    if(a>b) return a;
    else return b;
}
int main()
{
    int n=0;
    char c;
    while((c=getchar())!='\n'){
        if(c=='a') n++;
        else if(c=='('||c=='|'){
            stack1[top1++]=n;
            n=0;
            stack2[top2++]=c;
        }
        else if(c==')'){
            stack1[top1++]=n;
            while(stack2[--top2]!='('){
                top1--;
                stack1[top1-1]=max(stack1[top1],stack1[top1-1]);
            }
            top1=top1-2;
            n=stack1[top1+1]+stack1[top1];
        }
    }
    stack1[top1++]=n;
    while(top2!=0){
        top1--,top2--;
        stack1[top1-1]=max(stack1[top1],stack1[top1-1]);
    }
    printf("%d",stack1[0]);
    return 0;
}
```

