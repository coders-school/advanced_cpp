<!-- .slide: data-background="#111111" -->

## Cyclic dependencies

<img data-src="img/cyclicinverted.png" alt="cyclic dependencies" class="plain fragment fade-in">

* <!-- .element: class="fragment fade-in" --> Cyclic dependency is where you have class A with self-referencing member.
* <!-- .element: class="fragment fade-in" --> Cyclic dependency is where you have two classes A and B where A has a reference to B which has a reference to A.
* <!-- .element: class="fragment fade-in" --> How to fix it?

___

### `std::weak_ptr<>` to the rescue

Traits:

* <!-- .element: class="fragment fade-in" --> does not own an object
* <!-- .element: class="fragment fade-in" --> observes only
* <!-- .element: class="fragment fade-in" --> must be converted to <code>std::shared_ptr<></code> to access the object
* <!-- .element: class="fragment fade-in" --> can be created only from a <code>std::shared_ptr<></code>

<div>
    <img data-src="img/weakptrinverted.png" alt="weak pointers" class="plain fragment fade-in">
</div>

___

### `std::weak_ptr<>` usage

<div class="multicolumn" style="position: relative">
<div class="col" style="width: 65%; flex: none">

```cpp
#include <memory>
#include <iostream>

struct Msg { int value; };

void checkMe(const std::weak_ptr<Msg> & wp) {
    std::shared_ptr<Msg> p = wp.lock();
    if (p)
        std::cout << p->value << '\n';
    else
        std::cout << "Expired\n";
}

int main() {
    auto sp = std::shared_ptr<Msg>{new Msg{10}};
    auto wp = std::weak_ptr<Msg>{sp};
    checkMe(wp);
    sp.reset();
    checkMe(wp);
}
```

</div>

<div class="col">

```bash
> ./a.out
10
Expired
```

</div>
</div>

___

### `std::shared_ptr<>` cyclic dependencies

* How to solve this problem?

```cpp
#include <memory>

struct Node {
    std::shared_ptr<Node> child;
    std::shared_ptr<Node> parent;
};

int main () {
    auto root = std::shared_ptr<Node>(new Node);
    auto child = std::shared_ptr<Node>(new Node);
    root->child = child;
    child->parent = root;
}
```

___

### Breaking cycle - solution

* Use `std::weak_ptr<Node>` in one direction

```cpp
#include <memory>
struct Node {
    std::shared_ptr<Node> child;
    std::weak_ptr<Node> parent;
};

int main () {
    auto root = std::shared_ptr<Node>(new Node);
    auto child = std::shared_ptr<Node>(new Node);
    root->child = child;
    child->parent = root;
}
```
<!-- .element: class="fragment fade-in" -->

<div class="box fragment fade-in">

==148== All heap blocks were freed -- no leaks are possible

</div>
