# c语言巩固
## 一、动态内存分配
### malloc
```c
#include <stdio.h>
#include <stdlib.h>
/*使用动态内存分配要有头文件<stdlib.h>*/
int main(int argc, char *argv[]){
	/*在没有c99之前，给数组的动态内存分布*/
	int num;
	int *a;
	printf("输入数量：");
	scanf("%d",&num);
	a=(int*)malloc(num*sizeof(int)); 
	/*这里是给首地址为a的数组开辟动态内存空间
	malloc是void*类型，a是int*类型——要强制类型转换 
	(int*)是强制类型转换，根据a的类型而定
	sizeof(int)则是表示给里面的元素开辟空间
	a数组里面存储的是int元素——sizeof(int)
	num表示a数组里面存储的数据个数 
	*/ 
	for(i=0;i<num;i++)
	{
		scanf("%d",a[i]);
	}
	free(a);
	/*要记得把空间还掉
    只能还申请来的空间的首地址
    如果是大程序申请了没有free
    长时间运行内存逐渐下降*/
	return 0;
}
```

## 二、枚举
```c
#include<stdio.h>
enum color{red,yellow,green};
/*枚举
 enum 枚举类型名字{名字0，名字1......};
 大括号里面的名字就是常量符号，类型int，值依次从0-n
 c中尽量用字母来代表数字
*/
```
## 三、二维字符串数组【指针数组（查阅ppt）】
```c
char *colorname[3]={"red","yellow","green"};
```
## 四、可变数组【偏数据结构】
```c
typedef struct{
	int *array;
	int size;
}*Array
/*【tips:不要这样定义，此处只是举例子】
这样意味着Array是一个指针
类似于定义int *a代表定义了一个指向整数地址的指针a一样*/
```
## 五、链表
### 初始链表
```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
 
typedef struct _node{
	int value;
	struct _node *next;
}Node;

int main()
{
	Node *head=NULL;
	int number;
	do{
		scanf("%d",&number);
		if(number!=-1)
		{//add to linked-list
			Node *p=(Node*)malloc(sizeof(Node));//为结构体指针p开辟内存空间 
			p->value=number;
			p->next=NULL;
			//find the last
			Node *last=head;
			if(last)//如果last不是NULL 
			{
				while(last->next)//如果last->next指向的地址不是NULL 
				{
					last=last->next;//使last->next最终指向为NULL
				}
				//attach
				last->next=p;
			}
			else//last是NULL的情况 
			{
				head=p;	
			}
		}//上述过程只需要在纸上画几个循环便可以理解！ 
	}while(number!=-1);//如果number=-1跳出循环 
	return 0;

}
```
### 链表函数
```c
//错误做法
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
 
typedef struct _node{
	int value;
	struct _node *next;
}Node;

void add(Node *head,int number);

int main()
{
	Node *head=NULL;
	int number;
	do{
		scanf("%d",&number);
		if(number!=-1)
		{
			add(head,number);
		} 
	}while(number!=-1);//如果number=-1跳出循环 
	return 0;

}

void add(Node *head,int number)
{
	//add to linked-list
	Node *p=(Node*)malloc(sizeof(Node));//为结构体指针p开辟内存空间 
	p->value=number;
	p->next=NULL;
	//find the last
	Node *last=head;
	if(last)//如果last不是NULL 
	{
		while(last->next)//如果last->next指向的地址不是NULL 
		{
			last=last->next; 
		}
		//attach
		last->next=p;
	}
	else//last是NULL的情况 
	{
		head=p;	
		/*※此处对head进行了修改，但是不会影响主函数的head的值
		这个函数传进来的是head的值，而不是head的地址
		可以想象head里面存了一串数字【地址】来理解
		*/
	}
}
```
```c
//正确做法
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
 
typedef struct _node{
	int value;
	struct _node *next;
}Node;

typedef struct _list{
	Node *head;
}List;

void add(List *pList,int number);

int main()
{
	List list;
	int number;
	list.head=NULL;
	do{
		scanf("%d",&number);
		if(number!=-1)
		{
			add(&list,number);//这里传list的指针，相当于传指向Node的指针的指针？※好好理解一下
		} 
	}while(number!=-1);//如果number=-1跳出循环 
	return 0;

}

void add(List *pList,int number)
{
	//add to linked-list
	Node *p=(Node*)malloc(sizeof(Node));//为结构体指针p开辟内存空间 
	p->value=number;
	p->next=NULL;
	//find the last
	Node *last=pList->head;
	if(last)//如果last不是NULL 
	{
		while(last->next)//如果last->next指向的地址不是NULL 
		{
			last=last->next; 
		}
		//attach
		last->next=p;
	}
	else//last是NULL的情况 
	{
		pList->head=p;	
		/*这个函数传进来的是list的地址，在相同地址下，对地址所指向的内容进行修改时可以影响到主函数的那个head的值的*/
	}
}
```
### 打印链表
```c
void print(List *pList)
{
	Node *p;
	for (p=pList->head;p;p=p->next)
	{
		printf("%d\t",p->value);
	}
	printf("\n");
}
```
### tips:使用结构体指针需要面对的问题【边界情况】
```c
p->next//要使用这个语句时，p不能是NULL!小心这个边界情况！！
```
### 链表的查询以及局部删除
```c
	scanf("%d",&number);
	Node *p;
	int isFound=0;
	for(p=list.head;p;p=p->next)
	{
		if(p->value==number)
		{
			printf("找到了");
			isFound=1;
			break;
		}
	}
	if(!isFound)
	{
		printf("没找到");
	}
	/*寻找链表*/
	Node *q;//q指向p的前一个结点
	for(q=NULL,p=list.head;p;q=p,p=p->next)
	{
		if(p->value==number)
		{
			if(q)
			{
				q->next=p->next;
			}
			else
			{
				list.head=p->next;
			}
			free(p);
			break;
		}//画图理解
	}
	/*删除局部链表*/
```
### 链表的清除
```c
for(p=list.head;p;p=q)
{
	q=p->next;
	free(p);
}
```
## 六、不熟悉的c语言函数
- atoi()函数：把字符串转换成整型数的函数。