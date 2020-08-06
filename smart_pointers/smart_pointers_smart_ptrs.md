<!-- .slide: data-background="#111111" -->

# Smart pointers

___

## Smart pointers

* <!-- .element: class="fragment fade-in" --> A smart pointer manages a pointer to a heap allocated object

  * <!-- .element: class="fragment fade-in" --> Deletes the pointed-to object at the right time
  * <!-- .element: class="fragment fade-in" --> <code>operator->()</code> calls managed object methods
  * <!-- .element: class="fragment fade-in" --> <code>operator.()</code> calls smart pointer methods
  * <!-- .element: class="fragment fade-in" --> smart pointer to a base class can hold a pointer to a derived class

* <!-- .element: class="fragment fade-in" --> STL smart pointers:

  * <!-- .element: class="fragment fade-in" --> <code>std::unique_ptr<></code>
  * <!-- .element: class="fragment fade-in" --> <code>std::shared_ptr<></code>
  * <!-- .element: class="fragment fade-in" --> <code>std::weak_ptr<></code>
  * <!-- .element: class="fragment fade-in" --> <code>std::auto_ptr<></code> - removed in C++17
