### `case class` 
`case`类后,`Scala`会自动帮你创建一个伴生对象并帮你实现了一系列方法且带来了不少好处. `case`类本就旨在创建的是不可变数据.

*	1.实现了`apply`方法, 意味着你不需要使用new关键字就能创建该类对象

```
case class People(name: String, age: Int)

val p = People("Tom", 30)
```

*	2.实现了`unapply`方法, 可以通过模式匹配来获取类属性, 是Scala中抽取器的实现和模式匹配的关键方法

```
p match {
	case People(name,age) => println(name,age) 
}
```

*	3. 实现了类构造参数的`getter`方法(构造参数默认被声明为`val`),但是当你构造参数是声明为`var`类型的,它将帮你实现`setter`和`getter`方法(不建议将构造参数声明为`var`)

```
// getter
val name = p.name

// can not set
p.name = "Jim"
```

```
// var 
case class Employee(var name: String)

// applay
val e = Employee("Tom",30)
// getter
val name = e.name
// setter
e.name = "James"
```

*	4. 还默认帮你实现了`toString`, `equals`,`copy`和`hashCode`等方法

```
// 反编译
scalac People.scala
javap People
```
