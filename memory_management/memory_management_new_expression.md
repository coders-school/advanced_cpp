## New expression and operator `new`

[new expression](https://en.cppreference.com/w/cpp/language/new) does 3 things:

* <!-- .element: class="fragment fade-in" --> <code>sizeof(T)</code> bytes allocation on a heap (via proper <a href="https://en.cppreference.com/w/cpp/memory/new/operator_new">operator <code>new</code></a>)
* <!-- .element: class="fragment fade-in" --> <code>T</code>’s constructor call on allocated memory
* <!-- .element: class="fragment fade-in" --> the memory address assignment to the pointer

```cpp
// replaceable allocation functions
void* operator new ( std::size_t count );
void* operator new[]( std::size_t count );
// replaceable non-throwing allocation functions
void* operator new ( std::size_t count, const std::nothrow_t& tag);
void* operator new[]( std::size_t count, const std::nothrow_t& tag);
// user-defined placement allocation functions
void* operator new ( std::size_t count, user-defined-args... );
void* operator new[]( std::size_t count, user-defined-args... );
// additional param std::align_val_t since C++17, [[nodiscard]] since C++20
// some more versions on https://en.cppreference.com/w/cpp/memory/new/operator_new
```
