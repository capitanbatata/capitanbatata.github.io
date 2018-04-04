---
layout: post
title:  "My Haskell style guide"
date:   2018-03-24 23:40:00 +0100
categories: 
---

I'll present:

- Goals that a style guide should try to achive.
- Guidelines that help in reaching the goal.

Goal: readability is king. We spend more time reading code than writing it.
Every guideline should aim at making code more readable.

Minimize the column width.

# Case expressions

You'll get more space by writing this:

```haskell
f e ex = case ex of
    Nothing -> foo
    Just x  -> bar
```

instead of this:

```haskell
f e ex = 
    case ex of
        Nothing -> foo
        Just x  -> bar
```
