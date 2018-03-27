
This looks esoteric:

```haskell
(***) :: a b c -> a b' c' -> a (b, b') (c, c') 
```

But if we take `a` to be the function application:

```haskell
(***) :: (b -> c) -> (b' -> c') -> ((b, b') -> (c, c'))
```

Try it out!

```haskell
(+1) *** (++" bar") $ (0, "foo")
```
