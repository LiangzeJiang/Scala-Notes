# Scala Notes  

## From Scala for the Impatient

### Chapter 1 The Basics

---

1. Declaring Values and Variables: 

   - declare a constant: `val answer = 8*5`
   - declare a variable whose contents can vary: `var counter = 0`
   - specify the type if necessary:  `val greeting: String = null`  
   - multiple values: `val xmax, ymax = 100`

2. Types:

   - `Byte, Char, Short, Int, Long, Float, Double, Boolean`

   - examples:

     ```scala
     1.toString()   // "1"
     1.to(10)       // Range(1,2,3,4,5,6,7,8,9,10)
     "Hello".intersect("world")  // "lo"
     ```

3. Arithmetic and Operator Overloading: Operators are actually methods.

   ```scala
   a + b // a.+(b)
   1 to 10 // 1.to(10)
   10 to 1// empty range
   counter+=1 // no ++
   ```

4.  More about Calling Methods:

   ```scala
   "Bonjour".sorted // "Bjnooru", without()
   
   import scala.math._ // _ as * in Java
   sqrt(2)
   scala.math.sqrt(2) // if don't import scala.math package
   ```

5. The apply Method:

   ```scala
   val s = "Hello"
   s(4)  // yields 'o', equals to s.apply(4)
   
   ("Bonjour".sorted)(3)
   "Bonjour".sorted.apply(3)
   ```

6. Other tips: 

   - Scala allows you to define operators, leaving it up to you to use this feature with restraint and good taste.

   - ```scala
     "crazy" * 3 // "crazycrazycrazy"
     10 max 2 // 10
     ```

### Chapter 2 Control Structures and functions

#### 2.1 Conditional Expressions

In Scala, an if/else has a value, namely the value of the expression that follows the if or else.

```scala
if (x > 0) 1 else -1  // either 1 or -1
val s  = if (x > 0) 1 else -1 // can be assigned to a value
if(x > 0) s = 1 else s = -1 // same as the last row
// The first form is better because it can be used to initialize a val. The second form needs s to be a var.
// no a?: operation in scala
```

In Scala, every expression has a type.

```scala
if (x > 0) 1 else -1  // Int type
if (x > 0) "positive" else -1  // mixed-type: java.lang.String and Int

if (x > 0) 1 
if (x > 0) 1 else ()
```

#### 2.2 Statement Termination

In scala, a semicolon is never required if it falls just before the end of the line.  However, if you want to have more than one statement on a single line, you need to separate them with semicolons. 

```scala
if (n > 0) { r = r * n; n -= 1 }
```

If you want to continue a long statement over two lines, make sure that the first line ends in a symbol that cannot be the end of a statement. An operator is often a good choice.

```scala
s = s0 + (v - v0) * t + // The + tells the parser that this is not the end
 0.5 * (a - a0) * t * t 

if (n > 0) {
 r = r * n
 n -= 1
}
```

We can also use the semicolons, they do no harm.

#### 2.3 Block Expressions and Assignments

In Scala, a { } block contains a sequence of expressions, and the result is also an expression. The value of the block is the value of the last expression.

```scala
val distance = { val dx = x - x0; val dy = y - y0; sqrt(dx * dx + dy * dy) }
```

An assignment has no value, or, strictly speaking, they have a value of type Unit. Recall that the Unit type is the equivalent of the void type in Java and C++, with a single value written as ().

```scala
{ r = r * n; n -= 1 } // has a unit value, be aware of it when defining functions
x = y = 1 // No
```

#### 2.4 Input and Output

To print a value, use the `print` or `println` function. The latter adds a newline character after the printout.

```scala
print("Answer: ")
println(42)
// is same as
println("Answer: " + 42)

//or 
printf("Hello, %s! You are %d years old.%n", name, age)
print(f"Hello, $name! In six months, you'll be ${age + 0.5}%7.2f years old.%n")
// A formatted string is prefixed with the letter f, it is better than printf because it's type-safe
```

To input, use `scala.io.StdIn` .

```scala
import scala.io
val name = StdIn.readLine("Your name: ")
print("Your age: ")
val age = StdIn.readInt()
println(s"Hello, ${name}! Next year, you will be ${age + 1}.")
```

#### 2.5 & 2.6 Loops and Advanced For Loops

`While` loops like Java and C++.

```scala
while (n > 0) {
    r = r * n
    n -= 1
}
```

for loops:

```scala
val s = "Hello"
var sum = 0
for (i <- 0 to s.length - 1)
	sum += s(i)
// or
for (ch <- "Hello") sum += ch
```

- In Scala, loops are not used as often as in other languages. As you will see in Chapter 12, you can often process the values in a sequence by applying a function to all of them, which can be done with a single method call.
- In Scala, there is no `break` or `continue` . We can use a Boolean variable, a nested functions to return from the middle of a function or use `break` method in `scala.util.control.Breaks._` 

