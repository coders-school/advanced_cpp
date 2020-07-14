## `using` alias

**Rationale**: More intuitive alias creation.

A type alias is a name that refers to a previously defined type. It could be created with typedef.

From C++11 type aliases should be created with `using` keyword.

```cpp
typedef std::ios_base::fmtflags Flags;
using Flags = std::ios_base::fmtflags;
Flags fl = std::ios_base::dec;

typedef std::vector<std::shared_ptr<Socket>> SocketContainer;
std::vector<std::shared_ptr<Socket>> typedef SocketContainer; // correct ;)
using SocketContainer = std::vector<std::shared_ptr<Socket>>;
```

___

### Template alises

```cpp
template <typename T>
using StrKeyMap = std::map<std::string, T>;

StrKeyMap<int> my_map; // std::map<std::string, int>
```

Type alias can be parametrized with templates. It was impossible with typedef.

Template aliases cannot be specialized.

___

### Constructors inheritance

```cpp
struct A {
    explicit A(int);
    int a;
};

struct B : A {
    using A::A;  // implicit declaration of B::B(int)
    B(int, int); // overloaded inherited Base ctor
};
```

* <!-- .element: class="fragment fade-in" --> Derived class constructors are generated implicitly, only if they are used
* <!-- .element: class="fragment fade-in" --> Derived class constructors take the same arguments as base class constructors
* <!-- .element: class="fragment fade-in" --> Derived class constructor calls according base class constructor
* <!-- .element: class="fragment fade-in" --> Constructor inheritance in a class that adds a new field might be risky - new fields can be uninitialized
