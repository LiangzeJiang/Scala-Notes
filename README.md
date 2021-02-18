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

### Chapter 8 Inheritance

Highlights in this chapter:

- The `extends` and `final` keywords are as in Java
- You must use `override` when you override a method
- Only the primary constructor can call the primary superclass constructor
- You can override fields

#### 8.1 Extending a Class

Like in Java,

```scala
class Employee extends Person {
    var salary = 0.0
    ...
}
```

we can specify fields and methods that are new to the subclass or that override the methods in the superclass.

You can declare a class `final` so that it cannot be extended. You can also declare individual methods or fields `final` so that they cannot be overridden.

#### 8.2 Overriding Methods

In Scala, you **must** use the `override` modifier when you override a method that isn’t abstract.

```scala
class Person {
    ...
    override def toString = s"${getClass.getName}[name=$name]"
}
```

Invoking a superclass method in Scala works exactly like in Java, with the keyword `super`:

```scala
class Employee extends Person {
    ...
    override def toString = s"${super.getName}[name=$name]"
}
```

The call super.toString invokes the toString method of the superclass—that is, the Person.toString method.

#### 8.3 Type Checks and Casts

To test whether an object belongs to a given class, use the `isInstanceOf` method. . If the test succeeds, you can use the asInstanceOf method to convert a reference to a subclass reference:

```scala
if (p.isInstanceOf[Employee]) {
    val s = p.asInstanceOf[Employee] // s has type Employee
    ...
}
```

The `p.isInstanceOf[Employee]` test succeeds if p refers to an object of class `Employee` or its subclass (such as `Manager`). 

If p is `null`, then `p.isInstanceOf[Employee]` returns false and `p.asInstanceOf[Employee]` returns `null`. 

If p is not an Employee, then `p.asInstanceOf[Employee]` throws an exception. 

If you want to test whether p refers to an Employee object, but not a subclass, use

```scala
if (p.getClass == classOf[Employee])
```

#### 8.4 Protected Fields and Methods

As in Java or C++, you can declare a field or method as `protected`. Such a member is accessible from any subclass, but not from other locations.

#### 8.5 Superclass Construction

Recall from Chapter 5 that a class has one primary constructor and any number of auxiliary constructors, and that all auxiliary constructors must start with a call to a preceding auxiliary constructor or the primary constructor.

As a consequence, an auxiliary constructor can **never** invoke a superclass constructor directly. Because the auxiliary constructors of the subclass eventually call the primary constructor of the subclass, and only the primary constructor can call a superclass constructor.

Intertwining the class and the constructor makes for very concise code. You may find it helpful to think of the primary constructor parameters as parameters of the class. Here, the Employee class has three parameters: name, age, and salary, two of which it “passes” to the superclass.

```scala
class Employee(name: String, age: Int, val salary : Double) extends Person(name, age)
```

corresponding Java version is

```java
public class Employee extends Person {
    private double salary;
    public Employee(String name, int age, double salary) {
        super(name, age)
        this.salary = salary
    }
}
```

In a Scala constructor, you can never call super(params), as you would in Java, to call the superclass constructor.

#### 8.6 Overriding Fields

Recall from Chapter 5 that a field in Scala consists of a private field and accessor/mutator methods. You can override a `val` (or a parameterless `def`) with another val field of the same name.

```scala
abstract class Person {
    def id: Int // Each person has an ID that is computed in some way
    ...
}

class Student(override val id: Int) extends Person
// A student ID is simply provided in the constructor
```

- A def can only override another def. 
- A val can only override another val or a parameterless def. 
- A var can only override an abstract var.

#### 8.7 Anonymous Subclasses

As in Java, you make an instance of an anonymous subclass if you include a block with definitions or overrides, such as

```scala
val alien = new Person("Fred") {
    def greeting = "Greetings, Earthling! My name is Fred."
}
```

Technically, this creates an object of a structural type. The type is denoted as Person{def greeting: String}. You can use this type as a parameter type:

```scala
def meet(p: Person{def greeting: String}) {
    println(s"${p.name} says: ${p.greeting}")
}
```

#### 8.8 Abstract Classes

As in Java, you can use the abstract keyword to denote a class that cannot be instantiated, usually because one or more of its methods are not defined. For example,

```scala
abstract class Person(val name: String) {
    def id: Int // No method body, this is an abstract method
}
```

Here we say that every person has an ID, but we don’t know how to compute it. Each concrete subclass of Person needs to specify an id method.  In Scala, unlike Java, you do not use the abstract keyword for an abstract method. **You simply omit its body.** As in Java, a class with at least one abstract method must be declared abstract.

In a subclass, you **need not** use the `override` keyword when you define a method that was abstract in the superclass.

```scala
class Employee(name: String) extends Person(name) {
    def id = name.hashCode // override keyword not required
}
```

#### 8.9 Abstract Fields

In addition to abstract methods, a class can also have abstract fields. An abstract field is simply a field without an initial value.

```scala
abstract class Person {
    val id: Int
    var name: String
}
```

This class defines abstract getter methods for the id and name fields, and an abstract setter for the name field. The generated Java class has no fields.

Concrete subclasses must provide concrete fields,

```scala
class Employee(val id: Int) extends Person {
    var name = ""
}
```

As with methods, no override keyword is required in the subclass when you define a field that was abstract in the superclass.

You can always customize an abstract field by using an anonymous type:

```scala
val fred = new Person {
    val id = 1729
    var name = "Fred"
}
```

#### 8.10 Construction Order and Early Definitions

Check the book...

#### 8.11 The Scala Inheritance Hierarchy

- The classes that correspond to the primitive types in Java, as well as the type Unit, extend `AnyVal`. 

- You can also define your own `value classes` .

- All other classes are subclasses of the `AnyRef` class.

- Both `AnyVal` and `AnyRef` extend the `Any` class, the root of the hierarchy.

- The `Any` class defines methods `isInstanceOf`, `asInstanceOf`, and the methods for equality and hash codes.

- `AnyVal` does not add any methods. It is just a marker for value types.
- The AnyRef class adds the monitor methods wait and notify/notifyAll from the Object class. It also provides a synchronized method with a function parameter.
- All Scala classes implement the marker interface ScalaObject, which has no methods.

Picture on page101...

The `???` method is declared with return type Nothing. It never returns but instead throws a NotImplementedError when invoked. You can use it for methods that you still need to implement:

```scala
class Person(val name: String) {
    def description = ???
}
```

#### 8.12 Object Equality

In Scala, the eq method of the AnyRef class checks whether two references refer to the same object. The equals method in AnyRef calls eq. When you implement a class, you should consider overriding the equals method to provide a natural notion of equality for your situation.

