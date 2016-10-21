#SIMD系列教程
本人准备把这个写成一个教程。不过我自己也在学习的过程中，大家就这么看看吧。
写这个的主要原因，是我自己在很久之前看到一篇讲使用SIMD技术优化C++程序的文章。于是便想学习。但是网上成系统的资源太少。也算是回馈社会吧。
阅读本教程需要一点点c++的基础知识，一点点的汇编知识。基本上是无基础就可以的。

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
