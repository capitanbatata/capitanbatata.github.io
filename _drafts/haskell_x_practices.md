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