Advanced for Loops

```scala
for (i <- 1 to 3; j <- 1 to 3) print(f"${10 * i + j}%3d") // 11 12 13 21 22 23 31 32 33
for (i <- 1 to 3; j <- 1 to 3 if i != j) print(f"${10 * i + j}%3d") // 12 13 21 23 31 32
for (i <- 1 to 3; from = 4 - i; j <- from to 3) print(f"${10 * i + j}%3d")
// 13 22 23 31 32 33
for (i <- 1 to 10) yield i % 3 // Vector(1, 2, 0, 1, 2, 0, 1, 2, 0, 1)
```

#### 2.7 Functions

Scala has functions in addition to methods. A method operates on an object, but a function doesn’t. C++ has functions as well, but in Java, you have to imitate them with static methods.

The Scala compiler determines the return type from the type of the expression to the right of the = symbol.

1.  A very simple absolute value function:

   ```scala
   def abs(x: Double) = if (x >= 0) x else -x
   ```

2.  Function with more than one expression

   ```scala
   def fac(n: Int) = {
       var r = 1
       for (i <- 1 to n) r = r * i
       r
   } // The last expression of the block becomes the value that the function returns
   ```

3. A recursive function must have the return type specified.

   ```scala
   def fac(n: Int): Int = if (n<=0) 1 else n * fac(n - 1)
   ```

   

#### 2.8 & 2.9 Default and Named Arguments / Variable Arguments

The named arguments should come first.

```scala
def decorate(str: String, left: String = "[", right: String = "]") =
 left + str + right

decorate("Hello") // "[Hello]"
decorate("Hello", "<<<", ">>>") "<<<Hello>>>"
decorate(left = "<<<", str = "Hello", right = ">>>") // need not to be in the same order.
```

Example of variable arguments:

```SCALA
def sum(args: Int*) = {
    var result = 0
    for (arg <- args) result += arg
    result
}

val s = sum(1, 4, 9, 16, 25)
val s = sum(1 to 5) // not correct, cannot pass a range


def recursiveSum(args: Int*) : Int = {
 if (args.length == 0) 0
 else args.head + recursiveSum(args.tail : _*)
}
val s = sum(1 to 5: _*) // consider 1 to 5 as an argument sequence
```

#### 2.10 Procedures

Scala has a special notation for a function that returns no value. If the function body is enclosed in braces without a preceding = symbol, then the return type is Unit. Such a function is called a procedure. A procedure returns no value, and you only call it for its side effect.

```scala
def box(s : String) { // Look carefully: no =
 val border = "-" * (s.length + 2)
 print(f"$border%n|$s|%n$border%n")
} 
// omit the '=' or use "Unit"
def box(s : String): Unit = {
 ...
} 
-------
|Hello|
-------
```

#### 2.11 Lazy Values

When a val is declared as lazy, its initialization is deferred until it is accessed for the first time.

```scala
lazy val words = scala.io.Source.fromFile("/usr/share/dict/words").mkString
```

If we never accesses `words` , then the file is never opened.

Lazy values are useful to delay costly initialization statements. They can also deal with other initialization issues, such as circular dependencies. Moreover, they are essential for developing lazy data structures

#### 2.12 Exceptions

Scala exceptions work the same way as in Java or C++. When you throw an exception, for example:

```scala
throw new IllegalArgumentException("x should not be negative")
```

However, unlike Java, Scala has no “checked” exceptions—you never have to declare that a function or method might throw an exception.

...skip for now.

#### 2.13 Tips from exercises

1. ```scala
   def signum(n: Int): Int = {if (n > 0) 1 else if (n < 0) -1 else 0}
   ```

4. ```scala
   for (i <- 10.to(0,-1)) println(i)
   ```

### Chapter 3 Working with Arrays

#### 3.1 Fixed-Length Arrays

An array whose length does not change, use `Array` type in Scala.

```SCALA
val nums = new Array[Int](10) // An array of ten integers, all initialized with zero
val a = new Array[String](10) // A string array with ten elements, initialized with null
// When we supply initial values, we don't need to specify the type, because the type can be inferred, and we also don't use 'new'
val s = Array("Hello","World") 
s(0) = "Goodbye" // Use () to access the elements
```

#### 3.2 Variable-Length Arrays: Array Buffers

An array whose length can change, use `ArrayBuffer` type in Scala.

