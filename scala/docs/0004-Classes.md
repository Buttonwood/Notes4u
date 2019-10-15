## Classes
### 1.Simple Classes and Parameterless Methods
```
class Counter {
	// 类中可见
	private var value = 0
	
	// Methods are `public` by default
	def increment() {
		value + = 1
	}
	
	def current() = value
	
	// toString
	override def toString = ""
}

val myCounter = new Counter()
myCounter.increment() // Use `()` with mutator
println(myCounter.current) // Don't use `()` with accessor
```

*	一个scala文件可包含多个class; class无需声明为`public`
* 	方法默认为`public`
*	无参数的mutator method(改变状态)的方法后需要带上`()`
* 	无参数的accessor method(不改变状态)的方法后不需要`()`;定义时即可不带
*  method parameters in scala are `vals`, not `vars`: 不能被重新赋值

### 2. Properties with Getters and Setters
```
class Person {
	var age = 0
}

var fred = new Person()

// getter:  Calls fred.age()
println(fred.age)

// setter: Calls fred.age_=(21)
fred.age = 21
```

*	If the field is `private`, the getter and setter are `private`;
* 	If the field is `val`, only a getter is generated;
*  If you don't want any getter or setter, declare the field as a Object-Private field `private[this]`;
*  `private[this] var value = 0 // aObject.value is not allowed`

```
class Counter {
	private var value = 0
	
	def increment() {
		value += 1
	}
	// As Getter
	def current =  value
}
```

#### Summary
*	`var foo`:  Scala synthesizes a getter and a setter.
* 	`val foo`:  Scala synthesizes a getter, but no setter.
*  `private var foo = 0`: no getter 
*	You define methods `foo` and `foo_=`.
*	`private[this] var value = 0 // aObject.value is not allowed` called ***Object-private***, no getter, no setter
*	Parameters without `val` or `var` are `private` values, visible only within the class.

```
class Point(x: Int, y: Int)

val p = Point(1,2)
p.x // <-- does not compile
```

### 3. Bean Properties
*	`getFoo`
* 	`setFoo`

```
import scala.beans.BeanProperty

class Person {
	@BeanProperty var name: String = _ 
}

```
Generates four methods:

1.	`name:String`
2. `name_=(newValue:String):Unit`
3. `getName():String`
4. `setName(newValue:String):Unit`

![](https://note.youdao.com/yws/public/resource/faac1e243a45e8aaea557e652f373246/xmlnote/B8C7B96221244AFA9F1768BEB0B73073/15956)

```
var|val name
@BeanProperty var|var name
private var|val name
private[this] var|val name
private[ClassName] var|val name
```

```
class Person(@BeanProperty var name: String)
```

### 4. Constructor
#### 4.1 The Primary Constructor
*	The primary constructor is not defined with a `this` method. Instead, it is interwoven with the class definition.
*	Parameters of the primary constructor turn into fields that are initialized with the construction parameters. 
* 	The primary constructor executes all statements in the class definition.

```
class Person(val name: String, val age: Int) {
	println("Just constructed another person")

	def description = s"$name is $age years old"
}
```

```
class MyProg {
	private val props = new Properties
	props.load(new FileReader("myprog.properties"))
	// ...
}
```

```
class Person(@BeanProperty var name: String){
	//
}
```

*	If there are no parameters after the class name, then the class has a primary constructor with no parameters. 

*	Eliminating auxiliary constructors by using default arguments in the primary constructor

```
class Person(val name: String = "", val age: Int = 0) { }
```

*	To make the primary constructor private, place the keyword `private`

```
class Person private(val id: Int) { }
```

*	Fields and Methods Generated for Primary Constructor Parameters

![](https://note.youdao.com/yws/public/resource/faac1e243a45e8aaea557e652f373246/xmlnote/DDB2A827EA2A45DA8EB4B53734F9FC7D/15959)

#### 4.2. Auxiliary Constructors (`this()`)
```
class Person {
	private var name = ""
	private var age = 0
	
	// An auxiliary constructor
	def this(name: String){
		// Calls primary constructor
		this()
		this.name = name
	}
	
	// Another auxiliary constructor
	def this(name: String, age: Int) {
		// Calls previous auxiliary constructor
		this(name)
		this.age = age
	}
}
```

### 5. Nested Classes
```
class Outer(val name: String) { outer =>
	class Inner(val name: String) {
		//
		def desc = s"$name inside ${outer.name}"
	}
}
```


### 6. Abstract Class
```
// can not instantiate an abstract class
abstract class Element {
	// parameterless methods
	def contents: Array[String]
	
	def height: Int = contents.length
	
	def width: Int = if(height == 0 ) 0 else contents(0).length
}

class ArrayElement(conts: Array[String]) extends Element {
	def contents: Array[String] = conts
}
```


### Examples
```
class Rational(n: Int, d: Int) {
	require(d != 0)
	private val g = gcd(n.abs, d.abs)
	val numer = n/g
	val denom = d/g
	
	def this(n: Int) = this(n,1)
	
	def add(that: Rational): Rational = {
		new Rational(
			numer* that.denom + that.numer*denom,
			denom * that.denom
		)
	}

	override def toString = numer + "/" + denom
	
	private def gcd(a: Int, b: Int): Int = {
		if(b == 0) a else gcd(b, a % b)
	}
}
```


```
class Cat {
	val dangerous = false
}

class Tiger(d: Boolean, a: Int) extends Cat {
	// override fields
	override val dangerous = d
	private var age = a
}
```