if you define a `class Item(val description: String, val price: Double)`, you might want to consider two items equal if they have the same description and price. Here is an appropriate equals method:

```scala
final override def equals(other: Any) = {
    other.isInstanceOf[Item] && {
        val that = other.asInstanceOf[Item]
        description == that.description && price == that.price
    }
}

// or better, use pattern matching:
final override def equals(other: Any) = other match {
    case that: Item => description == that.description && price == that.price
    case _ => false
}
```

When you define equals, remember to define hashCode as well. The hash code should be computed only from the fields that you use in the equality check, so that equal objects have the same hash code. In the Item example, combine the hash codes of the fields.

```scala
final override def hashCode = (description, price).##
```

The `##` method is a null-safe version of the hashCode method that yields 0 for null instead of throwing an exception.

In an application program, you don’t generally call eq or equals. Simply use the == operator. For reference types, it calls equals after doing the appropriate check for null operands.

#### 8.13 Value Classes

Some classes have a single field, such as the wrapper classes for primitive types, and the “rich” or “ops” wrappers that Scala uses to add methods to existing types. It is inefficient to allocate a new object that holds just one value. Value classes allow you to define classes that are “inlined,” so that the single field is used directly.

A value class has these properties: 

1. The class extends AnyVal. 2. Its primary constructor has exactly one parameter, which is a val, and no body. 3. The class has no other fields or constructors. 4. The automatically provided equals and hashCode methods compare and hash the underlying value.

```scala
// define a value class that wraps a “military time” value:
class MilTime(val time: Int) extends AnyVal {
	def minutes = time % 100
	def hours = time / 100
	override def toString = f"$time04d"
}
```

#### 8.14 Exercise Tips

### Chapter 9 Files and Regular Expressions

#### 9.1 Reading Lines

To read all lines from a file, call the `getLines` method on a `scala.io.Source` object.

```scala
import scala.io.Source
val source = Source.fromFile("myfile.txt", "UTF-8")
val lineIterator = source.getLines // result in an iterator
for (l <- lineIterator) process l
```

Or you can put the lines into an array or array buffer by applying the toArray or toBuffer method to the iterator:

```scala
val lines = source.getLines.toArray

// Sometimes, you just want to read an entire file into a string.
val contents = source.mkString
```

Call close when you are done using the Source object.

#### 9.2 Reading Characters

To read individual characters from a file, you can use a Source object directly as an iterator since the `Source` class extends `Iterator[Char]`:

```scala
for (c <- source) process c
```

if your file isn’t large, you can just read it into a string and process that:

```scala
val contents = source.mkString
```

#### 9.3 Reading Tokens and Numbers

Here is a quick-and-dirty way of reading all whitespace-separated tokens in a source:

```scala
val tokens = source.mkString.split("\\s+")
```

To convert a string into a number, use the toInt or toDouble method. For example, if you have a file containing floating-point numbers, you can read them all into an array by

```scala
val numbers = for (w <- tokens) yield w.toDouble
// or
val number = tokens.map(_.toDouble)
```

Finally, note that you can read numbers from scala.io.StdIn:

```scala
print("How old are you?")
val age = Scala.io.readInt() // or use readDouble or readLong
```

#### 9.4 Reading from URLs and Other Sources

The Source object has methods to read from sources other than files:

```scala
val source1 = Source.fromURL("http://horstmann.com", "UTF-8")
val source2 = Source.fromString("Hello, World!")
 // Reads from the given string—useful for debugging
val source3 = Source.stdin
 // Reads from standard input
```

#### 9.5 Reading Binary Files

Scala has no provision for reading binary files. You’ll need to use the Java library. Here is how you can read a file into a byte array:

```scala
val file = new File(filename)
val in = new FileInputStream(file)
val bytes = new Array[Byte](file.length.toInt)
in.read(bytes)
in.close()
```

#### 9.6 Writing Text Files

Scala has no built-in support for writing files. To write a text file, use a java.io.PrintWriter, for example:

```scala
val out = new PrintWriter("numbers.txt")
for (i <- 1 to 100) out.println(i)
out.close()
```

#### 9.7 Visiting Directories

Check the book...

#### 9.8 Serialization

Here is how you declare a serializable class in Java and Scala.

```scala
//Java:
public class Person implements java.io.Serializable {
 private static final long serialVersionUID = 42L;
 ...
}
//Scala:
@SerialVersionUID(42L) class Person extends Serializable
```

#### 9.9 Process Control

Check the book...

#### 9.10 Regular Expressions

When you process input, you often want to use regular expressions to analyze it. The `scala.util.matching.Regex` class makes this simple. To construct a Regex object, use the r method of the String class:

```scala
val numPattern = "[0-9]+".r
// If the regular expression contains backslashes or quotation marks, then it is a
// good idea to use the “raw” string syntax, """...""".
val wsnumwsPattern = """\s+[0-9]+\s+""".r
```

The `findAllIn` method returns an `Iterator[String]` through all matches. You can use it in a for loop:

```scala
for (matchString <- numPattern.findAllIn("99 bottles, 98 bottles"))
	println(matchString)
// Alternatively, turn the iterator into an array:
val matches = numPattern.findAllIn("99 bottles, 98 bottles").toArray
// To find the first match in a string, use findFirstIn.
val firstMatch = wsnumwsPattern.findFirstIn("99 bottles, 98 bottles")
```

#### 9.11 Regular Expression Groups

Groups are useful to get subexpressions of regular expressions. Add parentheses around the subexpressions that you want to extract,

```scala
val numitemPattern = "([0-9]+)([a-z]+)".r
```

#### 9.12 Exercise Tips

### Chapter 10 Traits

#### 10.1 Why No Multiple Inheritance?

#### 10.2 Traits as Interfaces

#### 10.3 Traits with Concrete Implementations

#### 10.4 Objects with Traits

#### 10.5 Layered Traits

#### 10.6 Overriding Abstract Methods in Traits

#### 10.7 Traits for Rich Interfaces

#### 10.8 Concrete Fields in Traits

#### 10.9 Abstract Fields in Traits

#### 10.10 Trait Construction Order

#### 10.11 Initializing Trait Fields

#### 10.12 Traits Extending Classes

#### 10.13 Self Types

#### 10.14 What Happens under the Hood

#### 10.15 Exercise Tips

### Chapter 11 Operators

This chapter covers in detail implementing your own operators—methods with ptg20087099 the same syntax as the familiar mathematical operators. Operators are often used to build domain-specific languages—minilanguages embedded inside Scala.

#### 11.1 Identifiers

The names of variables, functions, classes, and so on are collectively called `identifiers`. As in Java, Unicode characters are allowed. 

We can also use:

