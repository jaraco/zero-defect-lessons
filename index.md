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
- Agile (fail fast)

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

1. Functional Programming
    1. What is the functional programming paradigm?
        1. Not what you were taught (set of instructions)
    1. Why is it beneficial?
        1. Stateless; no side effects
        1. Focus on what machines do best - perform an operation over a number of inputs
        1. Simple concepts are more scalable (map/reduce forms foundation of Hadoop)
    1. Compare for loop to list comprehension
    1. Limit parameters of functions (one input, one output ideal)
    1. Avoid boolean parameters (small change doubles states)
        1. Consider separate functions
    1. Examples:
        1. map
        1. reduce
        1. compose
1. Zero-defect Software
    1. Levels of rigor
        1. Mathematically-provable code
            1. brute-force all possible inputs
        1. Verifiably correct code
        1. Code written for verifiability
    1. Only the most simple code is mathematically provable; only simple code is verifable.
        1. Compose more complex functionality from verifiably-correct components.
        1. Limit the surface of the components.
        1. OO programming can help here to encapsulate state
    1. Use refactoring and functional programming principles to simplify the code safely
    1. Avoid if statements or other branching logic (number of states the sub program can be in)
        1. Think of 'if' statements like gotos. Avoid them - there's probably a better way
    1. Examples and case studies
        1. Examine a number of case studies drawn from real-world commits
1. Conclusion
    1. What is the common factor of all of this?
    1. Reducing interactions
    1. Reduces complexity
    1. Elegance begets simplicity
    1. Python is particularly well-suited to adopt functional, OO, or imperative paradigms when appropriate
