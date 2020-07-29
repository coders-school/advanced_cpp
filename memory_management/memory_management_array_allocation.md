## Dynamic array allocation

* `delete[]` is used to free an array memory

```cpp
#include <iostream>

int main() {
    int staticArray[] = {1, 2, 3, 4, 5, 6};

    constexpr auto size = 10;
    int* dynamicArray = new int[size];
    for (int i = 0; i < size; ++i)
        *(dynamicArray + i) = i * 10;

    for (int i = 0; i < size; ++i)
        std::cout << dynamicArray[i] << '\n';

    delete[] dynamicArray;
}
```