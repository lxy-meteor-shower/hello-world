# log_analyse

第一次：

```c
#include<stdio.h>
int max[200000];
int main()
{
    int N,i,num1=0,num2=0,flag,shu;
    max[num1]=0;
    scanf("%d",&N);
    for(i=0;i<N;i++){
        scanf("%d",&flag);
        switch(flag){
        case 0:
            scanf("%d",&shu);
            if(shu>max[num1]) max[num1+1]=shu;
            else max[num1+1]=max[num1];
            num1++;
            num2++;
            break;
        case 1:
            if(num2!=0) num2--;
            break;
        case 2:
            printf("%d\n",max[num2]);
            break;
        }
    }
}
```



第二次：

```c
#include<stdio.h>
int max[200000];
int main()
{
    int N,i,num=0,flag,shu;
    max[num]=0;
    scanf("%d",&N);
    for(i=0;i<N;i++){
        scanf("%d",&flag);
        switch(flag){
        case 0:
            scanf("%d",&shu);
            if(shu>max[num]) max[num+1]=shu;
            else max[num+1]=max[num];
            num++;
            break;
        case 1:
            if(num!=0) num--;
            break;
        case 2:
            printf("%d\n",max[num]);
            break;
        }
    }
}
```



这种题目的一个特点就是：

进栈一次出栈一次，出去了就不会在进来了，这算是栈的特点了，所以可以实现覆盖，所以保存每次栈中的从开始到后来的最大值就好了。