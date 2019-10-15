### Strings
```
val speech = """Four score and
#seven years ago""".stripMargin('#')

.stripMargin.replaceAll("\n", " ")
.split(" ")

s.split(",").map(_.trim)

println(s"${name}")
println(f"")

raw"foo\nbar"

val s = "%s is %d years old".format(name, age)

"hello, world".map(c => c.toUpper)
"hello, world".filter(_ != 'l').map(_.toUpper)

"123 Main Street".replaceAll("[0-9]", "x")

"hello".charAt(0)
```

### Complex numbers and Dates
*	Spire 
* 	ScalaLab
*  	Joda Time
*	nscala-time

### Files
```
import scala.io.Source
val lines = Source.fromFile("test.txt", "UTF-8").getLines.toArray
val text  = Source.fromFile("test.txt", "UTF-8").mkString
```

```
val source = Source.fromFile("myfile.txt", "UTF-8")
val tokens = source.mkString.split("\\s+")
val numbers = for(w <- tokens) yield w.toDouble
val numbers = tokens.map(_.toDouble)
```

```
val age = Scala.io.readInt()
```

```
val source1 = Source.fromURL("http://horstmann.com", "UTF-8")
val source2 = Source.fromString("Hello, World!")
val source3 = Source.Stdin
```

```
# binary Files
val file = new File(filename)
val in   = new FileInputStream(file)
val bs   = new Array[Byte](file.length.toInt)
in.read(bs)
in.close()
```

```
# writing 
val out = new PrintWriter("numbers.txt")
for(i <- 1 to 100) 
	out.println(i)
out.close()

out.print(f"$quantity%6d $price%10.2f")
```

```
# Directories
import jaba.nio.file._
String path = "/home/tanhao"
val entries = Files.walk(Paths.get(path))
try {
	entries.forEach()
} finally {
	entries.close()
}
```

```
# process
import scala.sys.process._
val result = "ls -al /home/tanhao"
```

### Regular Expresssions
```
val numPattern = "[0-9]+".r
val wsnumwsPattern = """\s+[0-9]+\s+""".r

for(matchString <- numPattern.findAllIn("99 bottles, 98 bottles"))
	println(matchString)

val matches = numPattern.findAllIn("99 bottles, 98 bottles").toArray

val firstMatch = wsnumwsPattern.findFirstIn("99 bottles, 98 bottles")
```

#### Groups
```
val pat = "([0-9]+) ([a-z]+)".r

findAllMatchIn
findFirstMatchIn

for (m <- pat.findAllMatchIn("99 bottles, 98 bottles"))
	println(m.group(1))
```
