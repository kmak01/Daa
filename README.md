#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
typedef struct node
{
    int data;
    struct node* next;
}node;
node *adj[100];
node *create(int x)
{
    node *head=(node *)malloc(sizeof(node));
    head->data=x;
    head->next=NULL;
    return head;
}
void add(int src,int dest)
{
    if(!adj[src])
    {
        adj[src]=create(dest);
        return;
    }
    node *t=adj[src];
    while(t->next)
    t=t->next;

    t->next=create(dest);
}
bool safe(int *color,int x,int index)
{
    node *t=adj[x];
    while(t)
    {
        if(color[t->data]==index)
        return false;
        t=t->next;
    }
    return true;
}
bool cancolor(int m,int x,int *color,int vertex)
{
    if(x==vertex+1)
    return true;

    for(int i=1;i<=m;i++)
    {
        if(safe(color,x,i))
        {
            color[x]=i;
            if(cancolor(m,x+1,color,vertex))
            return true;
            color[x]=-1;
        }
    }
    return false;
}
int main()
{
    int edges,vertex;
    printf("Enter no of vertices : ");
    scanf("%d",&vertex);
    for(int i=0;i<=vertex;i++)
    adj[i]=NULL;
    printf("Enter no of egdes : ");
    scanf("%d",&edges);

    printf("Enter edges : \n");
    for(int i=0;i<edges;i++)
    {
        int src,dest;
        scanf("%d%d",&src,&dest);
        add(src,dest);
        add(dest,src);
    }

    printf("Enter no of color : "); int m;
    scanf("%d",&m);

    int color[vertex+1];
    for(int i=1;i<=vertex;i++)
    color[i]=-1;

    if(cancolor(m,1,color,vertex))
    {
        for(int i=1;i<=vertex;i++)
        {
            printf("Node %d -> color %d\n",i,color[i]);
        }
    }
    else
    printf("Coloring not possible\n");
    return 0;
}