- The ASCII characters ! # % & * + - / : < = > ? @ \ ^ | ~ that are not letters, digits, underscore, the .,; punctuation marks, parentheses () [] {}, or quotation marks ' ` ".

- Unicode mathematical symbols or other symbols from the Unicode categories Sm and So. For example, ** and \sqrt are valid identifiers.

- Finally, you can include just about any sequence of characters in backquotes. For example,

  ```scala
  val `val` = 42
  ```

#### 11.2 Infix Operators

We can write```a identifier b``` where identifier denotes a method with two parameters (one implicit, one explicit). For example, the expression

```scala
1 to 10
// is actually a method call 
1.to(10)
```

This is called an infix(中缀) expression because the operator is between the arguments. The operator can contain letters, as in to, or it can contain operator characters—for example,

```scala
1 -> 10
// is actually a method call
1.->(10)
```

To define an operator in your own class, simply define a method whose name is that of the desired operator. For example, here is a Fraction class that multiplies two fractions according to the law

```scala
// (n1 / d1) × (n2 / d2) = (n1n2 / d1d2)
class Fraction(n: Int, d: Int) {
    private val num = ...
    private val den = ...
    ...
    def *(other:Fraction) = new Fraction(num * other.num, den * other.den)
}
```

#### 11.3 Unary Operators

Infix operators are binary operators—they have two parameters. An operator with one parameter is called a unary operator.

- The four operators `+, -, !, ~` are allowed as prefix operators, appearing before their arguments. They are converted into calls to methods with the name unary_operator. For example, `-a` means the same as `a.unary_-`.

- If a unary operator follows its argument, it is a postfix operator. The expression`a identifier` is the same as the method call `a.identifier()`. For example, 

  ```scala
  42 toString
  // is the same as
  42.toString()
  ```

#### 11.4 Assignment Operators

An assignment operator has the form `operator=`, and the expression `a operator= b` means the same as `a = a operator b`.

- <=, >=, and != are not assignment operators.
- An operator starting with an = is never an assignment operator (==, ===, =/=, and so on).
- If a has a method called operator=, then that method is called directly.

#### 11.5 Precedence

 Scala can have arbitrary operators, so it uses a scheme that works for all operators, while also giving the familiar precedence order to the standard ones. 

Except for assignment operators, the precedence is determined by the first character of the operator.

![image-20210202095753428](C:\Users\姜良泽\AppData\Roaming\Typora\typora-user-images\image-20210202095753428.png)

in the same row yield operators with the same precedence. For example, `+` and `->`have the same precedence. 

Postfix operators have lower precedence than infix operators: 

- `a infixOp bpostfixOp` is the same as `(a infixOp b)postfixOp`

#### 11.6 Associativity

When you have a sequence of operators of the same precedence, the associativity determines whether they are evaluated left-to-right or right-to-left. For example, in the expression 17 – 2 – 9, one computes (17 – 2) – 9. The – operator is left-associative.

In Scala, all operators are left-associative except for

- operators that end in a colon (:)
- assignment operators

In particular, the `::` operator for constructing lists is right-associative. For example, `1 :: 2 :: Nil` means `1 :: (2 :: Nil)`.

```scala
val a = List(1, 2, 3)
val b = List(4, 5, 6)

a::b // List(List(1, 2, 3), 4, 5, 6)
a++b // List(1, 2, 3, 4, 5, 6)
```

A right-associative binary operator is a method of its second argument. For example, `2 :: Nil` means `Nil.::(2)`

#### 11.7 The apply and update Methods

- Scala lets you extend the function call syntax `f(arg1, arg2, ...)` to values other than functions. If f is not a function or method, then this expression is equivalent to the call `f.apply(arg1, arg2, ...)`.
- When it occurs to the left of an assignment. The expression `f(arg1, arg2, ...) = value` corresponds to the call `f.update(arg1, arg2, ..., value)`

This mechanism is used in arrays and maps. For example,

```scala
val scores = new scala.collection.mutable.HashMap[String, Int]
scores("Bob") = 100
val bobScore = scores("Bob")
```

The apply method is also commonly used in companion objects to construct objects without calling new. For example, consider a Fraction class.

```scala
class Fraction(n: Int, d:Int) {
    ...
}

object Fraction {
    def apply(n: Int, d: Int) = new Fraction(n, d)
}
```

Because of the apply method, we can construct a fraction as `Fraction(3, 4)` instead of `new Fraction(3, 4)`. That sounds like a small thing, but if you have many Fraction values, it is a welcome improvement: `val result = Fraction(3, 4) * Fraction(2, 5)`

#### 11.8 Extractors

An extractor is an object with an unapply method. You can think of the unapply method as the opposite of the apply method of a companion object. An `apply` method takes construction parameters and turns them into an object. An `unapply` method takes an object and extracts values from it—usually the values from which the object was constructed.

Consider the Fraction class from the preceding section. The apply method makes a fraction from a numerator and denominator. An unapply method retrieves the numerator and denominator. You can use it in a variable definition

```scala
var Fraction(a, b) = Fraction(3, 4) * Fraction(2, 5)
// a, b are initialized with the numerator and denominator of the result
```

or a pattern match

```scala
case Fraction(a,b) => ... // a, b are bound to the numerator and denominator
```

A declaration val `Fraction(a, b) = f` becomes 

```scala
val tupleOption = Fraction.unapply(f)
if (tupleOption == None) throw new MatchError
// tupleOption is Some((t1, t2))
val a = t1
val b = t2
```

In the preceding example, the apply and unapply methods are inverses of one another. However, that is not a requirement. You can use extractors to extract information from an object of any type.

#### 11.9 Extractors with One or No Arguments

In Scala, there are no tuples with one component. If the unapply method extracts a single value, it should just return an Option of the target type. For example,

```scala
object Number {
    def unapply(input: String): Option[Int] = 
    	try {
            Some(input.trim.toInt)
        } catch {
            case ex: NumberFormatException => None
        }
}
```

With this extractor, you can extract a number from a string: `val Number(n) = "1729"`

#### 11.10 The unapplySeq Method

Check the book...

#### 11.11 Dynamic Invocation

Check the book...

#### 11.12 Exercise Tips

### Chapter 12 Higher-Order Functions

Scala mixes object orientation with functional features. In a functional programming language, functions are first-class citizens that can be passed around and manipulated just like any other data types. This is very useful whenever you want to pass some action detail to an algorithm. In a functional language, you just wrap that detail into a function that you pass as a parameter.

#### 12.1 Functions as Values

In Scala, a function is a first-class citizen, just like a number. You can store a function in a variable:

```scala
import scala.math._
val num = 3.14
val fun = ceil _
// this code sets num to 3.14 and fun to the ceil function
```

The `_` behind the ceil function indicates that you really meant the function, and you didn’t just forget to supply the arguments.