```scala
import scala.collection.mutable.ArrayBuffer

val b = ArrayBuffer[Int]() // An empty array buffer
val b = new ArrayBuffer[Int] // An empty array buffer
b += 1 // Add an element at the end by +=
b += (1, 2, 3, 5) // Add multiple elements at the end by += (...)
b ++= Array(8, 13, 21) // Append any collection with ++= operator
b.trimEnd(5) // Remove the last five elements, efficient

// Insert and remove at an arbitrary location is not efficient
b.insert(2, 6) // Insert 6 before index 2
b.insert(2, 7, 8, 9) // Insert 7,8,9 before index 2
b.remove(2) // Remove the element at index 2
b.remove(2, 3) // Remove 3 elements starting from index 2

// The conversion between Array and ArrayBuffer:
b.toArray
a.toBuffer
```

#### 3.3 Traversing Arrays and Array Buffers

To traverse an array or array buffer with a `for` loop:

```scala
for (i <- 0 until a.length)
	println(s"$i: ${a(i)}")
// The 'until' method is similar to the 'to' method, except that it excludes the last value.
for (i <- 0 to a.length - 1)
	println(s"$i: ${a(i)}")

// Others
for (i <- 0 until a.length by 2) // Range(0, 2, 4, ...)
	println(s"$i: ${a(i)}")

for (i <- 0 until a.length by -1) // Range(..., 2, 1, 0)
	println(s"$i: ${a(i)}")

// If we don't the index, we can directly tranverse a
for (elem <- a)
	println(elem)

// Instead of 'until' we can use:
a.indices
a.indices.reverse
```

#### 3.4 Transforming Arrays

In Scala, it is easy to take an array (or array buffer) and transform it in some way. Such transformations don’t modify the original array but yield a new one. Use the `for/yield` loop.

```scala
val a = Array(2, 3, 5, 7, 11)
val result1 = for (elem <- a) yield 2 * elem // Resulting in an Array(4, 6, 10, 14, 22)
val result2 = for (elem <- a if elem % 2 == 0) yield 2 * elem
// Or
a.filter(_ % 2 == 0).map(2 * _)
a filter {_ % 2 == 0} map {2 * _}
```

Generally, it is better to have all index values together instead of seeing them one by one.

```scala
val result = for (elem <- a if elem >= 0) yield elem // This way to find all non-negative elements, rather than using while loop and find them one by one

// We can keep the positions to remove
val positionsToRemove = for (i <- a.indices if a(i) < 0) yield i
for (i <- positionsToRemove.reverse) a.remove(i) 
// Or
val positionsToKeep = for (i <- a.indices if a(i) >= 0) yield i
for (j <- positionsToKeep.indices) a(j) = a(positionsToKeep(j))
a.trimEnd(a.length - positionsToKeep.length)
```

#### 3.5 Common Alogrithms

Scala has built-in functions for sums and sortings. In order to use the sum method, the element type must be a numeric type: either an integral or floating-point type or BigInteger/BigDecimal.

```scala
import scala.collection.mutable.ArrayBuffer

Array(1, 7, 2, 9).sum // 19, works for ArrayBuffer too
ArrayBuffer("Mary", "had", "a", "little", "lamb").max // "little"

val b = ArrayBuffer(1, 7, 2, 9)
val bSorted = b.sorted // b is unchanged, bSorted is ArrayBuffer(1,2,7,9)

// Sort an array but not array buffer in place
val a = Array(1, 7, 2, 9)
scala.util.Sorting.quickSort(a) // a is now Array(1, 2, 7, 9)
```

We can display the contents of an array or array buffer:

```scala
a.mkString(" and ") // "1 and 2 and 7 and 9"
a.mkString("<", ",", ">") //"<1,2,7,9>"

b.toString
 // "ArrayBuffer(1, 7, 2, 9)"
 // The toString method reports the type, which is useful for debugging
```

#### 3.6 Deciphering Scaladoc

Check the book...

#### 3.7 Multidimensional Arrays

Like in Java, multidimensional arrays are implemented as arrays of arrays. For example, a two-dimensional array of `Double` values has the type Array[Array[Double]]. To construct such an array, use the `ofDim` method:

```scala
val matrix = Array.ofDim[Double](3, 4) // Three rows, four columns
matrix(row)(column) = 42 // To access an element
// We can make ragged arrays, with varying row lengths
val triangle = new Array[Array[Int]](10)
	triangle(i) = new Array[Int](i + 1)
```

#### 3.8 Interoperating with Java

Since Scala arrays are implemented as Java arrays, you can pass them back and forth between Java and Scala.

#### 3.9 Exercise Tips

1. ```scala
   // import scala.util.Random
   def rand(n: Int) = new Array[Int](n).map(_ => Random.nextInt(n))
   ```

2. 

### Chapter 4 Maps and Tuples

A classic programmer’s saying is, “If you can only have one data structure, make it a hash table.” Hash tables—or, more generally, maps—are among the most versatile data structures.

Maps are collections of key/value pairs. Scala has a general notion of tuples— aggregates of n objects, not necessarily of the same type. A pair is simply a tuple with n = 2. Tuples are useful whenever you need to aggregate two or more values together.

