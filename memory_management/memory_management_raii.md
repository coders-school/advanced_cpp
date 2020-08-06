## RAII

<div class="multicolumn">
<div class="col">

* <!-- .element: class="fragment fade-in" --> Resource Acquisition Is Initialization
  * <!-- .element: class="fragment fade-in" --> idiom / pattern in C++
  * <!-- .element: class="fragment fade-in" --> each resource has a handler
  * <!-- .element: class="fragment fade-in" --> acquired in constructor
  * <!-- .element: class="fragment fade-in" --> released in destructor
* <!-- .element: class="fragment fade-in" --> Benefits
  * <!-- .element: class="fragment fade-in" --> shorter code (automation)
  * <!-- .element: class="fragment fade-in" --> clear responsibility
  * <!-- .element: class="fragment fade-in" --> applies to any resources
  * <!-- .element: class="fragment fade-in" --> no need for <code>finally</code> sections
  * <!-- .element: class="fragment fade-in" --> predictable release times
  * <!-- .element: class="fragment fade-in" --> language-level guarantee of correctness

</div>
<div class="col" style="font-size: 70%;margin-top: 70px;">

|             | Acquire       | Release          |
|:-----------:|:-------------:|:----------------:|
| memory      | new, new[]    | delete, delete[] |
| files       | fopen         | fclose           |
| locks       | lock, try_lock| unlock           |
| sockets     | socket        | close            |

</div>
<!-- .element: class="fragment fade-in" -->
