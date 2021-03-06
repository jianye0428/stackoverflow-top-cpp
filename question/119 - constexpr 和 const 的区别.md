<https://stackoverflow.com/questions/14116003/difference-between-constexpr-and-const>

## 问题

constexpr 和 const 之间有什么区别？

## 回答

**对变量来说，**

const 表示的只是这个变量不可修改，但并未限定这个变量是编译期常量还是运行期常量；而 constexpr 只能是编译期常量。

```c++
const int kSize = 1; // 编译期常量

void func()
{
    const int kRandomNumber = get_a_random_number(); // 运行期常量
    ...
    ...
}
```

对于 kSize，你既可以用 const 修饰，也可以用 constexpr。但对于 kRandomNumber，你只能用 const。

**对函数来说，**

const 修饰的函数一般都是成员函数，用来表示这个函数不会对成员变量产生写操作，这点很好理解。

我们重点来看 constexpr。

我们可以把 constexpr 拆开来看就是 `const expression`，意即常量表达式。

```c++
constexpr int func(int i)
{
    return i + 1;
}

int main()
{
    int i             = 10;
    const int ci      = 10;
    constexpr int cei = 10;

    std::array<int, func(i)>   arr1; // 编译错误
    std::array<int, func(ci)>  arr2; // 没问题
    std::array<int, func(cei)> arr3; // 没问题
    std::array<int, func(10)>  arr4; // 没问题

    func(i); // 直接使用，也是没问题的

    return 0;
}
```

constexpr 修饰的函数，简单的来说，如果其传入的参数可以在编译时期计算出来，那么这个函数就会产生编译时期的值。但是，传入的参数如果不能在编译时期计算出来，那么 constexpr 修饰的函数就和普通函数一样了，比如上面代码直接调用了 `func(i)`。不过，我们不必因此而写两个版本，所以如果函数体适用于 constexpr 函数的条件，可以尽量加上 constexpr。

参考：<https://www.zhihu.com/question/35614219>
