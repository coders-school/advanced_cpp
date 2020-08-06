<!-- .slide: data-background="#111111" -->

# Best practices

___

## Best practices

* <!-- .element: class="fragment fade-in" --> Rule of 0, Rule of 5
* <!-- .element: class="fragment fade-in" --> Avoid explicit <code>new</code>
* <!-- .element: class="fragment fade-in" --> Use <code>std::make_shared()</code> / <code>std::make_unique()</code>
* <!-- .element: class="fragment fade-in" --> Copying <code>std::shared_ptr<></code>
* <!-- .element: class="fragment fade-in" --> Use references instead of pointers

___

## Rule of 0, Rule of 5

### Rule of 5 <!-- .element: class="fragment fade-in" -->

* <!-- .element: class="fragment fade-in" --> If you need to implement one of those functions:
  * <!-- .element: class="fragment fade-in" --> destructor
  * <!-- .element: class="fragment fade-in" --> copy constructor
  * <!-- .element: class="fragment fade-in" --> copy assignment operator
  * <!-- .element: class="fragment fade-in" --> move constructor
  * <!-- .element: class="fragment fade-in" --> move assignment operator
* <!-- .element: class="fragment fade-in" --> It probably means that you should implement them all, because you have manual resources management.

### Rule of 0 <!-- .element: class="fragment fade-in" -->

* <!-- .element: class="fragment fade-in" --> If you use RAII wrappers on resources, you don’t need to implement any of Rule of 5 functions.

___

## Avoid explicit `new`

* <!-- .element: class="fragment fade-in" --> Smart pointers eliminate the need to use <code>delete</code> explicitly
* <!-- .element: class="fragment fade-in" --> To be symmetrical, do not use <code>new</code> as well
* <!-- .element: class="fragment fade-in" --> Allocate using:
  * <!-- .element: class="fragment fade-in" --> <code>std::make_unique()</code>
  * <!-- .element: class="fragment fade-in" --> <code>std::make_shared()</code>

___

<!-- .slide: style="font-size: 0.8em" -->

### Use `std::make_shared()` / `std::make_unique()`

* <!-- .element: class="fragment fade-in" --> What is a problem here?

```cpp
struct MyData { int value; };
using Ptr = std::shared_ptr<MyData>;
void sink(Ptr oldData, Ptr newData);

void use(void) {
    sink(Ptr{new MyData{41}}, Ptr{new MyData{42}});
}
```
<!-- .element: class="fragment fade-in" -->

* <!-- .element: class="fragment fade-in" --> Hint: this version is not problematic

```cpp
struct MyData { int value; };
using Ptr = std::shared_ptr<MyData>;
void sink(Ptr oldData, Ptr newData);

void use(void) {
    Ptr oldData{new MyData{41}};
    Ptr newData{new MyData{42}};
    sink(std::move(oldData), std::move(newData));
}
```
<!-- .element: class="fragment fade-in" -->

___

### Allocation deconstructed

`auto p = new MyData(10);` means:

* <!-- .element: class="fragment fade-in" --> allocate <code>sizeof(MyData)</code> bytes
* <!-- .element: class="fragment fade-in" --> run <code>MyData</code> constructor
* <!-- .element: class="fragment fade-in" --> assign address of allocated memory to <code>p</code>

The order of evaluation of operands of almost all C++ operators (including the order of
evaluation of function arguments in a function-call expression and the order of evaluation of
the subexpressions within any expression) is **unspecified**.
<!-- .element: class="fragment fade-in box" -->

___
<!-- .slide: style="font-size: 0.77em" -->

### Unspecified order of evaluation

* How about two such operations?

| first operation (A)                           | second operation (B)                          |
| :-------------------------------------------- | :-------------------------------------------- |
| (1) allocate `sizeof(MyData)` bytes           | (1) allocate `sizeof(MyData)` bytes           |
| (2) run `MyData` constructor                  | (2) run `MyData` constructor                  |
| (3) assign address of allocated memory to `p` | (3) assign address of allocated memory to `p` |

* <!-- .element: class="fragment fade-in" --> Unspecified order of evaluation means that order can be for example:
  * A1, A2, B1, B2, C3, C3
* <!-- .element: class="fragment fade-in" --> What if B2 throws an exception?

___

### Use `std::make_shared()` / `std::make_unique()`

* <!-- .element: class="fragment fade-in" --> <code>std::make_shared()</code> / <code>std::make_unique()</code> resolves this problem

```cpp
struct MyData{ int value; };
using Ptr = std::shared_ptr<MyData>;
void sink(Ptr oldData, Ptr newData);

void use() {
    sink(std::make_shared<MyData>(41), std::make_shared<MyData>(42));
}
```
<!-- .element: class="fragment fade-in" -->

* <!-- .element: class="fragment fade-in" --> Fixes previous bug
* <!-- .element: class="fragment fade-in" --> Does not repeat a constructed type
* <!-- .element: class="fragment fade-in" --> Does not use explicit <code>new</code>
* <!-- .element: class="fragment fade-in" --> Optimizes memory usage (only for <code>std::make_shared()</code>)

___

## Copying `std::shared_ptr<>`

```cpp
void foo(std::shared_ptr<MyData> p);

void bar(std::shared_ptr<MyData> p) {
    foo(p);
}
```

* <!-- .element: class="fragment fade-in" --> requires counters incrementing / decrementing
* <!-- .element: class="fragment fade-in" --> atomics / locks are not free
* <!-- .element: class="fragment fade-in" --> will call destructors

##### Can be better?
<!-- .element: class="fragment fade-in" -->

___

## Copying `std::shared_ptr<>`

```cpp
void foo(const std::shared_ptr<MyData> & p);

void bar(const std::shared_ptr<MyData> & p) {
    foo(p);
}
```

* <!-- .element: class="fragment fade-in" --> as fast as pointer passing
* <!-- .element: class="fragment fade-in" --> no extra operations
* <!-- .element: class="fragment fade-in" --> not safe in multithreaded applications

___

### Use references instead of pointers

* <!-- .element: class="fragment fade-in" --> What is the difference between a pointer and a reference?
  * <!-- .element: class="fragment fade-in" --> reference cannot be empty
  * <!-- .element: class="fragment fade-in" --> reference, once assigned cannot point to anything else
* <!-- .element: class="fragment fade-in" --> Priorities of usage (if possible):
  * <!-- .element: class="fragment fade-in" --> <code>(const) T&</code>
  * <!-- .element: class="fragment fade-in" --> <code>std::unique_ptr&ltT&gt</code>
  * <!-- .element: class="fragment fade-in" --> <code>std::shared_ptr&ltT&gt</code>
  * <!-- .element: class="fragment fade-in" --> <code>T*</code>

___

## Exercise: List

Take a look at `List.cpp` file, where simple (and buggy) single-linked list is implemented.

* `void add(Node* node)` method adds a new `Node` at the end of the list.
* `Node* get(const int value)` method iterates over the list and returns the first Node with matching `value` or `nullptr`

1. <!-- .element: class="fragment fade-in" --> Compile and run List application
2. <!-- .element: class="fragment fade-in" --> Fix memory leaks without introducing smart pointers
3. <!-- .element: class="fragment fade-in" --> Fix memory leaks with smart pointers. What kind of pointers needs to be applied and why?
4. <!-- .element: class="fragment fade-in" --> (Optional) What happens when the same Node is added twice? Fix this problem.
5. <!-- .element: class="fragment fade-in" --> (Optional) Create <code>EmptyListError</code> exception (deriving from <code>std::runtime_error</code>). Add throwing and catching it in a proper places.
