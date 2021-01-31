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

In Scala, a class is not declared as public. A Scala source file can contain multiple classes, and all of them have public visibility.

```scala
// a class example
class Counter {
 private var value = 0 // You must initialize the field
 def increment() { value += 1 } // Methods are public by default
 def current() = value
}

// to use this class
val myCounter = new Counter()
myCounter.increment()
println(myCounter.current) // do not change the state, so we drop the parentheses
// we can call parameterless method without parentheses
// we can define parameterless method without parentheses
```

It is considered good style to use () for a mutator method (a method that changes the object state), and to drop the () for an accessor method (a method that does not change the object state).

#### 5.2 Properties with Getters and Setters

With a public field, anyone could write to the parameter and change it. That’s why we prefer to use getter and setter methods:

```java
public class Person { // This is Java
 private int age;
 public int getAge() { return age; }
 public void setAge(int age) { this.age = age; }
}
```

Getters and setters are better than public fields because they let you start with simple get/set semantics and evolve them as needed.

Scala provides getter and setter methods for every field. Here, we define a public field:

```scala
class Person {
 var age = 0
}
```

Scala generates a class for the JVM with a `private age` field and `getter` and `setter` methods. These methods are public because we did not declare age as private. (For a private field, the getter and setter methods are private.)

In Scala, the getter and setter methods are called age and age_=. For example,

```scala
println(fred.age) // calls the method fred.age()
fred.age = 21 // calls fred.age_ = (21)
```

#### 5.3 Properties with Only Getters

Sometimes you want a read-only property with a getter but no setter. If the value of the property never changes after the object has been constructed, use a `val` field.

Sometimes, however, you want a property that a client can’t set at will, but that is mutated in some other way. You can’t implement such a property with a val—a val never changes. Instead, provide a private field and a property getter, like this:

```scala
class Counter {
    private var value = 0
    def increment() { value += 1}
    def current = value // No () in declaration
}
// Note that there are no () in the definition of the getter method. Therefore, you
// must call the method without parentheses:
val n = myCounter.current // Calling myCounter.current() is a syntax error
```

To summarize, you have four choices for implementing properties: 

1. `var foo`: Scala synthesizes a getter and a setter. 
2. `val foo`: Scala synthesizes a getter.
3. You define methods `foo` and `foo_=`. 
4. You define a method `foo`.

#### 5.4 Object-Private Fields

In Scala (as well as in Java or C++), a method can access the private fields of all objects of its class. For example,

```scala
class Counter {
 	private var value = 0
 	def increment() { value += 1 }
 	def isLess(other : Counter) = value < other.value
 	// Can access private field of other object
}
```

Scala allows an even more severe access restriction with the `private[this]` qualifier:

```scala
private[this] var value = 0
```

Now, the methods of the Counter class can only access the value field of the current object, not of other objects of type Counter. This access is sometimes called `object-private`.

With a class-private field, Scala generates private getter and setter methods. However, for an object-private field, no getters and setters are generated at all.

#### 5.5 Bean Properties

Check the book...

#### 5.6 Auxiliary Constructors

a Scala class can have as many constructors as you like. However, a Scala class has one constructor that is more important than all the others, called the `primary constructor`. In addition, a class may have any number of `auxiliary constructors`.

1. The auxiliary constructors are called this. (In Java or C++, constructors have the same name as the class—which is not so convenient if you rename the class.)
2. Each auxiliary constructor must start with a call to a previously defined auxiliary constructor or the primary constructor.

```scala
class Person {
	private var name = ""
	private var age = 0
	def this(name: String) { // An auxiliary constructor
		this() // Calls primary constructor
		this.name = name
	}
	def this(name: String, age: Int) { // Another auxiliary constructor
		this(name) // Calls previous auxiliary constructor
		this.age = age
	}
}

// construct the objects in three ways
val p1 = new Person // Primary constructor
val p2 = new Person("Fred") // First auxiliary constructor
val p3 = new Person("Fred", 42) // Second auxiliary constructor
```

