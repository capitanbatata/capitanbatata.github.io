---
layout: post
title:  "Haskell X Practices"
date:   2018-03-24 23:40:00 +0100
categories:
---

These might not best practices, but practices that made sense to me.

# Structuring applications with the `ReaderT` pattern

See [The `ReaderT` Design
Pattern](https://www.fpcomplete.com/blog/2017/06/readert-design-pattern). Main
takeaways:

- User `ReaderT` at the top level of your application.
- `StateT` does not make sense in code that does IO. If you have a mutable
  variable, use `IORef`, `TVar`, etc. This forces you to think about
  concurrency.
- Do not use `ExceptT` over `IO`.
- Define the `HasX` typeclases to decouple the functions that need parts of the
  environment. Notice how this corresponds with the principle of weakening the
  pre-conditons. If you say your function needs `Env`, this is a stronger
  pre-condition than if one would put the constraint `HasLogger e`.
- Regain purity by defining new type-classes (e.g. `MonadBalance`).


# Exception handling

Below I list some articles on this topic:

- [Exeptions Best Practices in Haskell](https://www.fpcomplete.com/blog/2016/11/exceptions-best-practices-haskell)
- [Defining Exceptions in Haskell](https://www.fpcomplete.com/blog/defining-exceptions-in-haskell)

Main takeaways:

- The first question to ask when defining the exception hand strategy for a
  function is: is this function dealing wit IO.
  - If dealing with IO, do not use things like `ExceptT` or `Either` which
    provide a false sense of security: by definition any `IO` action can fail,
    and they fail for different reasons.
  - If is a pure function then it might be fine to use multiple exception
    types.
- Using `String` (or similar types, like `Text`) for defining exceptions is
  plainly wrong.
- Using `MonadThrow` over `Either` will allow you to:
  - be composable
  - unify over maybe
  - although one would lose an information about the nature of the error being
    thrown.
- Use `MonadIO` and maybe `MonadMask` instead of `IO` to make IO related
  functions more general.
- GHC deals in a very convenient and pragmatic way with IO.
- You can even compose `MonadIO` and `MonadThrow` in different monads (getting
  a nested structure like `m (n a)`), where `MonadIO m` and `MonadThrow n`.
  Which one to use depends on the particular case at hand.
- There are several options to defining exceptions:
  - Mega exception types: where all the exceptions are defined in one data
    type.
  - Individual exception types: one can be more precise about the error type,
    but they do not compose well.
  - Abstract exception types: use an opaque type e.g. `IOError` which is only
    inspectable by accesors.
    - Big dissadvantage: is not self-documenting.

TODO: Additional reading [Trouble with Typed
Exceptions](https://www.parsonsmatt.org/2018/11/03/trouble_with_typed_errors.html).
