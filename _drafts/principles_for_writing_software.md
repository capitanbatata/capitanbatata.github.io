Some thoughts on principles I find important when developing software.

# Don't rush

Take your time to code a solution. Otherwise:

- You won't enjoy the process.
- Errors are likely to happen.

# Focus

Put your phone in silent mode. Or in the fridge. Use the pomodoro technique or
whatever helps you keep focused. 

# Be consistent

- In the terminology used.
- In the way modules are assembled together.

This makes things easier to find and to predict. Think of ruby and its minimum
surprise principle.

# Write code as if it was the final version

You think you'd go back and fix something. You won't.

# Allocate time to fix technical debt

In case you did not write the code as if it was the final version.

# Invariants of data and smart constructors

Not everything can be captured in the type-system. For this use smart
constructors that enforce invariants on data.

# Strive for simplicity

Just because you learned a new cool and very smart technique, it does not mean
that it will be the right tool for the job. This might end up complicating the
code-base unnecessarily. Whenever you learn something new, make sure you
understand the right context to use it. The advantages but also the
disadvantages. What are the trade-offs. Remember: perfection is not achieved
when there is nothing to add, but when there's nothing to remove.

An example of preferring smart over simple might be the use of
[free-monads](https://markkarpov.com/post/free-monad-considered-harmful.html).