#### 5.7 The Primary Constructor

In Scala, every class has a primary constructor. The primary constructor is not defined with a `this` method. Instead, it is interwoven with the class definition.

1. The parameters of the primary constructor are placed immediately after the class name.

   ```scala
   class Person(val name: String, val age: Int) {
       // Parameters of primary constructor in (...)
       ...
   }
   ```

2. The primary constructor executes all statements in the class definition. For example, in the following class, e.g., 

   ```scala
   class Person(val name: String, val age: Int) {
       println("Just constructed another person") // a part of the primary constructor
       def description = s"$name is $age years old"
   }
   ```

3. You can often eliminate auxiliary constructors by using default arguments in the primary constructor. For example:

   ```scala
   class Person(val name: String = "", val age: Int = 0)
   ```

If you find the primary constructor notation confusing, you don’t need to use it. Just provide one or more auxiliary constructors in the usual way, but remember to call this() if you don’t chain to another auxiliary constructor.

#### 5.8 Nested Classes

In Scala, you can nest just about anything inside anything. You can define functions inside other functions, and classes inside other classes. Here is a simple example of the latter:

```scala
import scala.collection.mutable.ArrayBuffer
class Network {
	class Member(val name: String) {
		val contacts = new ArrayBuffer[Member]
	}
	private val members = new ArrayBuffer[Member]
	def join(name: String) = {
		val m = new Member(name)
		members += m
		m
	}
}

val chatter = new Network
val myFace = new Network

```

In Scala, each instance has its own class Member, just like each instance has its own field members. That is, chatter.Member and myFace. Member are different classes. In our network example, you can add a member within its own network, but not across networks. 

#### 5.9 Exercises Tips

### chapter 6 Objects

In this short chapter, you will learn when to use the object construct in Scala. Use ptg20087099 it when you need a class with a single instance, or when you want to find a home for miscellaneous values or functions.

#### 6.1 Singletons

Scala has no static methods or fields. Instead, you use the `object` construct. An object defines a single instance of a class with the features that you want.

```scala
// When you need a new unique account number in your application, call Accounts.newUniqueNumber().
object Accounts {
    private var lastNumber = 0
    def newUniqueNumber() = { lastNumber += 1; lastNumber}
}
```

The constructor of an object is executed when the object is first used. In our example, the Accounts constructor is executed with the first call to `Accounts. newUniqueNumber()`. If an object is never used, its constructor is not executed.

Use an object in Scala whenever you would have used a singleton object in Java or C++: 

- As a home for utility functions or constants 
- When a single immutable instance can be shared efficiently 
- When a single instance is required to coordinate some service (the singleton design pattern)

#### 6.2 Companion Objects

In Java or C++, you often have a class with both instance methods and static methods. In Scala, you can achieve this by having a class and a “companion” object of the same name.

```scala
class Account {
    val id = Account.newUniqueNumber()
    private var balance = 0.0
    def deposit(amount: Double) { balance += amount }
}

object Account { // The companion object
    private var lastNumber = 0
    private def newUniqueNumber() = { lastNumber += 1; lastNumber}
}
```

The class and its companion object can access each other’s private features. They must be located in the `same source file`. 

Note that the companion object’s features are not in the scope of the class. The result is an object of a class that extends the given class and/or traits, and in addition has all of the features specified in the object definition.

#### 6.3 Objects Extending a Class or Trait

An object can extend a class and/or one or more traits.  The result is an object of a class that extends the given class and/or traits, and in addition has all of the features specified in the object definition.

```scala
object DoNothingAction extends UndoableAction("Do nothing") {
    override def undo() {}
    override def redo() {}
}
```

#### 6.4 The `Apply` Method

It is common to have objects with an apply method. The apply method is called for expressions of the form: `Object(arg1, ..., argN)`. Typically, such an apply method returns an object of the companion class.

