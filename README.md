#SIMD系列教程
本人准备把这个写成一个教程。不过我自己也在学习的过程中，大家就这么看看吧。
写这个的主要原因，是我自己在很久之前看到一篇讲使用SIMD技术优化C++程序的文章。于是便想学习。但是网上成系统的资源太少。也算是回馈社会吧。
阅读本教程需要一点点c++的基础知识，一点点的汇编知识。基本上是无基础就可以的。

##Why SIMD?
计算机，顾名思义就是用来做计算的。这里举个非常简单的例子。
```c++
vector<double> testAdd(vector<double> &x,vector<double> &y)
{
    size_t length=x.size();
    vector<double> z(length,0);
    for(size_t i=0;i<length;i++)
    {
        z[i]=x[i]+y[i];
    }
    return z;
    
}
```
这里我们定义了一个testAdd，测试用的函数。这个函数的用处就是输入两个矢量，将它们相加，然后将结果输出。假如X、Y的长度都是N，那么要经过N次加法，才能得到最后的结果。如果N很长的话，那么消耗的时间就会很长。

如何将计算这个函数的时间缩短呢？当然，我自己最直接的想法是并行化。并行化也很适合用在这个情况下。但是呢，that is a story for another day。

在这个时候还有另外一条路，那就是矢量化（vectorization）。这个时候需要一种叫SIMD（Single instruction, multiple data ），即多个数据用一条指令执行。

##何为SIMD
![Image of SIMD](https://github.com/fkfk000/SIMD-tutorial/blob/master/SIMD2.svg.png)




先看一个简单的例子：
```c++
#include <iostream>
//#include <immintrin.h>
#include <x86intrin.h>
using namespace std;
int main()
{
    __m256d t;
    double x[]={12.2,3.441,1234.53,13.0};
    t=_mm256_load_pd(x);
    t=_mm256_sqrt_pd(t);
    _mm256_store_pd(x, t);
    for(int i=0;i<=3;i++)
    {
        cout<<x[i]<<endl;
    }
    return 0;
}
```
要是用SIMD技术，首先需要载入头文件。`#include <x86intrin.h>`在这里我取了个巧，直接include了x86intrin.h这个头文件。
