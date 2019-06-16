1. Introduction
    1. My background
        1. computer science
        1. scheme (functional)
        1. zero-defect software
    1. Passion for reliability, re-use, sophistication
    1. Introspect how to come to elegant techniques
        1. Not all my techniques are elegant
        1. It's a joy when they are
    1. Try to distill some examples from elegant code to principles that led to them.
        1. Refactoring
        1. Functional programming
        1. Zero-defect software
    1. What about testing? This talk avoids talking about tests, which are still practical and necessary, but aims to build reliable code without relying on tests
1. Refactoring
    1. From Martin Fowler's book of the same title.
        1. The one book that made me a better programmer more than any other.
    1. Rely on implementation as the spec (assuming correct, restructure without altering semantic meaning)
    1. DRY - factor the code such that the code does what it's intended to and not more
    1. Use automated tools
        1. PyCharm
        1. Or don't, but follow the principles: commit between refactoring steps, even if you squash later (but please don't)
    1. Reshape the code to match the conceptual needs
    1. Keep functions small (fit on the screen might fit in your mind)
    1. Examples
        1. Extract function/method
        1. init/for/append -> single assignment
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