Technically, the _ turns the ceil method into a function. In Scala, you cannot manipulate methods, only functions

Here is how to call the function stored in fun: `fun(num) // 4.0` As you can see, the normal function call syntax is used. The only difference is that fun is a variable containing a function, not a fixed function.

Here is how you can give `fun` to another function:

```scala
Array(3.14, 1.42, 2.0).map(fun) // Array(4.0, 2.0, 2.0)
```

The map method accepts a function, applies it to all values in an array, and returns an array with the function values.

#### 12.2 Anonymous Functions

In Scala, you don’t have to give a name to each function, just like you don’t have to give a name to each number.

```scala
(x: Double) => 3 * x

// Of course, we can store this function in a variable
val triple = (x: Double) => 3 * x
// this is the same as
def triple(x: Double) = 3 * x

// but we don't need to name it
Array(3.14, 1.42, 2.0).map((x: Double) => 3 * x)
// or
Array(3.14, 1.42, 2.0) map {(x: Double) => 3 * x }
```

#### 12.3 Functions with Function Parameters

In this section, you will see how to implement a function that takes another function as a parameter.

```scala
def valueAtOneQuarter(f: (Double) => Double) = f(0.25)
// parameter can be any function receiving and returning a Double

// Another example
valueAtOneQuarter(ceil _)
valueAtOneQuarter(sqrt _)
// The type of valueAtOneQuarter is ((Double) => Double) => Double
```

Since valueAtOneQuarter is a function that receives a function, it is called a higher-order function.

A higher-order function can also produce a function.

```scala
def mulBy(factor: Double) = (x: Double) => factor * x
```

For example, mulBy(3) returns the function (x : Double) => 3 * x which you have seen in the preceding section. The power of `mulBy` is that it can deliver functions that multiply by any amount

#### 12.4 Parameter Inference

When you pass an anonymous function to another function or method, Scala helps you out by deducing types when possible. For example, you don’t have to write

```scala
valueAtOneQuarter((x: Double) => 3 * x) // 0.75

// because it has the type ((Double) => Double) => Double
def valueAtOneQuarter(f: (Double) => Double) = f(0.25)
```

Since the valueAtOneQuarter method knows that you will pass in a (Double) => Double function, you can just write

```scala
valueAtOneQuarter((x) => 3 * x)
```

As a special bonus, for a function that has just one parameter, you can omit the () around the parameter:

```scala
// we can further write it as
valueAtOneQuarter(x => 3 * x)
```

If a parameter occurs only once on the right-hand side of the =>, you can replace it with an underscore:

```scala
// Finally
valueAtOneQuarter(3 * _)
```

Keep in mind that these shortcuts only work when the parameter types are known.

```scala
val fun = 3 * _ // Error: Can't infer types
val fun = 3 * (_: Double) // OK
val fum: (Double) => Double = 3 * _
```

#### 12.5 Useful Higher-Order Functions

You have seen `map`, which applies a function to all elements of a collection and returns the result. Here is a quick way of producing a collection containing 0.1, 0.2, . . . , 0.9:

```scala
(1 to 9).map(0.1 * _)
```

Another example,

```scala
(1 to 9).map("*" * _).foreach(println _)
```

Here, we also use foreach, which is like map except that its function doesn’t return a value. The foreach method simply applies the function to each argument.

The filter method yields all elements that match a particular condition. For example, here’s how to get only the even numbers in a sequence:

```scala
(1 to 9).filter(_ % 2 == 0)
```

Of course, that’s not the most efficient way of getting this result.

The `reduceLeft` method takes a `binary` function—that is, a function with two parameters—and applies it to all elements of a sequence, going from left to right. For example,

```scala
(1 to 9).reduceLeft(_ * _) // Each underscore denotes a separate parameter.
// (...((1 * 2) * 3) * ... * 9)
```

You also need a binary function for sorting. For example,

```scala
"Mary had a little lamb".split(" ").sortWith(_.length < _.length)
```

#### 12.6 Closures

In Scala, you can define a function inside any scope: in a package, in a class, or even inside another function or method. In the body of a function, you can access any variables from an enclosing scope. That may not sound so remarkable, but note that your function may be called when the variable is no longer in scope.

Such a function is called a closure. A closure consists of code together with the definitions of any nonlocal variables that the code uses.

These functions are actually implemented as objects of a class, with an instance variable factor and an apply method that contains the body of the function.

#### 12.7 SAM Conversions (Single Abstract Method)

In Scala, you pass a function as a parameter whenever you want to tell another function what action to carry out.

Confused about this section...

#### 12.8 Currying

Currying (named after logician Haskell Brooks Curry) is the process of turning a function that takes two arguments into a function that takes one argument. That function returns a function that consumes the second argument.

```scala
// a function takes two arguments
val mul = (x: Int, y: Int) => x * y
// a function takes one arguments
val mulOneAtATime = (x: Int) => ((y: Int) => x * y)
// to call
mulOneAtATime(6)(7)
mul(6, 7)
// When you use def, there is a shortcut for defining such curried methods in Scala:
def mulOneAtATime(x: Int)(y: Int) = x * y
```

Sometimes, you can use currying for a method parameter so that the type inferencer has more information.  For example,

```scala
val a = Array("Hello", "World")
val b = Array("hello", "world")
a.corresponds(b)(_.equalsIgnoreCase(_))
```

Note that the function `_.equalsIgnoreCase(_)` is passed as a curried parameter, in a separate set of (...). When you look into the Scaladoc, you will see that corresponds is declared as

```scala
def corresponds[B](that: Seq[B])(p: (A, B) => Boolean): Boolean 
```

The that sequence and the predicate function p are separate curried parameters. The type inferencer can figure out what B is from the type of that, and then it can use that information when analyzing the function that is passed for p.

#### 12.9 Control Abstraction

In Scala, one can model a sequence of statements as a function with no parameters or return value. For example, here is a function that runs some code in a thread:

```scala
def runInThread(block: () => Unit) {
    new Thread {
        override def run () { block() }
    }.start()
}
```

The code is given as a function of type () => Unit. However, when you call this function, you need to supply an unsightly () =>:

```scala
runInThread{ () => println("Hi"); Thread.sleep(10000); println("Bye") }
```

To avoid the () => in the call, use the call by name notation: Omit the (), but not the =>, in the parameter declaration and in the call to the parameter function:

```scala
def runInThread(block: => Unit) {
    new Thread {
        override def run() { block }
    }.start()
}

// To call
runInThread { println("Hi"); Thread.sleep(10000); println("Bye") }
```

we can innovate a bit and define an until statement that works like while, but with an inverted condition:

