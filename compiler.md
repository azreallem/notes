# complier

## optimize

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
