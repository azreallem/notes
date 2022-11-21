# Grammer

## c++

### optimize of function

寫法1: 只針對1個function。

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

寫法2：包含在裡面的code都不會被optimize。注意！這個寫法不一定要以function為範圍，可以任意選取一段code。

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

### enums Start at 0

It will start at 0 and increment up along the way.
