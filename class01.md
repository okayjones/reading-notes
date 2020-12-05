# Class 01

## Pain vs. Suffering

[Source: Pain vs. Suffering](https://codefellows.github.io/code-401-python-guide/curriculum/class-01/notes/pain_suffering)

* Pain = growth
* Necesarry to go out of your comfort zone to grow
* Pain =/= suffering
* Suffering = pain w/o purpose

When you feel pain, reflect on:

1. What’s your perspective?
2. Why are you doing this?
3. Do you want what comes at the end of this journey?
4. Are you doing this for you?

## Big O

[Source: A beginner's guide to Big O notation](https://rob-bell.net/2009/06/a-beginners-guide-to-big-o-notation/)

* Describes performance of complecity of an algorithm
* Big 0 = worst case scenario
* Also described execution time or space used

### O(1) -- executed in same time regardless of input data

```java
bool IsFirstElementNull(IList<string> elements)
{
    return elements[0] == null;
}
```

### O(N) -- linear performance growth in proportion to dataset size

```java
bool ContainsValue(IList<string> elements, string value)
{
    foreach (var element in elements)
    {
        if (element == value) return true;
    }

    return false;
}
```

### O(N<sup>2</sup>) -- proportional to the square of the size of input data

```java
bool ContainsDuplicates(IList<string> elements)
{
    for (var outer = 0; outer < elements.Count; outer++)
    {
        for (var inner = 0; inner < elements.Count; inner++)
        {
            // Don't compare with self
            if (outer == inner) continue;

            if (elements[outer] == elements[inner]) return true;
        }
    }

    return false;
}
```

### O(2<sup>N</sup>) -- Doubles with each addition to input (exponenial)

```java
int Fibonacci(int number)
{
    if (number <= 1) return number;

    return Fibonacci(number - 2) + Fibonacci(number - 1);
}
```

### Logarithms -- Increasing has little affect

e.g. binary search 1/2 the set each search)
