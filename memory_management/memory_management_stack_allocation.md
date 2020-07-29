## Stack allocation

<div class="multicolumn">
<div class="col">

* <!-- .element: class="fragment fade-in" --> A call stack is composed of stack frames

* <!-- .element: class="fragment fade-in" --> A stack frame usually includes at least:
  * <!-- .element: class="fragment fade-in" --> arguments passed to a function (if any)
  * <!-- .element: class="fragment fade-in" --> the return address back to the caller
  * <!-- .element: class="fragment fade-in" --> space for local variables (if any)
* <!-- .element: class="fragment fade-in" --> Automatic deallocation when out of scope

<img height="200" data-src="img/stack.png" src="img/stack.png" alt="stack" class="plain">
<!-- .element: class="fragment fade-in" --> 

</div>
<div class="col">

```cpp
#include <iostream>

int sum(int a, int b)
{
    return a + b;
}

int main()
{
    int a = 10;
    int b = 20;

    std::cout << sum(a, b);

    return 0;
}


```

</div>
</div>