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