```scala
class Account private (val id: Int, initialBalance: Double) {
    private var balance = initialBalance
    ...
}

object Account {
    def apply(initialBalance: Double) = 
    	new Account(newUniqueNumber(), initialBalance)
    ...
}

// Now you can construct an account as
val acct = Account(1000.0)
```

#### 6.5 Application Objects

Each Scala program must start with an object’s `main` method of type `Array[String] => Unit`:

```scala
object Hello {
    def main(args: Array[String]) {
        println("Hello, World!")
    }
}
```

Instead of providing a `main` method for your application, you can extend the `App` trait and place the program code into the constructor body:

```scala
object Hello extends App {
    println("Hello, world!")
}
```

If you need the command-line arguments, you can get them from the args property:

```scala
object Hello extends App {
    if (args.length > 0)
    	println(f"Hello ${args(0)}")
    else
    	println("Hello, World!")
}
```

#### 6.6 Enumerations

Unlike Java or C++, Scala does not have enumerated types. However, the standard library provides an `Enumeration` helper class that you can use to produce enumerations. Define an object that extends the `Enumeration` class and `initialize each value in your enumeration with a call to the Value` method. For example,

```scala
object TrafficLightColor extends Enumeration {
    val Red, Yellow, Green = Value
}
```

Each call to the Value method returns a new instance of an inner class, also called Value.

Alternatively, you can pass IDs, names, or both to the Value method:

```scala
val Red = Value(0, "Stop")
val Yellow = Value(10)
val Green = Value("Go")
```

You can now refer to the enumeration values as TrafficLightColor.Red, TrafficLightColor.Yellow, and so on. If that gets too tedious, use

```scala
import TrafficLightColor._
```

The call `TrafficLightColor.values` yields a set of all values:

```scala
for (c <- TrafficLightColor.values) println(s"${c.id}: $c")
```

 Some people recommend that you add a type alias:

```scala
object TrafficLightColor extends Enumeration {
	type TrafficLightColor = Value
 	val Red, Yellow, Green = Value
}
```

Now the type of the enumeration is TrafficLightColor.TrafficLightColor, which is only an improvement if you use an import statement. For example,

```scala
import TrafficLightColor._
def doWhat(color: TrafficLightColor) = {
 if (color == Red) "stop"
 else if (color == Yellow) "hurry up"
 else "go"
}
```

Finally, you can look up an enumeration value by its ID or name. Both of the following yield the object TrafficLightColor.Red:

```scala
TrafficLightColor(0) // Calls Enumeration.apply
TrafficLightColor.withName("Red")
```

#### 6.7 Exercise Tips

### Chapter 7 Packages and Imports

#### 7.1 Packages

To add items to a package, you can include them in package statements, such as:

```scala
package com {
    package horstmann {
        package impatient {
            class Employee
            ...
        }
    }
}
```

Then the class name Employee can be accessed anywhere as `com.horstmann. impatient.Employee`.

Unlike an object or a class, a package can be defined in multiple files. The preceding code might be in a file Employee.scala, and a file Manager.scala might contain

```scala
package com {
    package horstmann {
        package impatient {
            class Employee
            ...
        }
    }
}
```

Conversely, you can contribute to more than one package in a single file. The file `Employee.scala` may contain:

```scala
package com {
    package horstmann {
        package impatient {
            class Employee
            ...
        }
    }
}

package net {
    package bigjava {
        class Counter
        ...
    }
}
```

There is no enforced relationship `between the directory of the source file and the package`.You don’t have to put `Employee.scala` and `Manager.scala` into a `com/horstmann/impatient` directory.

#### 7.2 Scope Rules

Scala packages nest just like all other scopes. You can access names from the enclosing scope. For example,

```scala
package com {
    package horstmann {
        object Utils {
            def percentOf(value: Double, rate: Double) = vlaue * rate / 100
            ...
        }
        package impatient {
            class Employee {
                ...
                def giveRaise(rate: scala.Double) {
                    salary += Utils.percentOf(salary, rate) //**
                }
            }
        }
	}
}
```

