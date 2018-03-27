You can convert a `Either a b` into an `Either a' b'` using `+++`:

```haskell
(+++) :: a b c -> a b' c' -> a (Either b b') (Either c c')
```

Examples:
```haskell
show +++ length $ Left 1
show +++ length $ (Right "bar" :: String)
```

And you can merge and extract the either using `|||`:

```haskell
(|||) :: a b d -> a c d -> a (Either b c) d
```

When we take the type `a` to be a function (`->`) we have:

```haskell
(|||) :: (b -> d) -> (c -> d) -> ((Either b c) -> d)
```

Try it out:
```haskell
(+2) ||| length $ Right ("Bar" :: String)
```

```haskell
(+2) ||| length $ Left 1
```
