---
title: 函数指针和指针函数
date: 2016-04-29 18:34:12
categories: Basic
tags: C
---

Unix C语言环境下的多线程编程使用到函数指针来传递需要执行的函数体到线程，使用的方式就是通过函数指针来指向需要执行的代码。
函数原型：
```
int pthread_create(pthread_t *thread, pthread_attr_t *attr, void *(*func)(void*), void *arg);
```
<!-- more -->
* 参数：
Thread 指向pthread_t类型变量的指针
attr         指向pthread_attr_t类型变量的指针，或者为NULL
func        指向新线程所运行函数的指针
arg          传递给func的参数
* 返回值：
0            成功返回
errcode    错误

我们可以使用函数pthread_join等待某进程结束。
函数原型：
```
int pthread_join(pthread_t thread,void ** retval);
```
* 参数：
thread         所等待的进程
retval          指向某存储线程返回值的变量
* 返回值： 
0                成功返回
errorcode    错误
以上两个函数都包含在头文件pthread.h中。

# 多线程demo
``` bash
/* Mhello1.c
   Hello,world -- Multile Thread
*/
#include
#include
#define NUM 6
int main()
{
    void print_msg(void*);
   
    pthread_t t1,t2;
    pthread_create(&t1,NULL,print_msg,(void *)"hello,");
    pthread_create(&t2,NULL,print_msg,(void *)"world!\n");
   
    pthread_join(t1,NULL);
    pthread_join(t2,NULL);  
}
void print_msg(void* m)
{
    char *cp=(char*)m;
    int i;
    for(i=0;i
    {
        printf("%s",m);
        fflush(stdout);
        sleep(1);
    }
}
运行结果：
$ gcc mhello1.c -o mhello1.exe
$ ./mhello1.exe
hello,world!
hello,world!
hello,world!
hello,world!
hello,world!
hello,world!
```

# 函数指针demo
``` bash
//例2 函数指针示例
#include <iostream>
using namespace std;
//函数声明
double triangle_area(double &x,double &y);//三角形面积
double rectangle_area(double &x,double &y);//矩形面积
double swap_value(double &x,double &y);//交换值
double set_value(double &x,double &y);//设定长宽（高）
// double print_area(double &x,double &y);//输出面积
double print_area(double(*p)(double&,double&), double &x,double &y);//利用函数指针输出面积
//函数定义
double triangle_area(double &x,double &y)
{
    cout<<"三角形的面积为：\t"<<x*y*0.5<<endl;
    return 0.0;
}
 
double rectangle_area(double &x,double &y)
{
    cout<<"矩形的面积为：\t"<<x*y<<endl;
    return 0.0;
}
 
double swap_value(double &x,double &y)
{
    double temp;
    temp=x;
    x=y;
    y=temp;
    return 0.0;
}
 
double print_area(double(*p)(double &x,double &y), double &x,double &y)
{
    cout<<"执行函数前:\n";
    cout<<"x="<<x<<"  y="<<y<<endl;
    //it is coming!...
    p(x,y);
    cout<<"函数指针传值后:\n";
    cout<<"x="<<x<<"  y="<<y<<endl;
    return 0.0;
}
 
double set_value(double &x,double &y)
//注意参数一定要定义成引用，要不是传不出去哈！
{
    cout<<"自定义长宽（高）为：\n";
    cout<<"长为：";
    cin>>x;
    cout<<"宽或者高为：";
    cin>>y;
    return 0.0;
}
 
int main()
{
    bool quit=false;//初始化退出的值为否
    double a=2,b=3;//初始化两个参数a和b
    char choice;
    //声明的p为一个函数指针，它所指向的函数带有梁个double类型的参数并且返回double
    double (*p)(double &,double &);
    while(quit==false)
    {
        cout<<"退出(q); 设定长、宽或高(1); 三角形面积(2); 矩形面积(3); 交换长宽或高(4)."<<endl;
        cin>>choice;
        switch(choice)
        {
        case 'q':
            quit=true;
            break;
        case '1':
            p=set_value;
            print_area(p,a,b);
            break;
        case '2':
            p=triangle_area;
            print_area(p,a,b);         
            break;
        case  '3':
            p=rectangle_area;
            print_area(p,a,b);
            break;
        case '4':
            p=swap_value;
            print_area(p,a,b);
            break;
        default:
            cout<<"请按规矩出牌！"<<endl;
        }
    }
    return 0;
}
```
# 简化的定义
如果你认为上面所诉的函数指针的声明格式有点罗嗦，那么我们也可以利用typedef来简化声明和定义的操作。比如说在上例2的第61行，那么长一串。我们完全可以在在程序一开始利用typedef来代替：

```
typedef double (*vp)(double &,double &);
```
这样一来，我们就可以把程序的第61行简化成：
```
vp p;
```
而且，我们在声明和定义print_area()函数的时候，就可以程序的第10行和第33行换成：

* 函数声明
```
double print_area(vp,double &x,double &y);
```
* 函数定义
```
double print_area(vp p, double &x,double &y)
```
