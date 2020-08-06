## Heap allocation

<div class="multicolumn">
<div class="col" style="margin-top: 70px; font-size: 85%;">

Heap allocation consists of a few steps:
<!-- .element: class="fragment fade-in" -->

* <!-- .element: class="fragment fade-in" --> pointer allocation on a stack
* <!-- .element: class="fragment fade-in" --> <code>sizeof(T)</code> bytes allocation on a heap
* <!-- .element: class="fragment fade-in" --> <code>T</code>’s constructor call on allocated memory
* <!-- .element: class="fragment fade-in" --> the memory address assignment to the pointer
* <!-- .element: class="fragment fade-in" --> manual deallocation using <code>delete</code> operator

</div>
<div class="col" style="margin: 15px;">

```cpp
void heap()
{
    int *p = new int(100);
    delete p;
}

void heap()
{
    int *p;
    p = (int*)malloc(sizeof(int));
    *p = 100;
    free(p);
}
```

</div>
</div>