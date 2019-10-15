## Variances
```
class Foo[+A] // A covariant class
class Bar[-A] // A contravariant class
class Baz[A]  // An invariant class
```

Name|Remarks|Examples
---|---|---
Variance|型变|`Function[-T,+R]`
Covariant|协变|`Supplier[+A]`
Contravariant|逆变|`Consumer[-A]`
Nonvariant|不变|`Array[A]`
Immutable|不可变的|`String`
Mutable|可变的|`StringBuilder`

*	一般地，「不可变的」(`Immutable`)类型意味着「型变」(`Variant`)，而「可变的」(`Mutable`)意味着「不变」(`Nonvariant`)。
*	如果它是一个生产者，其类型参数应该是协变的，即`C[+T]`；
*	如果它是一个消费者，其类型参数应该是逆变的，即`C[-T]`。

```
trait Supplier[+T] {
	// 它生产T类型的实例
	def get: T
}

trait Consumer[-T] {
	// 它消费T类型的实例
	def accept(t: T): Unit
}

trait Function1[-T, +R] {
	// 入参类型为-T，返回值类型为+R
	// 对于参数类型，函数是逆变的，而对于返回值类型，函数则是协变的。
	def apply(t: T): R
}

// Mutable
final class Array[T](val length: Int) {
  def apply(i: Int): T = ???
  def update(i: Int, x: T): Unit = ???
}
```

### [结论](https://www.jianshu.com/p/fa0c67fe1bc6)
*	对于不可变的(Immutable)类型：C[-T, +R, S]
	* 	逆变(Contravariant)的类型参数T只可能作为函数的参数
	*  协变(Covariant)的类型参数R只可能作为函数的返回值
		*	当协变类型参数R出现在函数参数时，使用「下界」R1 >: R进行界定，将其转变为不变的(Nonvariable)类型参数R1；  
	*  不变的(Nonvariable)类型参数S则没有限制，即可以作为函数的参数，也可以作为返回值
		*	当逆变类型参数T出现在函数返回值时，使用「上界」T1 <: T进行界定，将其转变为不变的(Nonvariable)类型参数T1  

### Covariant
```
sealed abstract class List[+R]
// A is a subtype of B, then List[A] is a subtype pf List[B]
// 协变(Covariant)的类型参数R只可能作为函数的返回值
```

### Contravariant
```
class Write[-T]
// A is a subtype of B, then Write[B] is a subtype pf Write[A]
// 逆变(Contravariant)的类型参数T只可能作为函数的参数
```

```
abstract class Printer[-A] {
	def print(value: A): Unit
}

class AnimalPrinter extends Printter[Animal]{
	def print(animal: Animal): Unit =
    println("The animal's name is: " + animal.name)
}

class CatPrinter extends Printer[Cat] {
  def print(cat: Cat): Unit =
    println("The cat's name is: " + cat.name)
}

// Printer[Animal] hnow how to print Cat
```

### Invariance
*	Default
*	For data safe

### Option
```
sealed trait Option[+A]
case class Some[+A](get: A) extends Option[A]
case object None extends Option[Nothing]
```

### List
```
sealed trait List[+A] {
	def cons[A1 :> A](a: A1): List[A1] = Cons(a, this)
}

case class Cons[A](head: A, tail: List[A]) extends List[A]
case object Nil extends List[Nothing]
```
