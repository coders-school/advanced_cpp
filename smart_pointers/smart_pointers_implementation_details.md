<!-- .slide: data-background="#111111" -->

# Implementation details

___

### Implementation details – `std::unique_ptr<>`

* <!-- .element: class="fragment fade-in" --> Just a holding wrapper
* <!-- .element: class="fragment fade-in" --> Holds an object pointer
* <!-- .element: class="fragment fade-in" --> Constructor copies a pointer
* <!-- .element: class="fragment fade-in" --> Call proper delete in destructor
* <!-- .element: class="fragment fade-in" --> No copying
* <!-- .element: class="fragment fade-in" --> Moving means:
  * <!-- .element: class="fragment fade-in" --> Copying original pointer to a new object
  * <!-- .element: class="fragment fade-in" --> Setting source pointer to <code>nullptr</code>
* <!-- .element: class="fragment fade-in" --> All methods are inline

___

### Implementation details – `std::shared_ptr<>`

<img data-src="img/sharedptr2inverted.png" alt="sharedptr2" class="plain fragment fade-in">

* <!-- .element: class="fragment fade-in" --> Holds an object pointer
* <!-- .element: class="fragment fade-in" --> Holds 2 reference counters:
  * <!-- .element: class="fragment fade-in" --> shared pointers count
  * <!-- .element: class="fragment fade-in" --> weak pointers count
* <!-- .element: class="fragment fade-in" --> Destructor:
  * <!-- .element: class="fragment fade-in" --> decrements <code>shared-refs</code>
  * <!-- .element: class="fragment fade-in" --> deletes user data when <code>shared-refs == 0</code>
  * <!-- .element: class="fragment fade-in" --> deletes reference counters when <code>shared-refs == 0</code> and <code>weak-refs == 0</code>
* <!-- .element: class="fragment fade-in" --> Extra space for a deleter
* <!-- .element: class="fragment fade-in" --> All methods are inline

___

### Implementation details – `std::shared_ptr<>`

* <!-- .element: class="fragment fade-in" --> Copying means:
  * <!-- .element: class="fragment fade-in" --> Copying pointers to the target
  * <!-- .element: class="fragment fade-in" --> Incrementing <code>shared-refs</code>

<img data-src="img/sharedptr3inverted.png" alt="sharedptr3" class="plain fragment fade-in">

* <!-- .element: class="fragment fade-in" --> Moving means:
  * <!-- .element: class="fragment fade-in" --> Copying pointers to the target
  * <!-- .element: class="fragment fade-in" --> Setting source pointers to <code>nullptr</code>

<img data-src="img/sharedptr4inverted.png" alt="sharedptr4" class="plain fragment fade-in">

___

### Implementation details – `std::weak_ptr<>`

* <!-- .element: class="fragment fade-in" --> Holds an object pointer
* <!-- .element: class="fragment fade-in" --> Holds 2 reference counters:
  * <!-- .element: class="fragment fade-in" --> shared pointers count
  * <!-- .element: class="fragment fade-in" --> weak pointers count
* <!-- .element: class="fragment fade-in" --> Destructor:
  * <!-- .element: class="fragment fade-in" --> decrements <code>weak-refs</code>
  * <!-- .element: class="fragment fade-in" --> deletes reference counters when <code>shared-refs == 0</code> and <code>weak-refs == 0</code>

<img data-src="img/sharedptr5inverted.png" alt="sharedptr5" class="plain fragment fade-in">

___

### Implementation details – `std::weak_ptr<>`

* <!-- .element: class="fragment fade-in" --> Copying means:
  * <!-- .element: class="fragment fade-in" --> Copying pointers to the target
  * <!-- .element: class="fragment fade-in" --> Incrementing <code>weak-refs</code>

<img data-src="img/sharedptr6inverted.png" alt="sharedptr6" class="plain fragment fade-in">

* <!-- .element: class="fragment fade-in" --> Moving means:
  * <!-- .element: class="fragment fade-in" --> Copying pointers to the target
  * <!-- .element: class="fragment fade-in" --> Setting source pointers to <code>nullptr</code>

<img data-src="img/sharedptr7inverted.png" alt="sharedptr7" class="plain fragment fade-in">

___

### `std::weak_ptr<>` + `std::shared_ptr<>`

* <!-- .element: class="fragment fade-in" --> Having a shared pointer and a weak pointer

<img data-src="img/sharedptr8inverted.png" alt="sharedptr8" class="plain fragment fade-in">

* <!-- .element: class="fragment fade-in" --> After removing the shared pointer

<img data-src="img/sharedptr9inverted.png" alt="sharedptr9" class="plain fragment fade-in">

___

## Making a `std::shared_ptr<>`

* <!-- .element: class="fragment fade-in" --> <code>std::shared_ptr&ltData&gt p{new Data};</code>

<img data-src="img/sharedptr10inverted.png" alt="sharedptr10" class="plain fragment fade-in">

* <!-- .element: class="fragment fade-in" --> <code>auto p = std::make_shared&ltData&gt();</code>
  * <!-- .element: class="fragment fade-in" --> Less memory (most likely)
  * <!-- .element: class="fragment fade-in" --> Only one allocation
  * <!-- .element: class="fragment fade-in" --> Cache-friendly

<img data-src="img/sharedptr11inverted.png" alt="sharedptr11" class="plain fragment fade-in">
