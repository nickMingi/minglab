---
layout: "post"
title: "AI Program Knowledge"
author: "mingi hong"
date: 2020-01-28 08:25:47 -0700
categories: "Computer AI"
permalink: /:categories/:title.html
---

## AI Knowledge

# Sample Questions

1. The below function attempts to format an integer, using commas as a thousands separator. Which test case would fail, indicating a bug?

{% highlight python %}
def format(num, sep=',');
    parts = []
    while num:
        num, mod = divmod(num, 1000)
        parts.append(f'{mod:03}')
    return sep.join(reversed(parts)) or '0'
{% endhighlight %}

choice

- assert format(0) == '0'
- assert format(100000) == '100,000'
- assert format(1000) == '1,000'
- assert format(100) == '100'

2. The below code implements the sieve of Eratosthenes algorithm for finding prime numbers. What is wrong with it?

{% highlight python %}
primes = Enumerator.new do |y|
    ints = (2..Float::INFINITY).lazy
    loop do
      prime = ints.next
      y << prime
      ints = ints.select { |i| i % prime > 0 }
    end
end
{% endhighlight %}

- It eventually raises a stack overflow error
- It only sieves even numbers
- It never terminates when called
- It doesn't output the first prime number

3. Assume there is an ORM with typical methods on its query object: all, count, exists, and first. Which code snippet has the most efficient query usage to determine if any matching row objects satisfy a predicate? Assume the and operator short-circuits.

- query.first() is not None and any(predicate(row) for row in query.all())
- query.count() and any(predicate(row) for row in query.all())
- query.exists() and any(predicate(row) for row in query.all())
- any(predicate(row) for row in query.all())

4. Which of the following algorithms is most suitable for binary classification?

- Simplex
- Linear regression
- Logistic regression
- K-means

5. What is the problem with executing the below SQL string?

{% highlight sql %}
"SELECT * FROM items WHERE id = '*%s*'" % id
{% endhighlight %}

- It has invalid syntax
- It does not quote its parameter
- It may be vulnerable to an injection
- It does not support integer ids

6. All of the below are common examples of probabilistic Monte-Carlo algorithms, with one-sided error and deterministic running time, except which one?

- randomized quicksort
- edge contraction to find a minimum cut in a graph
- bloom filters
- primality tests

7. The below code uses multiple inheritance to implement geometries. Fill in the missing line of code

{% highlight python %}
@dataclass(frozen=True)
class Parallelogram:
    width: float
    height: float
    angle: float

class Rhombus(Parallelogram):
    """A parallelogram with equal sides."""
    def __init__(self, side, angle):
        super().__init__(side, side, angle)

class Rectangle(Parallelogram):
    """A parallelogram with 90 degree angles."""
    def __init__(self, width, height):
        super.__init__(width, height, 90)

class Square(Rhombus, Rectangle):
    """A parallelogram with equal sides and 90 degree angles."""
    def __init__(self, side):
        #(Fill in the missing line here)
{% endhighlight %}

- super().__init__(side, 90)
- super(Square, self).__init__(side, 90)
- super(Rectangle, self).__init__(side, side)
- super(Rhombus, self).__init__(side, side)

8. When overwriting an existing file, what is a reason to first write to a temporary named file, and then use the system to rename it to the destination?
- To guarantee atomicity of the data written to the destination
- To save disk space
- To increase write performance
- To prevent permissions errors in opening the destination file

9. Given an array of hash sets, the task is to find the intersection of all the sets. Which implementation is both correct and the fastest asymptotically?

{% highlight python %}
fun <T> intersects(vararg sets: Set<T>): Set<T>{
    ...
}
{% endhighlight %}
- return sets.fold(sets.minBy({it.size})?:setOf()){r,s->r.intersect(s)}
- return sets.fold(setOf()){r,s->r.intersect(s)}
- return sets.fold(sets[0]){r,s->r.intersect(s)}
- return sets.reduce{r,s->r.intersect(s)}

10. Which of the following best describes the output of an "embedding"?
- A sparse,high-dimensional vector
- A basis vector
- A one-hot vector
- A dense,low-dimensional vector

