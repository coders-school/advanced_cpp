<!-- .slide: data-background="#111111" -->
# `static_assert`

___

## `static_assert`

**Rationale**: Preventing compilation on user defined conditions (usually specific types)

Performs compile-time assertion checking. Usually used with type traits library.

The message is optional from C++17.

```cpp
template <class T>
void swap(T& a, T& b)
{
    static_assert(std::is_copy_constructible<T>::value,
                  "Swap requires copying");
    static_assert(std::is_nothrow_move_constructible_v<T> &&
                  std::is_nothrow_move_assignable_v<T>);
    auto c = b;
    b = a;
    a = c;
}
```
