## "Hello World"

```scala
object Program extends App {
    println("Hello, World!")
}
```

## S-interpolation

```scala
val city = "Barcelona"
val year = 2020
val pi = 3.1415

val cityFormatted = s"City: $city" // City: Barcelona
val yearFormatted = s"Year: $year" // Year: 2020
val piFormatted = s"Pi: $pi" // Pi: 3.1415
```

## F-interpolation

```scala
val city = "Barcelona"
val year = 2020
val pi = 3.1415

// Statically typed
val cityFormatted = f"City: $city%s" // City: Barcelona
val yearFormatted = f"Year: $year%d" // Year: 2020
val piFormatted = f"Pi: $pi%05.2f" // Pi: 03.14
```

## Raw strings

```scala
// Does not interpret '\n' as a new line character
val raw = raw"Take escape characters literally \n"
```

## "For" loop

```scala
for (n <- 1 to 10) println(n)
```

## Pattern matching

```scala
val x = Option(42)

val s = x match {
    case None => "No value"
    case Some(x) => s"Value of $x.toString"
} // Some(42)
```

## Function

```scala
def add(a: Int, b: Int): Int = {
    a + b
}
val sum = add(5, 3)
```

## Lambda

```scala
val add = (x: Int, y: Int) => x + y
val sum = add(5, 3)
```

## OOP Class (avoid)

```scala
object Person {
    // if you want to instantiate w/o "new", you have to implement manually
    def apply(firstName: String, lastName: String, age: Int) =
        new Person(firstName, lastName, age)
}

// use var for mutable
class Person(val firstName: String, val lastName: String, val age: Int) {
    def isAdult: Boolean = age > 21

    // no default implementation for toString or equals/hashCode,
    // inherits from Any
}

val p = Person("User", "Random", 42)
```

## Case class (Products)

```scala
// Implementats copy, toString, equals/hashCode; supports pattern matching
case class Person(firstName: String, lastName: String, age: Int) {
    def isAdult: Boolean = age > 21
}

val p = Person("User", "Random", 42)
```

## Union types (Coproducts)

```scala
sealed trait Maybe[+A] {
    def map[B](f: A => B): Maybe[B]

    def flatMap[B](f: A => Maybe[B]): Maybe[B]
}

object Maybe {
    def of[A](value: A) = Some(value)
}

case object None extends Maybe[Nothing] {
    override def map[B](f: Nothing => B): Maybe[B] = this

    override def flatMap[B](f: Nothing => Maybe[B]): Maybe[B] = this
}

case class Some[+A](value: A) extends Maybe[A] {
    override def map[B](f: A => B): Maybe[B] = Some(f(value))

    override def flatMap[B](f: A => Maybe[B]): Maybe[B] = f(value)
}

val x = Maybe.of(5)
val y = x.map(_ * 2)
```

## Type classes

```scala
// type class, not to to be OOP-inherited
trait Animal[A] {
    def breed(a: A): List[A]
}

// "combines" type class implementations with classes conforming to a type class
implicit class AnimalOps[A](animal: A) {
    def breed(implicit animalTypeClassInstance: Animal[A]): List[A] =
    animalTypeClassInstance.breed(animal)
}

// does not OOP-implement the type class
class Cat
object Cat {
    // type class implementation for Cat
    implicit object CatAnimal extends Animal[Cat] {
    override def breed(a: Cat): List[Cat] = ???
    }
}

val cat = new Cat
val cats: List[Cat] = cat.breed
```

## Higher-kinded classes

```scala
trait Monad[F[_], A] {
    def map[B](f: A => B): F[B]
    def flatMap[B](f: A => F[B]): F[B]
}

implicit class ListAsMonad[A](list: List[A]) extends Monad[List, A] {
    override def map[B](f: A => B): List[B] = list.map(f)
    override def flatMap[B](f: A => List[B]): List[B] = list.flatMap(f)
}

def cartesianProduct[F[_], A, B](ma: Monad[F, A], mb: Monad[F, B]): F[(A, B)] =
    for {
        a <- ma
        b <- mb
    } yield (a, b)

val ma = List(1, 2, 3)
val mb = List("a", "b")

println(cartesianProduct(ma, mb))
```