```scala
def until(condition: => Boolean)(block: => Unit) {
    if (!condition) {
        block
        until(condition)(block)
    }
}

// To call
var x = 10
until (x == 0) {
    x -= 1
    println(x)
}
```



#### 12.10 The `return` Expression

In Scala, you don’t use a return statement to return function values. The return value of a function is simply the value of the function body.

However, you can use return to return a value from an anonymous function to an enclosing named function. This is useful in control abstractions. For example, consider this function:

```scala
def indexOf(str: String, ch: Char): Int = {
    var i = 0
    until (u == str.length) {
        if (str(i) == ch) return i
        i += 1
    }
    return -1
}
```

Here, the anonymous function { if (str(i) == ch) return i; i += 1 } is passed to until. When the return expression is executed, the enclosing named function indexOf terminates and returns the given value. 

If you use return inside a named function, you need to specify its return type. For example, in the indexOf function above, the compiler was not able to infer that it returns an Int. 

The control flow is achieved with a special exception that is thrown by the return expression in the anonymous function, passed out of the until function, and caught in the indexOf function.

#### 12.11 Exercise Tips

1. ```scala
   def values(fun: (Int) => Int, low: Int, high: Int) = for (i <- low to high) yield (i,fun(i))
   ```

2. ```scala
   max_value = a.reduceLeft((x, y) => if (x > y) x else y)
   ```

3. ```scala
   def factorial(num: Int) = (1 to num).reduceLeft(_ * _)
   ```

4. ```scala
   def largest(fun: (Int) => Int, inputs: Seq[Int]) = inputs.map(fun).reduceLeft((x, y) => if (x > y) x else y)
   ```

5. ```scala
   def largest_index(fun: (Int) => Int, inputs: Seq[Int]) = inputs.map(x => (x, fun(x)).reduceLeft((x, y) => if (x._2 > y._2) x else y)._1
   ```

### Chapter 13 Collections

Key points:

- All collections extend the `Iterable` trait.
- The three major categories of collections are sequences, sets and maps
- Scala has mutable and immutable versions of most collections
- A Scala list is either empty, or it has a head and a tail which is again a list.
- Sets are unordered collections.
- \+ adds an element to an unordered collection; +: and :+ prepend or append to a sequence; ++ concatenates two collections; - and -- remove elements.
- Use a `LinkedHashSet` to retain the insertion order or a `SortedSet` to iterate in sorted order.
- Mapping, folding, and zipping are useful techniques for applying a function or operation to the elements of a collection

#### 13.1 The Main Collections Traits

An Iterable is any collection that can yield an Iterator with which you can access all elements in the collection:

```scala
val coll = ... // some Iterable
val iter = coll.iterator
while (iter.hasNext)
	do something with iter.next()
```

A `Seq` is an ordered sequence of values, such as an array or list. An `IndexedSeq` allows fast random access through an integer index. For example, an `ArrayBuffer` is indexed but a linked list is not.

A Set is an unordered collection of values. In a SortedSet, elements are always visited in sorted order.

A Map is a set of (key, value) pairs. A SortedMap visits the entries as sorted by the keys.

There are methods `toSeq, toSet, toMap`, and so on, as well as a `generic to[C]` method, that you can use to translate between collection types.

```scala
val coll = Seq(1, 1, 2, 3, 5, 8. 13)
val set = coll.toSet
val buffer = coll.to[ArrayBuffer]
```

#### 13.2 Mutable and Immutable Collections

Scala supports both mutable and immutable collections. For example, there is a `scala.collection.mutable.Map` and a `scala.collection.immutable.Map`. Both have a common supertype scala.collection.Map (which, of course, contains no mutation operations).

Moreover, the `scala` package and the `Predef` object, which are always imported, have type aliases `List, Set, and Map` that refer to the immutable traits. For example, `Predef.Map` is the same as `scala.collection.immutable.Map`.

#### 13.3 Sequences

![immutable seq](C:\Users\姜良泽\AppData\Roaming\Typora\typora-user-images\image-20210205082814754.png)

A `Vector` is the immutable equivalent of an `ArrayBuffer`: an indexed sequence with fast random access. Vectors are implemented as trees where each node has up to 32 children. For a vector with one million elements, one needs four layers of nodes. Accessing an element in such a list will take 4 hops, whereas in a linked list it would take an average of 500,000.  

A `Range` represents an integer sequence, such as 0,1,2,3,4,5,6,7,8,9 or 10,20,30. Of course a Range object doesn’t store all sequence values but only the start, end, and increment. You construct Range objects with the `to` and `until` methods.

![image-20210205085459617](C:\Users\姜良泽\AppData\Roaming\Typora\typora-user-images\image-20210205085459617.png)

#### 13.4 Lists

In Scala, a list is either Nil (that is, empty) or an object with a head element and a tail that is again a list.

```scala
val digits = List(4,2)
```

The value of `digits.head` is 4, and `digits.tail` is List(2). Moreover, `digits.tail.head` is 2 and `digits.tail.tail` is `Nil`.

The :: operator makes a new list from a given head and tail. For example,

```scala
9 :: List(4,2) // List(9, 4, 2)
// also
9 :: 4 :: 2 :: Nil
// Note that :: is right-associative. With the :: operator, lists are constructed from
// the end:
9 :: (4 :: (2 :: Nil))
```

In Java or C++, one uses an iterator to traverse a linked list. You can do this in Scala as well, but it is often more natural to use recursion. For example, the following function computes the sum of all elements in a linked list of integers:

```scala
def sum(lst: List[Int]): Int = 
	if (lst == Nil) 0 else lst.head + sum(lst.tail)
// or use pattern matching
def sum(lst: List[Int]): Int = lst match {
    case Nil => 0
    case h :: t => h + sum(t) // h is lst.head, t is lst.tail
}
// Note the :: operator in the second pattern. It “destructures” the list into head
// and tail.

// we can directly use the library
List(9,4,2).sum
```

If you want to mutate list elements in place, you can use a ListBuffer, a data structure that is backed by a linked list with a reference to the last node. This makes it efficient to add or remove elements at either end of the list.

However, adding or removing elements in the middle is not efficient.  Of course, removing multiple elements by their index positions is very inefficient in a linked list. Your best bet is to generate a new list with the result

#### 13.5 Sets

A set is a collection of distinct elements. Trying to add an existing element has no effect:

```scala
Set(2, 0, 1) + 1 // Set(2, 0, 1)
```

Unlike lists, sets do not retain the order in which elements are inserted. By default, sets are implemented as hash sets in which elements are organized by the value of the hashCode method.

For example, if you iterate over `Set(1, 2, 3, 4, 5, 6)` the elements are visited in the order `5 1 6 2 3 4` You may wonder why sets don’t retain the element order. It turns out that you can find elements `much faster` if you allow sets to reorder their elements. Finding an element in a hash set is much faster than in an array or list.