By default, you get a hash map, but you can also get a tree map. Tuples are useful for aggregating values.

#### 4.1 Constructing a Map

```scala
// constructs an immutable Map[String, Int] whose contents cannot be changed.
val scores = Map("Alice" -> 10, "Bob" -> 3, "Cindy" -> 8)

// constructs a mutable map
val scores = scala.collection.mutable.Map("Alice" -> 10, "Bob" -> 3, "Cindy" -> 8)

// we have to specify the type parameters if we want to build a empty map
val scores = scala.collection.mutable.Map[String, Int]()
```

#### 4.2 Accessing Map Values

```scala
val bobsScore = scores("Bob")
```

If the map doesn’t contain a value for the requested key, an exception is thrown. To check whether there is a key with the given value, call the `contains`  method:

```scala
val bobsScore = if (scores.contains("Bob")) scores("Bob") else 0

// if the map contains the key "Bob", return the value; otherwise, return 0
val bobsScore = scores.getOrElse("Bob", 0)
```

#### 4.3 Updating Map Values

In a mutable map, you can update a map value, or add a new one, with a () to the left of an = sign:

```scala
// update rule of mutable map
scores("Bob") = 10 // update the existing value for a key
scores("Fred") = 7 // add new key/value pair
scores += ("Bob" -> 10, "Fred" -> 7) // add a series of pairs
scores -= "Alice" //remove a pair

// we cannot update an immutable map, but we can do this:
val newScores = scores + ("Bob" -> 10, "Fred" -> 7)
// or
var scores = ...
scores = scores + ("Bob" -> 10, "Fred" -> 7)
scores += ("Bob" -> 10, "Fred" -> 7) // we can use this update rule if scores is a var
// remove a key from an immutable map
scores = scores - "Alice"
// or
scores -= "Alice" // same as a mutable map
```

#### 4.4 Iterating over Maps

```scala
for ((k, v) <- scores) {do your things}
for (v <- scores.values) println(v)
scores.keySet // A set such as Set("Bob", "Cindy", "Fred", "Alice")
// reverse a map
for ((k, v) <- map) yield (v, k)
```

#### 4.5 Sorted Maps

There are two common implementation strategies for maps: hash tables and balanced trees. Hash tables use the hash codes of the keys to scramble entries, so iterating over the elements yields them in `unpredictable order` . By default, Scala gives you a map based on a hash table because it is usually more efficient. If you need to visit the keys in sorted order, use a `SortedMap` instead.

```scala
val scores = scala.collection.mutable.SortedMap("Alice" -> 10,
 "Fred" -> 7, "Bob" -> 3, "Cindy" -> 8)
```

If you want to visit the keys in insertion order, use a `LinkedHashMap`. For example,

```scala
val months = scala.collection.mutable.LinkedHashMap("January" -> 1,
 "February" -> 2, "March" -> 3, "April" -> 4, "May" -> 5, ...)
```

#### 4.6 Interoperating with Java

Check the book...

#### 4.7 Tuples

Maps are collections of key/value pairs. Pairs are the simplest case of tuples— aggregates of values of different types. A tuple value is formed by enclosing individual values in parentheses.

```scala
val t = (1, 3.14, "Fred") // a tuple of type Tuple3[Int, Double, Java.lang.String]
val second = t._2 // access its compoments with the methods _1, _2, _3
// or 
val second = t _2
// note that the component positions of a tuple start with 1, not 0.

// pattern matching
val (first, second, third) = t // Sets first to 1, second to 3.14, third to "Fred"
```

#### 4.8 Zipping

```scala
val symbols = Array("<", "-", ">")
val counts = Array(2, 10, 2)
val pairs = symbols.zip(counts)
// yields an array of pairs Array(("<", 2), ("-", 10), (">", 2))
// the pairs can then be processed together
for ((s, n) <- pairs) print(s * n) // Prints <<---------->>

// we can use toMap method turns a collection pairs into a map
keys.zip(values).toMap // a Map
```

#### 4.9 Exercise Tips

1. ```scala
   val discount = for((k, v) <- gizmos ) yield (k,v * 0.9)
   ```

2. ```scala
   def minmax(a: Array[Int]) = (a.min, a.max)
   ```

3. ```scala
   def lteqgt(values: Array[Int], v: Int) = (values.count(_<v), values.count(_==v), values.count(_>v))
   ```

### Chapter 5 Classes

#### 5.1 Simple Classes and Parameterless Methods

#### 5.2 Properties with Getters and Setters

#### 5.3 Properties with Only Gettters

#### 5.4 Object-Private Fields

#### 5.5 Bean Properties

#### 5.6 Auxiliary Constructors

#### 5.7 The Primary Constructor

#### 5.8 Nested Classes

#### 5.9 Exercises Tips

