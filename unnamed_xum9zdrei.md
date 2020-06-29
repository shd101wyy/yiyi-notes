---
tags:
  - pl
  - pl/haskell
created: 2020-06-15T09:10:23.073Z
modified: 2020-06-15T09:31:54.433Z
---

# Haskell

> http://learnyouahaskell.com/a-fistful-of-monads

**functor**

```haskell
class Functor f where
    fmap :: (a -> b) -> f a -> f b

(<$>) :: (Functor f) => (a -> b) -> f a -> f b
f <$> x = fmap f x
```

example:

```haskell
ghci> fmap (++"!") (Just "wisdom")
Just "wisdom!"
ghci> fmap (++"!") Nothing
Nothing
```

**applicative functor**

```haskell
class (Functor f) => Applicative f where
    pure :: a -> f a
    (<*>) :: f (a -> b) -> f a -> f b
```

example:

```haskell
ghci> max <$> Just 3 <*> Just 6
Just 6
ghci> max <$> Just 3 <*> Nothing
Nothing
```

**monoid**

A monoid is when you have an associative binary function and a value which acts as an identity with respect to that function. When something acts as an identity with respect to a function, it means that when called with that function and some other value, the result is always equal to that other value. 1 is the identity with respect to \* and [] is the identity with respect to ++. There are a lot of other monoids to be found in the world of Haskell, which is why the Monoid type class exists. It's for types which can act like monoids. Let's see how the type class is defined:

```haskell
class Monoid m where
    mempty :: m
    mappend :: m -> m -> m
    mconcat :: [m] -> m
    mconcat = foldr mappend mempty
```

**monad**

```haskell
class Monad m where
    return :: a -> m a

    (>>=) :: m a -> (a -> m b) -> m b

    (>>) :: m a -> m b -> m b
    x >> y = x >>= \_ -> y

    fail :: String -> m a
    fail msg = error msg
```

`>>=` is pronounced as _bind_

example:

```haskell
ghci> return "WHAT" :: Maybe String
Just "WHAT"
ghci> Just 9 >>= \x -> return (x*10)
Just 90
ghci> Nothing >>= \x -> return (x*10)
Nothing
```
