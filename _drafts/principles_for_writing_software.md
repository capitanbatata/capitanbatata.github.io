Some thoughts on principles I find important when developing software.

# Don't rush

Take your time to code a solution. Otherwise:

- You won't enjoy the process.
- Errors are likely to happen.

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
