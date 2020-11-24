# 编译
1. 开始报错cpp版本

然后
```cmake
macro(use_cxx11)
if(CMAKE_VERSION VERSION_LESS "3.1.3")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
else()
    set(CMAKE_CXX_STANDARD 11)
    set(CMAKE_CXX_STANDARD_REQUIRED ON)
endif()
endmacro(use_cxx11)

use_cxx11()
```

2. 又报错
```
In file included from /home/wolfdan/code/cpp_study/section5/client.cpp:10:0:
/home/wolfdan/code/cpp_study/section5/SalesData.hpp:8:23: fatal error: msgpack.hpp: No such file or directory
 #include <msgpack.hpp>
                       ^
compilation terminated.
```

所以就是
```
[wolfdan@ecs-t6-medium-2-linux-20191014150446 cpp_study]$ find . -name "*msgpack*"
./section4/msgpack.cpp
```
2020年11月23日17:14:41
然后就知道应该是整个大文件编译

3. 整体编译发现json.hpp需要到另一个文件去找
(搜狗输入法对于mobaxshell的vim不太兼容删除键)

编译报错，然后去看json.cpp,发现json.hpp要自己wget.然后放到../common下

但是其实还是要放到section4下面

4. make报错了
```bash
[ 21%] Building CXX object section2/CMakeFiles/auto.dir/auto.cpp.o
/home/wolfdan/code/cpp_study/section2/auto.cpp: In function ‘void case6()’:
/home/wolfdan/code/cpp_study/section2/auto.cpp:97:5: error: expected primary-expression before ‘decltype’
     decltype(auto)     x1 = (x);
     ^
/home/wolfdan/code/cpp_study/section2/auto.cpp:97:5: error: expected ‘;’ before ‘decltype’
/home/wolfdan/code/cpp_study/section2/auto.cpp:98:5: error: expected primary-expression before ‘decltype’
     decltype(auto)     x2 = &x;
     ^
/home/wolfdan/code/cpp_study/section2/auto.cpp:98:5: error: expected ‘;’ before ‘decltype’
/home/wolfdan/code/cpp_study/section2/auto.cpp:99:5: error: expected primary-expression before ‘decltype’
     decltype(auto)     x3 = x1;
     ^
/home/wolfdan/code/cpp_study/section2/auto.cpp:99:5: error: expected ‘;’ before ‘decltype’
make[2]: *** [section2/CMakeFiles/auto.dir/auto.cpp.o] Error 1
make[1]: *** [section2/CMakeFiles/auto.dir/all] Error 2
make: *** [all] Error 2

```

5. section2,3持续报错，取消编译
2020年11月25日00:41:01 现在的目标是要运行section5，调研这个项目可否作为毕设，所以就把section2报错的地方注释掉先

2020年11月25日00:45:32 又报错，继续注释
```
/home/wolfdan/code/cpp_study/section2/const.cpp: In function ‘constexpr int fib(int)’:
/home/wolfdan/code/cpp_study/section2/const.cpp:79:1: error: body of constexpr function ‘constexpr int fib(int)’ not a return-statement
 }
 ^
/home/wolfdan/code/cpp_study/section2/const.cpp: In function ‘int main(’:
/home/wolfdan/code/cpp_study/section2/const.cpp:89:21: error: ‘constexpr int fib(int)’ called in a constant expression
     int fib5 = fib(5);
                     ^
make[2]: *** [section2/CMakeFiles/section2.dir/const.cpp.o] Error 1
make[1]: *** [section2/CMakeFiles/section2.dir/all] Error 2
make: *** [all] Error 2
```


2020年11月25日00:54:02，**section2，section3接连报错，所以直接把总CMakeList.txt中除了section4,5的地方全注释掉**

6. 又丢失头文件
```bash
/home/wolfdan/code/cpp_study/section4/cpr.cpp:13:21: fatal error: cpr/cpr.h: No such file or directory
 #include <cpr/cpr.h>
                     ^
compilation terminated.
make[2]: *** [section4/CMakeFiles/section4.dir/cpr.cpp.o] Error 1
make[1]: *** [section4/CMakeFiles/section4.dir/all] Error 2
make: *** [all] Error 2
[wolfdan@ecs-t6-medium-2-linux-20191014150446 build]$ cd ..
[wolfdan@ecs-t6-medium-2-linux-20191014150446 cpp_study]$ find . -name "*cpr*"
./section4/cpr.cpp
./build/section4/CMakeFiles/cpr.dir
```

找到`.c`文件，然后看到最上面的GitHub链接，就可以找到对应的地方去进行安装头文件

```bash
/cpr/cprtypes.h:4:23: fatal error: curl/curl.h: No such file or directory
 #include <curl/curl.h>
```

2020年11月25日01:20:42 累了，睡觉