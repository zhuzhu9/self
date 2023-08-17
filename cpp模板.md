返回类型推断

```c++
template<typename T1, typename T2, typename RT>
RT max(T1 a,T2 b)
```



```c++
template<typename RT, typename T1, typename T2>
RT max(T1 a,T2 b)

::max<int> (3.4,6);
```



C++14开始可以不需要返回类型声明为任何模板的参数类型了，但是需要声明返回值为auto。

```c++
template<typename T1, typename T2>
auto max(T1 a,T2 b){
    return b < a ? a : b;
}
```

在C++14之前（C++11），可以用尾值返回类型，让编译器推导出来返回值的类型。

```c++
template<typename T1, typename T2>
auto max(T1 a,T2 b) -> decltype(b < a ? a : b){
    return b < a ? a : b;
}
```

由a和b通过运算符?:推到出来的返回值的类型。

**decltype推导类型的规则**

1. 如果e是一个没有带括号的标记符表达式或者类成员访问表达式，那么的decltype(e)就是e所命名的实体的类型。此外，如果e是一个被重载的函数，则会导致编译错误。
2. 否则 ，假设e的类型是T，如果e是一个将亡值，那么decltype(e)为T&&
3. 否则，假设e的类型是T，如果e是一个左值，那么decltype(e)为T&。
4. 否则，假设e的类型是T，则decltype(e)为T。



如果T是引用类型，返回类型可以被推导成引用。返回值应该是decay后的T，

```c++
template<typename T1, typename T2>
auto max(T1 a,T2 b) -> typename ::std::decay<decltype(true?a:b)>::type{
    return b < a ? a : b;
}
```

::std::decay<>类型萃取，它返回type成员作为目标类型，由于type成员是一个类型，为了获取结果需要用typename。

### 返回类型声明为公共类型

::std::common_type<>::type产生的类型是他的他一个模板参数的公共类型

```c++
#include <type_traits>
template<typename T1,typename T2>
::std::common_type_t<T1,T2> max(T1 a, T2 b){
    return b < a ? a : b;
}
```

从C++14开始，可以简化类型萃取，在后面加个_t，可以省略typename和::type

```c++
typename ::std::common_type<T1, T2>::type;
::std::common_type_t<T1, T2>;
```



























