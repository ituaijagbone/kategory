---
layout: docs
title: SequenceK
permalink: /docs/datatypes/sequencek/
---

## SequenceK

SequenceK implements lazy lists representing lazily-evaluated ordered sequence of homogenous values.

It can be created from Kotlin Sequence type with a convenient `k()` function.

```kotlin:ank
import arrow.*
import arrow.data.*

sequenceOf(1, 2, 3).k()
```

SequenceK derives many useful typeclasses. For instance, it has a [`SemigroupK`](/docs/typeclasses/semigroupk/) instance.

```kotlin:ank
val hello = sequenceOf('h', 'e', 'l', 'l', 'o').k()
val commaSpace = sequenceOf(',', ' ').k()
val world = sequenceOf('w', 'o', 'r', 'l', 'd').k()

hello.combineK(commaSpace.combineK(world)).toList() == hello.combineK(commaSpace).combineK(world).toList()
```

[`Functor`](/docs/typeclasses/functor/)

Transforming a sequence:
```kotlin:ank
val fibonacci = generateSequence(0 to 1) { it.second to it.first + it.second }.map { it.first }.k()
fibonacci.map { it * 2 }.takeWhile { it < 10 }.toList()
```

[`Applicative`](/docs/typeclasses/applicative/)

Applying a sequence of functions to a sequence:
```kotlin:ank
SequenceK.applicative().run { sequenceOf(1, 2, 3).k().ap(sequenceOf({ x: Int -> x + 1}, { x: Int -> x * 2}).k()).toList() }
```

SequenceK is a [`Monad`](/docs/_docs/typeclasses/monad/) too. For example, it can be used to model non-deterministic computations. (In a sense that the computations return an arbitrary number of results.)

```kotlin:ank
import arrow.typeclasses.*

val positive = generateSequence(1) { it + 1 }.k() // sequence of positive numbers
val positiveEven = positive.filter { it % 2 == 0 }.k()

SequenceK.monad().binding {
   val p = positive.bind()
   val pe = positiveEven.bind()
   p + pe
}.fix().take(5).toList()
```

Folding a sequence,

```kotlin:ank
sequenceOf('a', 'b', 'c', 'd', 'e').k().foldLeft("") { x, y -> x + y }
```

## Available Instances

* [Show]({{ '/docs/typeclasses/show' | relative_url }})
* [Eq]({{ '/docs/typeclasses/eq' | relative_url }})
* [Applicative]({{ '/docs/typeclasses/applicative' | relative_url }})
* [Foldable]({{ '/docs/typeclasses/foldable' | relative_url }})
* [Functor]({{ '/docs/typeclasses/functor' | relative_url }})
* [Monad]({{ '/docs/typeclasses/monad' | relative_url }})
* [Monoid]({{ '/docs/typeclasses/monoid' | relative_url }})
* [MonoidK]({{ '/docs/typeclasses/monoidk' | relative_url }})
* [Semigroup]({{ '/docs/typeclasses/semigroup' | relative_url }})
* [SemigroupK]({{ '/docs/typeclasses/semigroupk' | relative_url }})
* [Traverse]({{ '/docs/typeclasses/traverse' | relative_url }})
* [TraverseFilter]({{ '/docs/typeclasses/traversefilter' | relative_url }})
* [Index]({{ '/docs/optics/index' | relative_url }})
* [FilterIndex]({{ '/docs/optics/filterindex' | relative_url }})
