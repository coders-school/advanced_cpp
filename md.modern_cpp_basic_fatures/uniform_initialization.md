<!-- .slide: data-background="#111111" -->
# Uniform initialization

___

## C++98 initialization

<pre><code class="cpp" data-trim data-line-numbers data-noescape>
int a;          <span class="fragment">// undefined value</span>
int b(5);       <span class="fragment">// direct initialization, b = 5</span>
int c = 10;     <span class="fragment">// copy initialization, c = 10</span>
int d = int();  <span class="fragment">// default initialization, d = 0</span>
int e();        <span class="fragment">// function declaration - "most vexing parse"</span>

int values[] = { 1, 2, 3, 4 };  <span class="fragment">// brace initialization of aggregate</span>
int array[] = { 1, 2, 3.5 };  <span class="fragment">// C++98 - ok, implicit type narrowing</span>

struct P { int a, b; };
P p = { 20, 40 };   <span class="fragment">// brace initialization of POD</span>

std::complex<double> c(4.0, 2.0); <span class="fragment">// initialization of classes</span>

std::vector<std::string> names; <span class="fragment">// no initialization for list of values</span>
names.push_back("John");
names.push_back("Jane");
</code></pre>

___

## C++11 initialization with {}

**Rationale**: eliminate problematic initialization cases from C++98, initialization of STL containers, have only one universal way of initialization.

<pre><code class="cpp" data-trim data-line-numbers data-noescape>
int a;          <span class="fragment">// still undefined value</span>
int b{5};       <span class="fragment">// brace initialization, b = 5</span>
int c{};        <span class="fragment">// brace initialization, c = 0</span>

int values[] = { 1, 2, 3, 4 };  <span class="fragment">// brace initialization of aggregate</span>
int array[] = { 1, 2, 3.5 };    <span class="fragment">// C++11: error - implicit type narrowing</span>

struct P { int a, b; };
P p = { 20, 40 };   <span class="fragment">// brace initialization of POD</span>

std::complex<double> c{4.0, 2.0};   <span class="fragment">// brace initialization calls adequate c-tor</span>

std::vector<std::string> names = { "John", "Jane" };  <span class="fragment">// brace initialization of vector</span>
</code></pre>

___

## In-class initialization of non-static variables

```cpp
struct Foo {
    Foo() {}
    Foo(std::string a) : a_(a) {}
    void print() { std::cout << a_ << std::endl; }

private:
   std::string a_ = "Foo";              // C++98: error, C++11: OK
   static const unsigned VALUE = 20u;   // C++98: OK, C++11: OK
};

Foo().print();          // Foo
Foo("Bar").print();     // Bar
```

___

## `std::initializer_list<T>`

```cpp
auto values = {1, 2, 3, 4, 5};   // values : std::initializer_list<int>
std::vector<int> v = {1, 2, -3}; // creates vector from
                                 // std::initalizer_list<int>
```

* <!-- .element: class="fragment fade-in" --> Defined in <code><initializer_list></code> header
* <!-- .element: class="fragment fade-in" --> Elements are kept in an array
* <!-- .element: class="fragment fade-in" --> Elements are immutable
* <!-- .element: class="fragment fade-in" --> Elements must be copyable
* <!-- .element: class="fragment fade-in" --> Have limited interface and access via iterators - <code>begin()</code>, <code>end()</code>, <code>size()</code>
* <!-- .element: class="fragment fade-in" --> Should be passed to functions by value

___

## `std::initializer_list<T>`

<pre><code class="cpp" data-trim data-line-numbers data-noescape>
template<class Type>
class Bar {
    std::vector<Type> values_;
public:
    Bar(std::initializer_list<Type> values) : values_(values) {}
    Bar(Type a, Type b) : values_{a, b} {}
};

<span class="fragment">Bar<int> c = {1, 2, 5, 51};</span>   <span class="fragment">// calls std::initializer_list c-tor</span>
<span class="fragment">Bar<int> d{1, 2, 5, 51};</span>      <span class="fragment">// calls std::initializer_list c-tor</span>
<span class="fragment">Bar<int> e = {1, 2};</span>          <span class="fragment">// calls std::initializer_list c-tor</span>
<span class="fragment">Bar<int> f{1, 2};</span>             <span class="fragment">// calls std::initializer_list c-tor</span>
<span class="fragment">Bar<int> g(1, 2);</span>             <span class="fragment">// calls Bar(Type a, Type b) c-tor</span>
<span class="fragment">Bar<int> h = {};</span>              <span class="fragment">// calls std::initializer_list c-tor</span>
                         <span class="fragment">// or default c-tor if exists</span>
<span class="fragment">Bar<std::unique_ptr<int>> c = {new int{1}, new int{2}};</span>
<span class="fragment">// error - std::unique_ptr is non-copyable</span>
</code></pre>

C-tor with <code>std::initializer_list</code> has greater priority if other c-tors match.
<!-- .element: class="fragment fade-in" -->