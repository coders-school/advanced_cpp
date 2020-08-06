## Memory corruption detection

* <!-- .element: class="fragment fade-in" --> Address Sanitizer (ASAN)
  * <!-- .element: class="fragment fade-in" --> add compilation flags:
    * <!-- .element: class="fragment fade-in" --> <code>–fsanitize=address -g</code>
    * <!-- .element: class="fragment fade-in" --> <code>–fsanitize=leak -g</code>
  * <!-- .element: class="fragment fade-in" --> run a binary
* <!-- .element: class="fragment fade-in" --> Valgrind
  * <!-- .element: class="fragment fade-in" --> compile a binary
  * <!-- .element: class="fragment fade-in" --> run a binary under valgrind:
    * <!-- .element: class="fragment fade-in" --> <code>valgrind /path/to/binary</code>
  * <!-- .element: class="fragment fade-in" --> use additional checks:
    * <!-- .element: class="fragment fade-in" --> <code>valgrind --leak-check=full /path/to/binary</code>

Neither work on Windows 😕
<!-- .element: class="fragment fade-in" -->
