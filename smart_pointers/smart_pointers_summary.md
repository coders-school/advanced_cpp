<!-- .slide: data-background="#111111" -->

# Smart pointers - summary

* <!-- .element: class="fragment fade-in" --> <code>#include &ltmemory&gt</code>
* <!-- .element: class="fragment fade-in" --> <code>std::unique_ptr<></code> for exclusive ownership
* <!-- .element: class="fragment fade-in" --> <code>std::shared_ptr<></code> for shared ownership
* <!-- .element: class="fragment fade-in" --> <code>std::weak_ptr<></code> for observation and breaking cycles

___

## Exercise: ResourceFactory

1. <!-- .element: class="fragment fade-in" --> Compile and run ResourceFactory application
2. <!-- .element: class="fragment fade-in" --> Put comments in places where you can spot some problems
3. <!-- .element: class="fragment fade-in" --> How to remove elements from the collection (<code>vector&ltResource*&gt</code> resources)?
4. <!-- .element: class="fragment fade-in" --> Check memory leaks
5. <!-- .element: class="fragment fade-in" --> Fix problems
