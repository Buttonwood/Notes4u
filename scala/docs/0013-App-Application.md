## Apps and Applications
### `main`
```
object Hello {
	def main(args: Array[String]): Unit = {
		println("Hello Scala")
	}
}
```

### `App` trait
*	If the object that will be used as the entry point to your application extends a `trait` called `App` then any code not placed into any named function or method (main is an example of a named method) will be assumed to be part of the main method. 
*	Avoiding the need to write the `main` methods.
*	`Application` trait was replaces by `App` since Scala 2.9.
*	If you include the `App` trait (using the keyword `extends`) you cannot also write the `main` method, you can choose one or other approach but not both.

```
class Hello extends App {
	println("Hello World")
}
```