A linked hash set remembers the order in which elements were inserted. It keeps a linked list for this purpose.

```scala
val weekdays = scala.collection.mutable.LinkedHashSet("MO", "TU", "We", "Th", "Fr")
// if you want to iterate over elements in sorted order, use a sorted set
val numbers = scala.collection.mutable.SortedSet(1, 2, 3, 4, 5)
```

The contains method checks whether a set contains a given value. The subsetOf method checks whether all elements of a set are contained in another set.

```scala
val digits = Set(1, 7, 2, 9)
digits contains 0 // False
Set(1,2) subsetOf digits // True
```

The `union, intersect, and diff` methods carry out the usual set operations. If you prefer, you can write them as `|, &, and &~`. You can also write `union as ++ and difference as --`.

#### 13.6 Operators for Adding or Removing Elements

![image-20210205091516073](C:\Users\姜良泽\AppData\Roaming\Typora\typora-user-images\image-20210205091516073.png)

Generally, `+` is used for adding an element to an unordered collection, while `+:` and `:+` add an element to the beginning or end of an ordered collection.

![image-20210205091743409](C:\Users\姜良泽\AppData\Roaming\Typora\typora-user-images\image-20210205091743409.png)

These operators return new collections (of the same type as the original ones) without modifying the original. Mutable collections have a += operator that mutates the left-hand side.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     

#### 13.7 Common Methods

![image-20210205092223355](C:\Users\姜良泽\AppData\Roaming\Typora\typora-user-images\image-20210205092223355.png)

![image-20210205092719991](C:\Users\姜良泽\AppData\Roaming\Typora\typora-user-images\image-20210205092719991.png)

![image-20210205092740030](C:\Users\姜良泽\AppData\Roaming\Typora\typora-user-images\image-20210205092740030.png)

![image-20210205092759076](C:\Users\姜良泽\AppData\Roaming\Typora\typora-user-images\image-20210205092759076.png)

#### 13.8 Mapping a Function

You may want to transform all elements of a collection. The map method applies a function to a collection and yields a collection of the results.

```scala
val names = List("Peter", "Paul", "Mary")
names.map(_.toUpperCase) // List("PETER", "PAUL", "MARY")
// the same as
for (n <- names) yield n.toUpperCase
```

If the function yields a collection instead of a single value, you may want to concatenate all results. In that case, use flatMap.

```scala
def ulcase(s: String) = Vector(s.toUpperCase(), s.toLowerCase())
names.map(ulcase) // List(Vector("PETER", "Peter"), Vector("PAUL", "paul"), Vector("MARY", "mary"))
names.flatMap(ulcase) //  List("PETER", "peter", "PAUL", "paul", "MARY", "mary")
```

Another example,

```scala
for (i <- 1 to 10) yield i * i
// is translated to
(1 to 10).map(i => i * i)

for (i <- 1 to 10; j <- 1 to i) yield i * j
// is translated to 
(1 to 10).flatMap(i => (1 to i).map(j => i * j))
```

The `transform` method is the in-place equivalent of map. It applies to mutable collections, and replaces each element with the result of a function. For example, the following code changes all buffer elements to uppercase:

```scala
val buffer = ArrayBuffer("Peter", "Paul", "Mary")
buffer.transform(_.toUppercase)
```

If you just want to apply a function for its side effect and don’t care about the function values, use foreach:

```scala
names.foreach(println)
```

The`collect` method works with partial functions—functions that may not be defined for all inputs. It yields a collection of all function values of the arguments on which it is defined. For example,

```scala
"-3+4".collect { case '+' => 1; case '-' => 1 } // Vector(-1, 1)
```

The `groupBy` method yields a map whose keys are the function values, and whose values are the collections of elements whose function value is the given key. For example,

```scala
val words = ...
val map = words.groupBy(_.substring(0, 1).toUpper)
```

builds a map that maps "A" to all words starting with A, and so on.

#### 13.9 Reducing, Folding and Scanning

The `map` method applies a unary function to all elements of a collection. The methods that we discuss in this section combine elements with a binary function. The call `c.reduceLeft(op)` applies op to successive elements.

```scala
op(op(coll(0), coll(1)), coll(2))...

List(1, 7, 2, 9).reduceLeft(_ - _)
// (((1-7)-2)-9) = -17
List(1, 7, 2, 9).reduceRight(_ - _)
// 1-(7-(2-9)) = -13
```

Often, it is useful to start the computation with an initial element other than the initial element of a collection. The call coll.foldLeft(init)(op) computes

```scala
op(op(init, coll(0)), coll(1))...

List(1, 7, 2, 9).foldLeft(0)(_ - _)
0 - 1 - 7 - 2 - 9 = -19
```

The initial value and the operator are separate “curried” parameters so that Scala can use the type of the initial value for type inference in the operator. For example, in List(1, 7, 2, 9).foldLeft("")(_ + _), the initial value is a string, so the operator must be a function (String, Int) => String.

You can also write the foldLeft operation with the `/:` operator, like this: `(0 /: List(1, 7, 2, 9))(_ - _) `The /: is supposed to remind you of the shape of the tree.

There is a `foldRight` or `:\ `variant as well.

Folding is sometimes attractive as a replacement for a loop. Suppose, for example, we want to count the frequencies of the letters in a string. One way is to visit each letter and update a mutable map.

```scala
val freq = scala.collection.mutable.Map[Char, Int]()
for (c <- "Mississippi") freq(c) = freq.getOrElse(c, 0) + 1
// Now freq is Map('i' -> 4, 'M' -> 1, 's' -> 4, 'p' -> 2)
```

Here is another way of thinking about this process. At each step, combine the frequency map and the newly encountered letter, yielding a new frequency map.

```scala
(Map[Char, Int]() /: "Mississippi") {
    (m, c) => m + (c -> (m.getOrElse(c, 0) + 1))
}
```

Finally, the scanLeft and scanRight methods combine folding and mapping. You get a collection of all intermediate results:

```scala
(1 to 10).scanLeft(0)(_ + _)
// Vector(0, 1, 3, 6, 10, 15, 21, 28, 36, 45, 55)
```

#### 13.10 Zipping

 Sometimes, you have two collections, and you want to combine corresponding elements.

```scala
val prices = List(5.0, 20.0, 9.95)
val quantities = List(10,2,1)
prices zip quantities //  List[(Double, Int)] = List((5.0, 10), (20.0, 2), (9.95, 1))

// Now it is easy to apply a function to each pair.
(prices zip quantities) map { p => p._1 * p._2 }
// List(50.0, 40.0, 9.95)
// To compute the total price
((prices zip quantities) map { p=> p._1 * p._2 }) sum
```