The following code takes advantage of the fact that the scala package is always imported. Therefore, the collection package is actually `scala.collection`.

```scala
package com {
    package horstmann {
        package impatient {
            class Manager {
                val subordinates = new collection.mutable.ArrayBuffer[Employee]
                ...
            }
        }
    }
}
```

#### 7.3 Chained Package Clauses

A package clause can contain a “chain,” or path segment, such a clause limits the visible members.  Now a `com.horstmann.collection` package would no longer be accessible as `collection`.

```scala
package com.horstmann.impatient {
    // Members of com and com.horstmann are not visible here
    package people {
        class Person
        ...
    }
}
```

#### 7.4 Top-of-File Notation

we can also change the aforementioned codes as:

```scala
package com.horstmann.impatient
package people

class Person
...
```

#### 7.5 Package Objects

A package can contain classes, objects, and traits, but not the definitions of functions or variables. That’s an unfortunate limitation of the Java virtual machine. It would make more sense to add utility functions or constants to a package than to some `Utils` object. Package objects address this limitation.

Every package can have one package object. You define it in the parent package, and it has the same name as the child package. For example,

```scala
package com.horstmann.impatient

package object people {
    val defaultName = "John Q. Public"
}

package people {
    class Person {
        var name = defaultName // A constant from the package
    }
    ...
}
```

#### 7.6 Package Visibility

 The following method is visible in its own package:

```scala
package com.horstmann.impatient.people

class Person {
    private[people] def description = s"A person with name $name"
    ...
}

// you can extend the visibility to an enclosing package:
private[impatient] def description = s"A person with name $name"
```

#### 7.7 Imports

Imports let you use short names instead of long ones. With the clause

```scala
import java.awt.Color
```

you can write Color in your code instead of java.awt.Color.

```scala
import java.awt._ // to import all members of a package as
```

You can also import all members of a class or object.

```scala
import java.awt.Color._
val c1 = RED // Color.RED
val c2 = decode("#ff0000") // Color.decode
```

Once you import a package, you can access its subpackages with shorter names.

```scala
import java.awt._

def handler(evt: event.ActionEvent) { // java.awt.event.ActionEvent
    ...
}
```

#### 7.8 Import Can Be Anywhere

In Scala, an import statement can be anywhere, not just at the top of a file. The scope of the import statement extends until the end of the enclosing block. For example,

```scala
class Manager {
    import scala.collection.mutable._
    val subordinates = new ArrayBuffer[Employee]
    ...
}
```

By putting the imports where they are needed, you can greatly reduce the potential for conflicts.

#### 7.9 Renaming and Hiding Members

If you want to import more than one member from a package, use a selector like this:

```scala
import java.awt.{Color, Font}

// The selector syntax lets you rename members:
import java.util.{HashMap => JavaHashMap}
import scala.collection.mutable._
```

Now JavaHashMap is a java.util.HashMap and plain HashMap is a scala.collection.mutable. HashMap.

The selector HashMap => _ hides a member instead of renaming it. This is only useful if you import others:

```scala
import java.util.{HashMap => _, _}
import scala.collection.mutable._
```

Now HashMap unambiguously refers to scala.collection.mutable.HashMap since java.util. HashMap is hidden.

#### 7.10 Implicit Imports

Every Scala program implicitly starts with

```scala
import java.lang._
import scala._
import Predef._
```

As with Java programs, java.lang is always imported. Next, the scala package is imported, but in a special way. Unlike all other imports, this one is allowed to override the preceding import. For example, scala.StringBuilder overrides java.lang.StringBuilder instead of conflicting with it.

Finally, the Predef object is imported. It contains commonly used types, implicit conversions, and utility methods. (The methods could equally well have been placed into the scala package object, but Predef was introduced before Scala had package objects.)

```scala
// we do not need to write package names that start with scala. E.g.,
collection.mutable.HashMap
// is equal to
scala.collection.mutable.HashMap
```

#### 7.11 Exercise Tips

