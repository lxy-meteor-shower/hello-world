# Team_Queue

第一：要学会变通，其实这个和第一次做的那个差不多，什么要随机访问，什么就做下标，随机访问的就是数组了。**记住了！！！**

第二：题解的方法，是为整体开一个队伍的队列，为每一个队伍开一个成员的队列，当出队时，确定了队伍，就可以只从这个队伍出，直到全部出完，换下一支队伍。抓问题的主要矛盾，一个是团队之间谁先出，一个是哪个团队先，如果那个团队出完了，整体踢出队列，之后只能从后面排队。



我还是用了数组链表，虽然说很曲折，但是好歹还是成功了

```c
#include<stdio.h>
#include<string.h>
int member[1000000][2],team[1001];
int main()
{
    int k,i,j,n,m,head,tail,num=0,t;
    char a[10];
    scanf("%d",&k);
    while(k){
        num++;
        t=0;
        printf("Scenario #%d\n",num);
        head=tail=-1;
        for(i=0;i<=k;i++) team[i]=-1;
        for(i=1;i<=k;i++){
            scanf("%d",&n);
            for(j=0;j<n;j++){
                scanf("%d",&m);
                member[m][0]=i;
            }
        }
        scanf("%s",a);
        while(strcmp(a,"STOP")){
            if(!strcmp(a,"ENQUEUE")){
                scanf("%d",&m);
                if(t==0) head=tail=m;
                t++;
                if(team[member[m][0]]==-1||team[member[m][0]]==tail){
                    member[tail][1]=m;
                    tail=m;
                    team[member[m][0]]=m;
                }
                else{
                    member[m][1]=member[team[member[m][0]]][1];
                    member[team[member[m][0]]][1]=m;
                    team[member[m][0]]=m;
                }
            }
            else if(!strcmp(a,"DEQUEUE")){
                printf("%d\n",head);
                t--;
                if(head==team[member[head][0]])
                    team[member[head][0]]=-1;
                head=member[head][1];
            }
            scanf("%s",a);
        }
        scanf("%d",&k);
        printf("\n");
    }
}
```

