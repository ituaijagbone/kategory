---
layout: docs
title: Semigroup
permalink: /docs/typeclasses/semigroup/
---

## Semigroup

A semigroup for some given type `A` has a single operation (which we will call `combine`), which takes two values of type `A`, and returns a value of type `A`. This operation must be guaranteed to be associative. That is to say that:

```kotlin
(a.combine(b)).combine(c)
```

must be the same as

```kotlin
a.combine(b.combine(c))
```

for all possible values of a, b ,c.

There are instances of `Semigroup` defined for many types found in Kategory and the Kotlin std lib. 
For example, `Int` values are combined using addition by default but multiplication is also associative and forms another `Semigroup`.

### Examples

Now that you've learned about the Semigroup instance for Int try to guess how it works in the following examples:

```kotlin:ank:silent
import kategory.*
```

```kotlin
semigroup<Int>().combine(1, 2)
//3
```

```kotlin
ListKW.semigroup<Int>().combine(listOf(1, 2, 3).k(), listOf(4, 5, 6).k())
//ListKW(list=[1, 2, 3, 4, 5, 6])
```

```kotlin
Option.monoid<Int>().combine(Option(1), Option(2))
//Some(value=3)
```

```kotlin
Option.monoid<Int>().combine(Option(1), Option.None)
//None
```

Many of these types have methods defined directly on them, which allow for such combining, e.g. `+` on `List`, but the value of having a `Semigroup` typeclass available is that these compose.

There is also extension syntax available for Semigroup.

```kotlin
listOf(Option(1), Option(2)).combineAll(Option.semigroup<Int>())
//Some(value=3)
```

```kotlin
listOf(1, 2, 3).combineAll()
//6
```

```kotlin
listOf("K", "Λ", "T", "E", "G", "O", "R", "Y").combineAll()
//KΛTEGORY
```

Contents partially adapted from [Scala Exercises Cat's Semigroup Tutorial](https://www.scala-exercises.org/cats/semigroup)