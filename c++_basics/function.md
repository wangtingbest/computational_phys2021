##函数 
**问题**：如何定义并输出一个2\*2，3\*3，4\*4的矩阵？
```c++
#include <iostream>
using namespace std;
int main(){
    double I2[3][3],I3[3][3],I4[3][3];
    //2*2
    for (int i=0;i<2;i++){
        for (int j=0;j<2;j++){
            i==j ? I2[i][j]=1 : I2[i][j]=0; //I_ij=delta_ij
            cout<<"I2["<<i<<"]["<<j<<"]="<<I2[i][j]<<endl;
        }
    }       
    //3*3
    for (int i=0;i<3;i++){
        for (int j=0;j<3;j++){
            i==j ? I3[i][j]=1 : I3[i][j]=0; //I_ij=delta_ij
            cout<<"I3["<<i<<"]["<<j<<"]="<<I3[i][j]<<endl;
        }
    }       
    //4*4
    for (int i=0;i<4;i++){
        for (int j=0;j<4;j++){
            i==j ? I4[i][j]=1 : I4[i][j]=0; //I_ij=delta_ij
            cout<<"I4["<<i<<"]["<<j<<"]="<<I4[i][j]<<endl;
        }
    }
}
```
问题：**代码大量重复，可读性差，不易维护，代码难以重用。**

解决方案：用模块化方法即函数方法定义代码
```c++
//identity_matrix_func.cpp
#include <iostream>
using namespace std;

void identity_matrix(int N){
    double I[N][N];
    for (int i=0;i<N;i++){
        for (int j=0;j<N;j++){
            i==j ? I[i][j]=1 : I[i][j]=0; //I_ij=delta_ij
            cout<<"I2["<<i<<"]["<<j<<"]="<<I[i][j]<<endl;
        }
    }       
}

int main(){
    identity_matrix(2);
    identity_matrix(3);
    identity_matrix(4); 
}
```
#### 函数的定义
```c++
函数返回类型 函数名 （形参1，形参2，...,）{
    函数体；
}
```
如上述函数
  ```c++
  * 函数返回类型 void,void 表示没有返回类型，即不返回任何值
  * 函数名 identity_matrix
  * 只有一个整数型形参 int N 
  ```
  ```c++
  例：定义一个求两个数中的最大值
  double max(double x,double y){
      if (x>y) 
          return x;
      else 
          return y;
  }
  ```




#### 函数的调用
```c++
函数名 （实参1，实参参2，...,);

如 
identity_matrix(3)；
max(3.2, 5);
```

#### 函数的声明
* 对于变量，**先定义，后访问**；
如
int i;
i=1;
* 对函数， **先定义，再调用**。
为了能在定义之前调用函数。需要先声明函数。
则函数可以 **先声明，再调用**。（定义可放在调用之后）

声明的作用是声明函数的存在。
```c++
函数返回类型 函数名 （形参1，形参2，...,);
```
```c++
#include <iostream>
using namespace std;

//声明函数 max
double max(double x,double y)；

int main(){
    double MX;
    MX=max(-13.2,-6);
    cout<<"MX="<<MX<<endl;

}

double max(double x,double y){
      if (x>y) 
          return x;
      else 
          return y;
  }
若注释掉声明部分，编译报错：
error: no matching function for call to 'max'
即找不到函数
```

### 变量的作用域 
* 局域变量 作用域是块作用域，只能相应的被块内的语句访问；
* 全局变量 作用域是整个源程序，可能被任何块语句访问。

```c++  
#include <iostream>
using namespace std;

double x=3.14;//全局变量
//int x=3; //会报错，因为同一作用域变量不能重名。

void log();  //声明函数 log

int main(){
    cout<<"x="<<x<<endl; //打印全局变量x的值
    x=5; //全局变量赋值为5
    cout<<"x="<<x<<endl; 
    double x=666;   //定义局部变量
    cout<<"x="<<x<<endl;
    log();//调用log函数

}

void log(){
    int x=0;
    cout<<"x="<<x<<endl;
}
```
注：
* 同一作用域的变量不能重名。
* 不同作用域的变量可以重名，重名变量以局部优先原则访问。

**全局变量可以先声明，后定义**
```c++  
#include <iostream>
using namespace std;

extern double x;//全局变量

void log();  //声明函数 log

int main(){
    cout<<"x="<<x<<endl; //打印全局变量x的值
    x=5; //全局变量赋值
}
double x=3.14;
```



### 函数间参数传递的三种方式
#### 1. 值传递
* 将主调函数中的实参的数值传递给被调函数的形参。

特点
* 只将主调函数的实参值传给被调函数，传递是单向的。被调函数的形参不会影响主调函数的实参值。
  
例：
```c++
#include <iostream>
using namespace std;

void int f(int i){
    i=i*2;
}
int main(){
    i=3;
    cout<<"i="<<i<<endl;
    f(i);
    cout<<"i="<<i<<endl;
}
//输出结果 
//i=3
//i=3
```
#### 2.引用传递
实际中，我们会遇到希望被调函数改变主调函数中实参值的情况，
如：
交换值函数，要求，输入两个变量，交换所对应的值。
```c++
#include <iostream>
using namespace std;

void swap (double a, double b);
int main(){
    double a=1,b=2;
    cout<<"before exchange:"<<endl;
    cout<<"a="<<a<<"\tb="<<b<<endl;
    swap(a,b);
    cout<<"after exchange:"<<endl;
    cout<<"a="<<a<<"\tb="<<b<<endl;
}
void swap (double a, double b){
    double tmp;
    tmp=a;
    a=b;
    b=tmp;
}
```
传值方法不能改变主调变量中的值！
解决方法：
用引用！

```c++
#include <iostream>
using namespace std;

void swap (double &a, double &b);
int main(){
    double a=1,b=2;
    cout<<"before exchange:"<<endl;
    cout<<"a="<<a<<"\tb="<<b<<endl;
    swap(a,b);
    cout<<"after exchange:"<<endl;
    cout<<"a="<<a<<"\tb="<<b<<endl;
}
void swap (double &a, double &b){
    double tmp;
    tmp=a;
    a=b;
    b=tmp;
}
```
#### 3.指针传递
```c++
#include <iostream>
using namespace std;

void swap (double *a, double *b);
int main(){
    double a=1,b=2;
    cout<<"before exchange:"<<endl;
    cout<<"a="<<a<<"\tb="<<b<<endl;
    swap(a,b);
    cout<<"after exchange:"<<endl;
    cout<<"a="<<a<<"\tb="<<b<<endl;
}
void swap (double *a, double *b){
    double tmp;
    tmp=*a;
    *a=*b;
    *b=tmp;
}
```
### 函数间传递数组
#### 传递1维数组
两种方式：
1. 传整个1维数组的形参
```c++
函数返回类型 函数名（数组名[],数组长度）
例  求含N个数的数组最大元素
#include<iostream>
using namespace std;
double Max(int a[],int N){
   int max=a[0];
   for (int i=1;i<N;i++)
       if (a[i]>max) max=a[i];
    return max;
}
int main(){
    const int N=5;
    int a[N]={1,2,-12};
    cout<<Max(a,N)<<endl;
}
```
