## Pattern matching
*	`switch`
* 	type inquiry
*  destructuring

### A better Switch
```
var sign = ...
val ch: Char = ...

ch match {
	case '+' => sign = 1
	case '-' => sign = -1
	// default
	case _   => sign = 0
}

// match is an expression
sign = ch match {
	case '+' => 1
	case '-' => -1
	case_ => 0
}
```

*	用于值判断

```
object MatchApp extends App {
	val names = Array("Tom", "Jim", "Greate")
	val name = names(Random.nextInt(names.length))
	
	name match {
		case "Tom" => println("Tom")
		case "Jim" => println("Jim")
		case "Greate" => println("Greate")
		case _ => println("Someone else")
	}
}
```

### Type Patterns
```
obj match {
	case x:Int => x
	case s:String => Integer.parseInt(s)
	case _:BigInt => Int.MaxValue
	case _ => 0
}

case m: Map[_,_] => ...
case m: Array[Int]
```

### Matching Arrays,Lists and Tuples
```
arr match {
	case Array(0) => "0"
	case Array(x,y) => s"$x $y"
	case Array(0, _*) => "0 ..."
	case _ => "..."
}

case Array(x, rest @ _*) => rest.min
```

```
// list
lst match {
	case 0::Nil => "0"
	case x::y::Nil => s"$x $y"
	case 0 :: tail => "0 ..."
	case _ => "something else"
}
```

```
// tuple
pair match {
     case (0, _) => "0 ..."
     case (y, 0) => s"$y 0"
     case _ => "neither is 0"
}
```

### Regular expressins
```
val pat = "([0-9]+) ([a-z]+)".r
val str = "99 bottles"

str match {
	// set num=99, item = "bottles"
	case pat(num, item) => ...
}

pat.unapplySeq()
```

### Patterns in Variable Declarations
```
val (x, y) = (1,2)
val Array(first, second, rest @ _*) = arr
```

### Patterns in `for`
```
import scala.collection.JavaConversions.propertiesAsScalaMap

for((k,v) <- System.getProperties())

for((k, "") <- System.getProperties())

for((k,v) <- System.getProperties() if v == "")
```

### Case Classes
*	Case classes are a special kind of classes that are optimized for use in pattern matching.

```
abstract class Amount 
case class Dollar(value: Double) extends Amount
case class Currency(value: Double, unit: String) extends Amount
// singletons
case object Nothing extends Amount

amt match {
	case Dollar(v) => s"$$$v"
	case Currency(_, u) => s"$u"
	case Nothing => ""
}
```


```
abstract class Notification

case class Email(sender: String, title: String, body: String) extends Notification

case class SMS(caller: String, message: String) extends Notification

case class VoiceRecording(contactName: String, link: String) extends Notification


def showNotification(notification: Notification): String = {
  notification match {
    case Email(email, title, _) =>
      s"You got an email from $email with title: $title"
    case SMS(number, message) =>
      s"You got an SMS from $number! Message: $message"
    case VoiceRecording(name, link) =>
      s"you received a Voice Recording from $name! Click the link to hear it: $link"
  }
}

val someSms = SMS("12345", "Are you there?")
val someVoiceRecording = VoiceRecording("Tom", "voicerecording.org/id/123")

println(showNotification(someSms))  // prints You got an SMS from 12345! Message: Are you there?

println(showNotification(someVoiceRecording))  // you received a Voice Recording from Tom! Click the link to hear it: voicerecording.org/id/123
```

### Constant patterns
*	常量变量
* 	常量字面量

```
scala> val site = "alibaba.com"
scala> site match { case "alibaba.com" => println("ok") }

//注意这里常量必须以大写字母开头
scala> val ALIBABA="alibaba.com"
scala> def foo(s:String) { s match { case ALIBABA => println("ok") } } 
```

### Variable patterns
```
scala> site match { case whateverName => println(whateverName) }
scala> List(1,2) match{ case List(x,2) => println(x) }
```

###  通配符模式(wildcard patterns)
```
scala> List(1,2,3) match{ case List(_,_,3) => println("ok") }
```

### 构造器模式(constructor patterns)
```
scala> :paste
//抽象节点
trait Node 
//具体的节点实现，有两个子节点
case class TreeNode(v:String, left:Node, right:Node) extends Node 
//Tree，构造参数是根节点
case class Tree(root:TreeNode)  

scala>val tree = Tree(TreeNode("root",TreeNode("left",null,null),TreeNode("right",null,null)))

scala> tree.root match { 
        case TreeNode(_, TreeNode("left",_,_), TreeNode("right",null,null)) =>
             println("bingo") 
    }
```

### 类型模式(type patterns)
```
scala> "hello" match { case _:String => println("ok") }

// 范型
scala> def foo(a:Any) = a match { 
            case a :List[_] => println("ok"); 
            case _ => 
        } 
```

### 变量绑定模式 (variable binding patterns)
```
scala> tree.root match { 
			case TreeNode(_, leftNode@TreeNode("left",_,_), _) => leftNode 
	}
```


### Sum Up
*	`macth` better than `switch`, without fall-through
* 	`_` aviod `MatchError`
*	You can match on the type of an expression; prefer this over `isInstanceOf/asInstanceOf`.
* 	arrays/tuples/case classes and bind parts of the pattern to variables

