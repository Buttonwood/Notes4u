## Functions
*	A method operates on an object, but a function doesn’t. 
* 	C++ has functions as well, but in Java, you have to imitate them with static methods.

### Functional programming
*	Functional programming aims to avoid side effects.
*	Functional programming avoids concepts such as state.
* 	Functional programming promotes immutable data.
*	Functional programming promotes declarative programming.

### Advantages to Functional Programming
*	Less code.
* 	Lack of (hidden) side effects (Referential Transparency).
*	Recursion is a natural control structure.

### Disadvantages of Functional Programming
*	Input–output is harder in a purely functional language(Good for stram style processing).
* 	Interactive applications are harder to develop.
*	Continuously running programs such as services or controllers may be more difficult to develop, as they are naturally based upon the idea of a continuous loop.
* 	Functional programming languages have tended to be less efficient on current hardware platforms.
*  Not data oriented.

###  Function define
*	Specify the function’s name, parameters, and body
* 	You must specify the types of all parameters.

```
def abs(x: Double) = if(x > 0) x else -x
```

```
def fac(n: Int) = {
	var r = 1
	for(i <- 1 to n) r = r * i
	r
}
```

*	With a recursive function, you must specify the return type. 

```
def fac(n: Int): Int = if(n<=0) 1 else n*fac(n-1)
```

#### Default and Named Arguments
*	Named arguments can make a function call more readable. 
* 	They are also useful if a function has many default parameters.

```
def join(str: String, left: String = "[", right: String = "]") = 
	left + str + right
	
join("hello")
join("hello","<<<")
join("hello","<<<",">>>")
join(str="hello",right=">>>")
join(str="hello",left="<<<",right=">>>")
```

#### Variable Arguments
```
def sum(args: Int*) = {
	var result = 0
	for(arg <- args) result += arg
	result
}

val s = sum(1,2,3,4,5);
val s = sum(1 to 5: _*);//Consider 1 to 5 as an argument seuquence

def recursiveSum(args: Int*): Int = {
	if(args.length = 0) 0
	else args.head += recursiveSum(args.tail: _*)
}
```

#### Procedures
*	Returns no value, and you only call it for its side effect.
* 	The return type is `Unit`

```
def box(s: String) {
	val border = "-" * (s.length + 2)
	print(f"$border%n|$s|%n$border%n")
}

// explicit return type
def box(s: String) : Unit = {
	val border = "-" * (s.length + 2)
	print(f"$border%n|$s|%n$border%n")
}
```

#### Lazy Values
*	When a `val` is declared as `lazy`, its initialization is deferred until it is accessed for the first time.
* 	Laziness is not cost-free. Every time a `lazy` value is accessed, a method is called that checks, in a threadsafe manner, whether the value has already been initialized.

```
// Evaluated as soon as defined
val words = scala.io.Source.fromFile("/usr/share/dict/words").mkString

// Evaluated the first time is used
lazy val words = scala.io.Source.fromFile("/usr/share/dict/words").mkString

// Evaluated every time is used
def words = scala.io.Source.fromFile("/usr/share/dict/words").mkString
```

### Anonymous functions
```
(x: Double) => 3 * x
val triple = (x: Double) => 3 * x

def triple(x: Double) = 3 * x
Array(3.14, 1.42, 2.0).map(triple)

Array(3.14, 1.42, 2.0).map((x: Double) => 3 * x)
Array(3.14, 1.42, 2.0) map { (x: Double) => 3 * x }

```

### Functions with Function Parameters
*	takes another function as a parameter.

```
def valueAtOneQuarter(f: (Double) => Double) = f(0.25)
```


### Parameter Inference
```
valueAtOneQuarter((x: Double) => 3 * x) // 0.75
valueAtOneQuarter((x) => 3 * x)
valueAtOneQuarter(x => 3 * x)
valueAtOneQuarter(3 * _)
```

###  Higher-Order Functions
```
// map
(1 to 10).map(0.1 * _)
(1 to 10).map("*" * _).foreach(println _)

1 to 10 map ("*" * _ ) foreach println
1 to 10 map (100 * _ ) foreach println

// filter
(1 to 10).filter(_ % 2 == 0)

// reduceLeft
(1 to 10) reduceLeft(_ + _)

// sortWith
"Mary had a little lamb".split(" ").sortWith(_.length < _.length)
```

### Closures
*	A closure consists of code together with the
definitions of any nonlocal variables that the code uses.

```
def mulBy(f: Double) = (x: Double) => f * x
val trible = mulBy(3)
val half = nulBy(0.5)
println(s"${triple(14)} ${half(14)}") // Prints 42 7
```


### Curring
*	arguments could be separated into different argument lists. 

```
def add(x: Int): (Int) => Int = (y: Int) => x + y
def add(x: Int)(y: Int) = x + y
add(5)(6)

def courceAverage(tests: Double*)(assns: Double*)(quizzes: Double*): Double = {
	val avgTest = average(tests:_*)
	val avgAssn = average(assns:_*)
	val avgQuiz = averageDropMin(quizzes:_*)
	fullAve(avgTest, avgAssn, avgQuiz)
}

courseAverage(80,95,100)(100,80)(56,78,92,76)
```

### 函数类型
#### Function Literal函数字面值
*	本质上是匿名函数(Anonymous Function)

```
(s: String) => s.toLowerCase
```

*	匿名函数(Anonymous Function)

```
new Function1[String, String]{
	def apply(s: String): String = s.toLowerCase
}
```

```
new (String => String) {
	def apply(s: String): String = s.toLowerCase
}
```

*	函数实际上是`FunctionN[A1, A2, ..., An, R]`类型的一个实例而已
	*	例如`(s: String) => s.toLowerCase`是`Function1[String, String]`类型的一个实例


### References
*	https://www.jianshu.com/p/8869c0777cbe
