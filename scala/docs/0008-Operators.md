## Operators
```
1 to 10
1.to(10)

1 -> 10
1 .->(10)
```

### Binary operators
*	Infix operators are binary operators—they have two parameters. 

```
# assignment
a operator= b
a = a operator b
a += b
a -= b
```

*	all operators are left-associative except for
	*	end in a colon(:)
	* 	assignment operators(op=) 

```
1 :: 2 :: Nil
a :: (2 :: Nil)
```

### Unary operators
*	An operator with one parameter is called a unary operator.

#### Prefix operators
```
+
-
!
~
```

#### Postfix operators
```
a identifier
a.identifier()

42 toString
42.toString()
```

*	Postfix operators can lead to parsing errors. 

```
import scala.language.postfixOps
// -language:postfixOps
```

##TD
