## A simple question…

How many possible paths of execution are here?

```cpp
String EvaluateSalaryAndReturnName(Employee e)
{
    if( e.Title() == "CEO" || e.Salary() > 100000 )
    {
        cout << e.First() << " " << e.Last()
             << " is overpaid" << endl;
    }
    return e.First() + " " + e.Last();
}

```

* <!-- .element: class="fragment fade-in" --> 23 (twenty three)
* <!-- .element: class="fragment fade-in" --> Exceptions are the reason
* <!-- .element: class="fragment fade-in" --> Example by Herb Sutter, <a href="http://www.gotw.ca/gotw/020.htm">GotW#20</a>