If one collection is shorter than the other, the result has as many pairs as the shorter collection.

The zipAll method lets you specify defaults for the shorter list:

```scala
List(5.0, 20.0, 9.95).zipAll(List(10, 2), 0.0, 1)
// List((5.0, 10), (20.0, 2), (9.95, 1))
```

The zipWithIndex method returns a list of pairs where the second component is the index of each element

```scala
"Scala".zipWithIndex
// Vector(('S', 0), ('c', 1), ('a', 2), ('l', 3), ('a', 4))
```

#### 13.11 Iterators

You can obtain an iterator from a collection with the iterator method. This isn’t as common as in Java or C++ because you can usually get what you need more easily with one of the methods from the preceding sections. 

However, iterators are useful for collections that are expensive to construct fully. For example, Source.fromFile yields an iterator because it might not be efficient to read an entire file into memory. There are a few Iterable methods that yield an iterator, such as grouped or sliding.

When you have an iterator, you can iterate over the elements with the next and hasNext methods.

```scala
while (iter.hasNext)
	do something with iter.next()

for (elem <- iter)
	do something with elem
```

Sometimes, you want to be able to look at the next element before deciding whether to consume it. In that case, use the buffered method to turn an Iterator into a BufferedIterator. The head method yields the next element without advancing the iterator.

```scala
val iter = scala.io.Source.fromFile(filename).buffered
while (iter.hasNext && iter.head.isWhitespece) iter.next
// Now iter points to the first non-whitespace character
```

After calling a method such as map, filter, count, sum, or even length, the iterator is at the end of the collection, and you can’t use it again. With other methods, such as find or take, the iterator is past the found element or the taken ones.

#### 13.12 Streams

In the preceding sections, you saw that an iterator is a “lazy” alternative to a collection.

However, iterators are fragile. Each call to next mutates the iterator. Streams offer an immutable alternative. A stream is an immutable list in which the tail is computed lazily—that is, only when you ask for it.

For example, 

```scala
def numsFrom(n: BigInt): Stream[BigInt] = n #:: numsFrom(n + 1)
```

The `#::` operator is like the `::` operator for lists, but it constructs a stream.

```scala
val tenOrMore = numsFrom(10) // Stream(10, ?)
tenOrMore.tail.tail.tail // Stream(13, ?)

// Stream methods are executed lazily. For example, 
val squares = numsFrom(1).map(x => x * x) // Stream(1, ?)
// we have to call squares.tail to force evaluation of the next entry
```

If you want to get more than one answer, you can invoke take followed by force, which forces evaluation of all values. For example,

```scala
squares.take(5).force // Stream(1, 4, 9, 16, 25)
squares.force // No! OutOfMemoryError
```

You can construct a stream from an iterator. A stream caches the visited lines so you can revisit them

```scala
val words = Source.fromFile("/usr/share/dict/words").getLines.toStream
words // Stream(A, ?)
words(5) // Aachen
words // Stream(A, A's, AOL, AOL's, Aachen, ?)
```

#### 13.13 Lazy Views

In the preceding section, you saw that stream methods are computed lazily, delivering results only when they are needed. You can get a similar effect with other collections by applying the `view` method. This method yields a collection on which methods are applied lazily.

```scala
val palindromicSquares = (1 to 1000000).view
	.map(x => x * x).filter(x => x.toString == x.toString.reverse)
```

yields a collection that is unevaluated. (Unlike a stream, not even the first element is evaluated.) When you call

```scala
palindromicSquares.take(10).mkString(",")
```

then enough squares are generated until ten palindromes have been found, and then the computation stops. Unlike streams, views do not cache any values. If you call palindromicSquares.take(10).mkString(",") again, the computation starts over.

- The apply method forces evaluation of the entire view. Instead of calling lazyView(i), call `lazyView.take(i).last`.
- As with streams, use the `force` method to force evaluation of a lazy view. You get back a collection of the same type as the original.

When you obtain a view into a slice of a mutable collection, any mutations affect the original collection.

```scala
ArrayBuffer buffer = ...
buffer.view(10, 20).transform(x => 0)
// clears the given slice and leaves the other elements unchanged.
```

#### 13.14 Interoperability with Java Collections

Check the book...

#### 13.15 Parallel Collections

 Scala offers a particularly attractive solution for manipulating large collections. Such tasks often parallelize naturally. For example, to compute the sum of all elements, multiple threads can concurrently compute the sums of different sections; in the end, these partial results are summed up. Of course it is troublesome to schedule these concurrent activities—but with Scala, you don’t have to. If coll is a large collection, then

```scala
coll.par.sum // compute the sum concurrently
```

The `par` method produces a `parallel implementation` of the collection. That implementation parallelizes the collection methods whenever possible. For example,

```scala
coll.par.count(_ % 2 == 0)
```

counts the even numbers in coll by evaluating the predicate on subcollections in parallel and combining the results. You can parallelize a for loop by applying .par to the collection over which you iterate, like this:

```scala
for (i <- (0 until 100000).par) print(s" $i")
// the numbers are printed in the order they are produced by the threads
// working on the task.
```

If parallel computations mutate shared variables, the result is unpredictable. For example, do not update a shared counter:

```scala
var count = 0
for (c <- coll.par) { if (c % 2 == 0) count += 1} // Error!
```

The parallel collections returned by the par method belong to types that extend the ParSeq, ParSet, or ParMap traits. These are not subtypes of Seq, Set, or Map, and you cannot pass a parallel collection to a method that expects a sequential collection.

You can convert a parallel collection back to a sequential one with the seq method.

```scala
val result = coll.par.filter(p).seq
```

#### 13.16 Exercise Tips

1. ```scala
   // https://github.com/BasileDuPlessis/scala-for-the-impatient/blob/master/src/main/scala/com/basile/scala/ch13/Ex01.scala
   import scala.collection.mutable.Map
   import scala.collection.mutable.SortedSet
   object Ex1 extends App {
       def indexes(s: String) = {
           s.indices.foldLeft( Map[Char, SortedSet[Int]]() ) {
               (m, i) => m += ( s(i) -> (m.getOrElse(s(i), SortedSet[Int]()) += i))
           }
       }
   }
   ```

2. ```scala
   // the only difference comes to the operator
   import scala.collection.immutable.SortedMap
   import scala.collection.immutable.SortedSet
   object Ex2 extends App {
       def indexes(s: String) = {
           s.indices.foldLeft( SortedMap[Char, Set[Int]]() ) {
               (m, i) => m + ( s(i) -> (m.getOrElse(s(i), SortedSet[Int]()) + i))
           }
       }
   }
   ```

