## Dynamic allocation problems

* <!-- .element: class="fragment fade-in" --> accessing out-of-bounds memory
* <!-- .element: class="fragment fade-in" --> dangling pointer
* <!-- .element: class="fragment fade-in" --> double deleting
* <!-- .element: class="fragment fade-in" --> <code>null</code> pointer dereference
* <!-- .element: class="fragment fade-in" --> freeing memory blocks that were not dynamically allocated
* <!-- .element: class="fragment fade-in" --> freeing a portion of a dynamic block
* <!-- .element: class="fragment fade-in" --> memory leak

These problems can be addressed by ASAN (Address Sanitizer) or Valgrind.
Unfortunately they do not work on Windows 😕
<!-- .element: class="fragment fade-in" -->

___

## Accessing out-of-bounds memory

Undefined behavior

```cpp
#include <iostream>

int main() {
    const auto size = 10;
    int* dynamicArray = new int[size];

    for (int i = 0; i <= size; ++i)
        *(dynamicArray + i) = i * 10;

    for (int i = 0; i <= size; ++i)
        std::cout << dynamicArray[i] << '\n';

    delete[] dynamicArray;
}
```

___

## Dangling pointer

A pointer which indicates to something that is not valid

```cpp
#include <iostream>

struct Msg {
    int value{100};
};

void processMsg(Msg* msg) {
    std::cout << msg->value << '\n';
}

int main() {
    Msg* m = new Msg();
    // ...
    delete m;
    processMsg(m);

    return 0;
}
```

___

## Double delete

Happens when a dangling pointer is deleted

```cpp
class Msg {};

void processMsg(Msg* msg) {
    // ...
    delete msg;
}

int main() {
    Msg* m = new Msg{};
    processMsg(m);
    delete m;
}
```

___

## Null pointer dereference

Happens when a `nullptr` is used

```cpp
#include <iostream>

int main() {
    int* p = new int{10};
    delete p;
    p = nullptr;

    std::cout << *p << '\n';

    return 0;
}
```

___

## Freeing stack allocated blocks

```cpp
class Msg {};

void processMsg(Msg* msg) {
    // ...
    delete msg;
}

int main() {
    Msg m;
    processMsg(&m);

    return 0;
}
```

___

## Freeing a portion of a dynamic block

Using `delete` instead of `delete[]`

```cpp
int main() {
    constexpr auto size = 4u;
    int* array = new int[size]{1, 2, 3, 4};
    delete array;

    return 0;
}
```

___

## Memory leak

Allocated memory which cannot be freed because there is no pointer that points to it

```cpp
#include <iostream>

int main() {
    int* p = new int{10};
    p = new int{20};
    std::cout << *p << '\n';
    delete p;

    return 0;
}
```
