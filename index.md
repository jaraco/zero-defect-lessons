# Lessons from Zero-Defect Software

## Jason R. Coombs

Note: Welcome to (conference, forum)... My name is Jason and I'll be talking today about engineering zero-defect software.

---

# Intro and Background

- Computer Science
- Scheme (functional programming)
- Zero-Defect Software

...

## Passion

![Passion Fruit Image](https://saporitoovs.com/wp-content/uploads/2017/08/passionfruit.jpg)

...

## Passion

- Reliability
- Re-use
- Sophistication

Note: I hate repeating myself. After several decades of hacking and seeking a greater capability for programming, I've encountered lessons and accumulated patterns that work well for me.

...

## Introspection

Note: Inspired by a tweet a few years ago, I reflected on the foundations of my technique that have led to an ability to rapidly develop robust software.

---

# Distilled Inspiration

...

## Refactoring

...

## Functional Programming

...

## Zero-Defect Software

Note: Finally, there are techniques from Zero-Defect software.

...

## Honorable Mentions

- Object-oriented Programming
- Test/Documentation/Behavior-Driven-Developmpent
- Static analysis tools
- Agile (fail fast)
- Performance

Note: Static analysis tools (linters, style checkers, IDEs) are a powerful way to avoid common mistakes before even saving a file. In this talk, I'll be focused on optimizing for correctness of design and disregarding the impact of performance. Often this tradeoff is a good one to make in practice too, as readability matters and premature optimization is a fools errand.
...

## Examples

- Dredged my [first production code](https://github.com/jaraco/esdsort/commit/0e69e0f6687648cb61f50840e25c81e6f57cca61) (c. 1992).
- C [ported to Python](https://github.com/jaraco/esdsort/commit/1e7d063ff1101d9d3b91d861f5a1a0a5efde51e3).

---

![](https://images-na.ssl-images-amazon.com/images/I/51K-M5hR8qL._SX392_BO1,204,203,200_.jpg)

_The single book that most influenced my programming for the better_

...

## Implementation as Spec

- Inherited
- Best source of truth
- Assumptions baked-in
- Latent institutional knowledge
- Legacy behaviors

...

## DRY

- Factor the code
- Essential functionality

Notes: What does "factor" mean? It's a piece of behavior, an aspect of the program. It may be a semantic construct like a interface or a concrete one like an object or function. "Essential" - what it's intended to do and not more (constraints and limits can empower)

...

## Tools

- Eclipse IDE
- JetBrains/PyCharm
- Manual
- consider committing each stable change

...

## Refactor to

- learn a new codebase
- reshape to match conceptual needs
- keep functions small

Notes: For learning a new codebase, make safe operations as you mold the codebase to your expertise (esp. if you're sole maintainer). For small functions, if it can fit on the screen, it might fit in your mind.

...

## Examples


---

# Functional Programming

...

## What is functional programming?

- not what you were taught
- not a set of instructions
- expressions over statements
- all state is localized

...

## Benefits

- Stateless
  - no side effects
  - no interactions except at input/output
- Focus on what machines do best
  - perform an operation over a (possibly unlimited) number of inputs
- Generalizable abstraction
  - Simple concepts are more scalable
  - Map/reduce forms foundation of Hadoop

...

## Comprehensions

```python
items = []
for item in inputs:
    items.append(handle(item))
```

Note: Consider this very common pattern.

...

## Comprehensions

Operate on expressions:

```python
items = [handle(item) for item in inputs]
```

Note: This syntax is shorter, but that's not it's key benefit. The key benefit is that the syntax is mainly one expression applied to an iterable. This code communicates explicitly that there's no manipulation of inputs and exactly one operation on each item. It further reduces the temptation to inject such complicating behaviors (for the same reasons you'd avoid "goto". "handle" might be arbitrarily complex, but that complexity is encapsulated by its signature.

...

## Keep Functions Simple

- one output parameter
- few input parameters (ideally one)
- rely on exceptions for handling exceptional conditions
- prefer single-word actions
  - `string.lower(input)` > `string.string_lowercase_input(input)`
  - `dedent(lstrip(input))` > `dedent_and_lstrip(input)`

Note: Here's where Python really shines - there's (preferably) one outcome from a function (its prime directive). Languages like Go require exceptional conditions to be handled in-band. Java features like ".." require each and every exception to be handled explicitly. Many functions can be represented with single word actions. If there are multiple words, especially if the words combine more than a single concern, suggests the function may be doing too much (or unintentionally dragging in the parent scope or parameters).

...

## Primitives

- map
- reduce
- compose
- partial

Note: Functional paradigm brings several primitive concepts that apply to many use cases, including map, reduce, compose, and partial.

## Map and Reduction

- map: operation on a series of inputs producing series of outputs
- reduce: given a series of inputs, aggregate into a single output

Note: map is perhaps the most essential computatal operation

...

## Composition

Given two or more functions, produce a new function that takes the result from one function as the input to the other. Consider f(x) = x<sup>2</sup> + 1, decomposes to

- f1(x) = x + 1
- f2(x) = x<sup>2</sup>

`f(x) = f2(f1(x))` or `f(x) = f2∘f1(x)`

...

## Composition (Python)

```python
def f1(x):
    return x + 1

def f2(x):
    return x ** 2

from jaraco.functools import compose

f = compose(f2, f1)
```

Note: Now f1 and f2 can be independently verified and tested, perhaps used separately elsewhere, and the composition of those two is "f". Pragmatically, you probably wouldn't do this for a simple mathematical function as illustrated here (especially if you're using CPython which has high function call overhead).

...

## Partial

```python
def add_n(x, n):
    return x + n

f1 = partial(add_n, n=1)
```


Note: The last powerful primitive that I'll cover is the "partial", a way of binding state to a function.

...

## Partial (compared to OO)

```python
class Adder:
    def __init__(self, addend):
        self.addend = addend

    def __call__(self, addend):
        return addend + self.addend

f1 = Adder(1)
```

Note: Functional programming tends to use less indendation and has more private state (here the 'Adder.addend' is readily inspectable).

...

## Boolean Parameters

```python
def read_file(filename, use_bytes=True):
    if use_bytes:
        with open(filename, 'rb') as reader:
            return reader.read()
    else:
        with open(filename, 'r') as reader:
            return reader.read()
```

Note: If you have a function accepting a boolean parameter, especially if that boolean parameter is only observed at the beginning or end of the function or if the boolean parameter selects behavior for the entire body of the call, it's probably better factored out into separate functions.

...

## Boolean Parameters (separate functions)

```python
def read_text(filename):
    with open(filename, 'r') as reader:
        return reader.read()

def read_bytes(filename):
    with open(filename, 'rb') as reader:
        return reader.read()
```

...

## Deduplicate

```python
def read_text(filename):
    return read(filename, 'r')

def read_bytes(filename):
    return read(filename, 'rb')

def read(filename, mode):
    with open(filename, mode) as reader:
        return reader.read()
```

Note: Now that we've extracted the duplicating code, we observe that filename and mode are passed directly into another function - another bad smell. Instead, it would be better to rely on the canonical interface.

...

## TBD

```python
def read(stream):
    """
    To read text, ``read(open(filename, 'r'))``
    """
    with stream:
        return stream.read()
```

Note: Rely on canonical interfaces wherever possible. Avoid passing parameters through.

---

# Zero Defect Software

...

## Levels of Rigor

- Mathematically provable
  - brute force
  - property-based testing
- Verifiably correct code
- Code written for verifiability

## Cleanroom Development

- Careful specification of functions, components, and control constructs
  - clarity
  - precision
  - completeness
- Catch many errors before compile/execution time
- Understand the program thoroughly
- Rigorous reviews

## Practice

- Only the most simple code is mathematically provable
- Only simple code is verifiable
- Limit relevant state, interactions
- Declare and enforce constraints (inputs, outputs)
- Use refactoring, functional programming to safely simplify the code

...

## Legacy code

- Code is the spec

Note: Often with legacy code, there is no specification. The code is the spec. Derive a spec.

## Expectations

- Slower time to initial release
  - Faster (or comparable) development lifecycle
- Low level of defects during testing
- Few defects in production

Note: A high level of defects during testing indicates failure in the process. Revisit and bolster the review process.

...

## Limiting State

- Avoid if statements and other branching logic
- 'if' is the new 'goto'
  - avoid them; there's probably a better way

Note: Limiting branching logic reduces the number of states the subprogram can be in and thus the number of combinations that need to be considered.

...

## Examples

```python
def main():
    # ... (no branches)
    try:
        open("esdsort.dat", 'w')
    except Exception:
        pass
    # ...
```

- infer the purpose
- derive a spec
- extract a function

...

## Truncate output file

```python

OUTPUT_FILENAME = "esdsort.dat"


def truncate(filename):
    """
    Unconditionally ensure the named file exists and is truncated.
    """
    try:
        open(filename, 'w')
    except Exception:
        pass


def main():
    # ... (no branches)
    truncate(OUTPUT_FILENAME)
    # ...
```

Notes: In this change, I've now made explicit and apparent the purpose of that segment of code. This change also makes more explicit the specification that the output file will be truncated during startup. The code is still the spec, but it reads more like a spec. I've also extracted a variable for the output filename, an operation that probably should be done in a separate commit and affecting all hard-coded usages of that name. Now we can perform a rigorous review of this new truncate behavior. Can you imagine any

...

## Optimize

```python

def truncate(filename):
    """
    Unconditionally ensure the named file exists and is truncated.
    """
    open(filename, 'w').close()
```

Note: Now that we have a spec (which doesn't say anything about suppressing errors), optimize the function to follow best practices for Python, namely let exceptions bubble up and close file handles.

---

# Conclusions

All together, these approaches

- Reduce interactions,
- Reducing complexity.
- Elegance begets simplicity.
- Python shines
  - functional, OO, imperative where appropriate
  - robust patterns for exception handling

---

# Q&A