3. ```scala
   def mkString[T](s: Seq[T], seq: String = ", "): String = s.map(_.toString) reduceLeft(_.toString + seq + _.toString)
   ```

### Chapter14 Pattern Matching and Case Classes

Pattern matching is a powerful mechanism that has a number of applications:  switch statements, type inquiry, and “destructuring” (getting at the parts of complex expressions). Case classes are optimized to work with pattern matching.

#### 14.1 A Better Switch

The equivalent of the C-style switch statement in Scala:

```scala
var sign = ...
val ch: Char = ...
ch match {
    case '+' => sign = 1
    case '-' => sign =-1
    case _ => sign =0 // catch-all pattern
}

// can be written as
sign = ch match {
    case '+' => 1
    case '-' => -1
    case _ => 0
}

// use | to separate multiple alternatives
prefix match {
    case "0" | "0x" | "0X" =>...
    ...
}

// use match statement with any types
color match {
    case Color.RED => ...
    case Color.BLACK => ...
    ...
}
```

If no pattern matches, a MatchError is thrown.

#### 14.2 Guards

Suppose we want to extend our example to match all digits. In a C-style switch statement, you would simply add multiple case labels, for example case '0': case '1': ... case '9':. (Except that, of course, you can’t use ... but must write out all ten cases explicitly.) In Scala, you add a `guard clause` to a pattern:

```scala
ch match {
    case _ if Character.isDigit(ch) => digit = Character.digit(ch, 10)
    case '+' => sign = 1
    case '-' => sign = -1
    case _ => sign =0
}
```

The guard clause can be any Boolean condition.

#### 14.3 Variables in Patterns

If the case keyword is followed by a variable name, then the match expression is assigned to that variable.

```scala
str(i) match {
    case '+' => sign = 1
    case '-' => sign = -1
    case ch => digit = Character.digit(ch, 10)
}

// use the variable name in a guard
str(i) match {
    case ch if Character.isDigit(ch) => digit = Character.digit(ch, 10)
    ...
}
```

#### 14.4 Type Patterns

You can match on the type of an expression:

```scala
obj match {
    case x: Int => x
    case s: String => Integer.parseInt(s)
    case _: BigInt => Int.MaxValue
    case _ => 0
}
```

In Scala, this form is preferred to using the `isInstanceOf` operator. Note the variable names in the patterns. In the first pattern, the match is bound to x as an Int, and in the second pattern, it is bound to s as a String. No `asInstanceOf` casts are needed!

#### 14.5 Matching Arrays, Lists and Tuples

To match an array against its contents, use Array expressions in the patterns, like this:

```scala
arr match {
    case Array(0) => "0"
    case Array(x, y) => s"$x, $y"
    case Array(0, _*) => "0 ..."
    case _ => "something else"
}
```

The first pattern matches the array containing 0. The second pattern matches any array with two elements, and it binds the variables x and y to the elements. The third pattern matches any array starting with zero.

If you want to bind a variable argument match _* to a variable, use the @ notation like this:

```scala
case Array(x, rest @ _*) => rest.min
```

You can match lists in the same way, with List expressions. Alternatively, you can use the :: operator:

```scala
lst match {
    case 0 :: Nil => "0"
    case x :: y :: Nil => s"$x, $y"
    case 0 :: tail => "0 ..."
    case _ => "something else"
}
```

With tuples, use the tuple notation in the pattern:

```scala
pair match {
    case (0, _) => "0 ..."
    case (y, 0) => s"$y 0"
    case _ => "neither is 0"
}
```

Again, note how the variables are bound to parts of the list or tuple. Since these bindings give you easy access to parts of a complex structure, this operation is called destructuring.

#### 14.6 Extractors

In the preceding section, you have seen how patterns can match arrays, lists, and tuples. These capabilities are provided by extractors—objects with an `unapply` or `unapplySeq` method that extract values from an object. The unapply method is provided to extract a fixed number of objects, while unapplySeq extracts a sequence whose length can vary. For example, consider the expression

```scala
arr match {
    case Array(x, 0) => x
    case Array(x, rest @ _*) => rest.min
    ...
}
```

The Array companion object is an extractor—it defines an unapplySeq method. That method is called with the expression that is being matched, not with what appear to be the parameters in the pattern.  In the first case, the match succeeds if the array has length 2 and the second element is zero. In that case, the initial array element is assigned to x.

#### 14.7 Patterns in Variable Declarations

In the preceding sections, you have seen how patterns can contain variables. You can use these patterns inside variable declarations. For example,

```scala
val (x, y) = (1, 2) // simultaneously define x as 1 and y as 2
```

That is useful for functions that return a pair, for example:

```scala
val (q, r) = BigInt(10) /% 3 // The /% method returns a pair containing the quotient and the remainder, which are captured in the variables q and r
```

#### 14.8 Patterns in `for ` Expressions

Traverse a map:

```scala
for ((k, v) <- System.getProperties())
	println(s"$k -> $v)
```

In a for expression, match failures are silently ignored. For example, the following loop prints all keys with empty value, skipping over all others:

```scala
for ((k, "") <- System.getProperties())
	println(k)
```

You can also use a guard. Note that the `if` goes after the `<-` symbol:

```scala
for ((k,v) <- System.getProperties() if v == "")
	println(k)
```

#### 14.9 Case Classes

Case classes are a special kind of classes that are optimized for use in pattern matching. In this example, we have two case classes that extend a regular (noncase) class:

```scala
abstract class Amount
case class Dollar(value: Double) extends Amount
case class Currency(value: Double, unit: String) extends Amount
```

You can also have case objects for singletons:

```scala
case object Nothing extends Amount
```

When we have an object of type Amount, we can use pattern matching to match the amount type and bind the property values to variables:

```scala
amt match {
    case Dollar(v) => s"$$$v"
    case Currency(_, u) => s"Oh noes, I got $u"
    case Nothing => ""
}
```

#### 14.10 The `copy` Method and Named Parameters

The copy method of a case class makes a new object with the same values as an existing one. For example,

```scala
val amt = Currency(29.95, "EUR")
val price = amt.copy()
```

By itself, that isn’t very useful—after all, a Currency object is immutable, and one can just share the object reference. However, you can use named parameters to modify some of the properties:

```scala
val price = amt.copy(value = 19.95) // Currency(19.95, "EUR")
val price = amt.copy(unit = "CHF") // Currency(29.95, "EUR")
```



#### 14.11 Infix Notation in `case` Clauses

#### 14.12 Matching Nested Structures

#### 14.13 Are Case Classes Evil

#### 14.14 Sealed Classes

#### 14.15 Simulating Enumerations

#### 14.16 The `Option` Type

#### 14.17 Partial Functions

#### 14.18 Exercise Tips

