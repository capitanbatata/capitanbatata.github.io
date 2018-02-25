---
layout: post
title:  "Lenses don't compose backwards: an example"
date:   2018-02-25 23:40:00 +0100
categories: lenses
---

I was always puzzled by the way lenses seemed to compose backwards. And
apparently I'm not the only one, as [this question on Stack
Overflow][so-question] shows. What really helped me to understand why lenses
compose the way they do was [this Reddit post][reddit-post], from which I
stole the title for this blog post.

As a future reference for myself, I'm writing this example, which uses
the `Atom` and `Point` data-types from the [lens tutorial][lens-tutorial]:

{% highlight haskell %}
data Atom = Atom { _element :: String, _point :: Point }

data Point = Point { _x :: Double, _y :: Double }
{% endhighlight %}

Suppose we have a value `a` of type `Atom`. If we want to access value of the
`x` coordinate of its `Point`, we can readily do this by using accessors:

{% highlight haskell %}
_x . _point $ a
{% endhighlight %}

However, if we use the lenses `x` and `point`, we access the value of `x` as
follows:

{% highlight haskell %}
a ^. point . x
{% endhighlight %}

How is that possible? Do lenses compose backwards?

The main thing to remember about lenses and their composition is that:

> Lenses are not accessors

If we look at the type of the accessors above we have:

{% highlight haskell %}
_x          :: Point  -> Double
--             --v--     ---v--
--     Super-structure  Sub-Structure
_point      :: Atom   -> Point
--             --v--     ---v--
--     Super-structure  Sub-Structure
(.)         :: (b     ->  c    ) -> (a    ->     b) -> (a    ->      c)
(.)         :: (Point -> Double) -> (Atom -> Point) -> (Atom -> Double)
_x . _point ::                                          Atom -> Double
{% endhighlight %}

However, if we look at the (simplified) type of the lenses we have:

{% highlight haskell %}
x     :: (Double -> f Double) -> (Point -> f Point)
--       ---------v----------    ---------v-------
--          Sub-structure         Super-structure
point :: (Point  -> f Point ) -> (Atom -> f Atom  )
--       ---------v----------    ---------v-------
--          Sub-structure         Super-structure
{% endhighlight %}

The only type in common that `x` and `point` have is `Point -> f Point` so if
we want to use function composition, we have to apply the `x` first, and then
`point`:

{% highlight haskell %}
(.) :: (b      ->  c     ) 
    -> (a      ->  b     ) 
    -> (a      ->  c     )
(.) :: ((Point -> f Point) -> (Double -> f Double)) 
    -> ((Atom  -> f Atom ) -> (Point  -> f Point))
    -> ((Atom  -> f Atom ) -> (Double -> f Double))
{% endhighlight %}

So we see that when composing lenses, we start from the function that focuses
on the innermost element we want to access (`x` in this case), and we work our
way up from there: first `Point` then `Atom`. Contrast this with the way the
accessor works: first we apply the accessor for `Atom`, then for `Point`.

If we understand that lenses are not accessors and we understand their type,
then it is easier to see that it is only natural that they compose the way they
do.

[lens-tutorial]: https://hackage.haskell.org/package/lens-tutorial-1.0.3/docs/Control-Lens-Tutorial.html
[so-question]: https://stackoverflow.com/questions/23092468/lenses-composing-backwards-and-in-lens-context
[reddit-post]: https://www.reddit.com/r/haskell/comments/23x3f3/lenses_dont_compose_backwards/
