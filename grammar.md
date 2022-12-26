# Grammar

## C++ GRAMMAR

### optimize function

写法1: 只针对1个function。

```cpp
__attribute__((optimize("O0")))
int add (int a, int b )
{
    int x = a;
    int y = b;
    return x + y;
}

int main ()
{
    int r = 1;
    int a = r;
    int b = r;
    func ();
    return 0;
}
```

写法2：包含在里面的code都不会被optimize。

> [!TIP]
> 注意！这个写法不一定要以function为范围，可以任意选取一段code。

```cpp
#pragma GCC push_options
#pragma GCC optimize ("O0")
int add (int a, int b )
{
    int x = a;
    int y = b;
    return x + y;
}

#pragma GCC pop_options
int main ()
{
    int r = 1;
    int a = r;
    int b = r;
    func ();
    return 0;
}
```

### enums Start At 0

It will start at 0 and increment up along the way.

## OPTIMIZE

```cpp
// toggle this
// __attribute__((optimize(0)))
void waste_time() { for (unsigned i = 100000; i--; ); }

// always leave this on
__attribute__((optimize(0)))
void test() {
    for (unsigned i = 1000; i--; ) {
        waste_time();
    }
}

int main() {
    std::cout << Timer::measure(test).count() << "ms\n";
}
```
