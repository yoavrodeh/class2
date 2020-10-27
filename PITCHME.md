# OOP in JAVA

## A Little More



---
## Today
1. Reading input from the user with a `Scanner`.
1. Writing formatted output.
1. Nodes and Lists.
1. Basic Method overloading.
1. `static` methods and variables.
1. Packages.


---
## Resources
+ [Scanning and formatting](https://docs.oracle.com/javase/tutorial/essential/io/scanfor.html)
+ [Static variables and methods](https://docs.oracle.com/javase/tutorial/java/javaOO/classvars.html)
+ [Packages](https://docs.oracle.com/javase/tutorial/java/package/index.html)

---
### Standard I/O
+ Normally:
  + `System.in`, reads from keyboard.
  + `System.out` and `System.err`  write to screen.
+ Someone running a java program can **redirect** these, and so separate `err` from `out`.
+ Eclipse makes `System.err` red.
+ Mixed printing to `System.out` and `System.err` may be in the wrong order.


---
### Reading
+ Simplest way is to use a `Scanner` object.
+ A `Scanner` breaks its input into strings, ignoring spaces and newlines.
+ on each call to `next()`, it gives the next string.
  + It remembers where it is in the input.
+ It can check and convert the string to other types.



---
@code[java  code-noblend](src/ScannerExample.java)
@[1-11](`hasNext` will return true until all the original string is scanned. `next` will both return the current part of the string, and advance the inner "cursor" to the next one.)
@[13-19](`hasNextInt` will be true if the next string can be read as an int. `nextInt` will convert it to an int.)
@[21-25](This reads from the user.)



---
### `Scanner`
+ `hasNext()` says if there is anything left.
+ `next()` returns it.
+ Many versions for specific types: `Int`, `Double`, `Boolean`, etc.
+ `nextLine()` returns a full line.
+ Many complex features, such as changing what to ignore, looking for specific patterns, etc.




---
@code[java  code-noblend](src/ScannerSimple.java)
@[1-10](Standard use case.)
@[12-19](Why do we need `s.next()`?)



---
@box[rounded](Create only one `Scanner` object for `System.in`, because otherwise some of the input may be lost.)

The reason is a `Scanner` may read ahead of what you are currently reading and keep the extra information in an internal buffer. It uses it to answer later calls to `next()`.



---
### Formated output
pretty similar to `printf` of C/C++.




---
@code[java  code-noblend](src/Formater.java)
@[3-9](`System.out.format` replaces `System.out.print`.)
@[11-13](`String.format` returns a new string.)



--- 
### `format`
+ `%d` for integers (decimal), `%f` for floating point.
+ `%s` for string.
+ `%n` for newline.
+ number between `%` and the letter, is minimum length.
+ number after `.` is the precision.
+ many more features. See for example [here](https://dzone.com/articles/java-string-format-examples).
+ The class `String` has a static format method (more on that soon).




---
### Lists
+ A simple example of recursive data structures.
+ Nothing really new here technically, just good practice.



---
@code[java  code-noblend](src/MainForList.java)
This is what we want. How can we implement it?


---
@code[java  code-noblend](src/Node.java)
@[1-10](A `Node` holds a reference to a `Node`.)
@[12-19](A recursive algorithm for a recursive data structure.) 



---
@code[java  code-noblend](src/List.java)
@[1-12](A list just holds the first node.)
@[14-19](How would you make this recursive?)




---
### Method Overloading

We can have two methods with the same name, as long as they have different signatures
+ That is, their name and parameter list (considering only the **types**).
+ Java can tell by how a method is called, which version to call.

```java code-noblend
int f(int x, int y) {...}
double f(int x, String y) {...}

f(3, "abc");
```


---
### Static methods
+ Also called **class methods**, because they belong to the class and not to any specific object.
+ Therefore, cannot access the instance variables.
+ Exist even if no object of this class was created.
+ Methods of the `Math` class: `sin`, `cos`, `exp`, etc.
+ To use them, 
  + from within the class, just normally.
  + from outside, using the class name (`Math.cos`).
+ Why is `main` static?



---
@code[java  code-noblend](src/MyMath.java)
To use it from anywhere write `MyMath.pow(2,10)`.



---
@code[java  code-noblend](src/Point.java)
@[1-10](Using `String`'s static method `format`.)
@[12-20](The static method does not have an `x` and `y` of its own.)
@[22-24](The non-static method can use the static one.)

 
---
@code[java  code-noblend](src/MainForPoint.java)
The `dist` method is a good example for a static method. It naturally belongs to the class, though technically could have been placed anywhere else.



---
### Static Variables
+ Also called **class variables**. 
+ Like static methods, they belong to the class, and not to an object.
+ Can be thought of as *global* variables
  + So try to avoid them!
+ Static methods **can** access static variables.
  
  

---
@code[java  code-noblend](src/Static1.java)
Actually, `inc` and `getx` could have been declared `static`.
  
  
  
---
@code[java  code-noblend](src/Student.java)
@[1-12](`lastnum` increases every time a `Student` instance is created.)
@[14-20](We can just put our `main` anywhere we like.)


---
### Constants
+ Like `Math.Pi`.
+ Are normally declared:
  + `public static final int SIZE = 100`
+ `final` means they cannot be changed after instantiation.
+  Of course can be `private` as well.
+ `static` because we need only one copy for all objects.


---
@code[java  code-noblend](src/Singleton.java)

What does this do??


---
It is actually a known *design pattern* called 
### Singleton
@quote[Ensure a class has only one instance and provide a global point of access to it.](Design Patterns, Elements of Reusable Object-Oriented Software)

+ Useful when you need a window manager class, a file system class, etc.


---
Look back at the code, and answer these questions:
+ Why don't we just not write a constructor? @css[fragment](*Then we would have the default public constructor and could create instances freely.*)
+ What if `getInstance` was not static? @css[fragment](*No instance could be created, and so this method could not be called.*)
+ What if `instance` was not static? @css[fragment](*the method `getInstance` would not compile.*)


---
### Useful Java library static methods, 
These work for any primitive or String array `arr`:
+ `Arrays.sort(arr)`
+ `Arrays.shuffle(arr)`, randomly mixes `arr`.
+ `Arrays.binarySearch(arr, x)`. 
+ There are actually many overloaded versions of each of these methods.
+ `shuffle` works for any array, but `sort` can't unless we tell it how to compare elements in the array. We will learn this later.


---
+ `String.valueOf(x)` returns a string representation of `x`. 
  + Works for all primitive types (by method overloading). 
    Useful for getting a string from a number.
  + returns "null" if `x` is `null`, and `x.toString()` otherwise.
+ `Arrays.toString(arr)` shows the array using `String.valueOf` on all elements.
  + Returns for example: `[1, 34, 2, -3]`.
  + `arr.toString()` doesn't work so nicely.



---
### Question
Instead of having `List` and `Node`. We'll only have `Node` with a static field `head`. What is the problem with this idea?

@css[fragment](This way, there could be only one list, because there is only one `head`.)



---
### Packages
+ Are a way to separate classes into groups:
  + `java.util`, `javafx.scene`
+ A few ways to use the class `Scanner` of package `java.util`:
  + Use the full name `java.util.Scanner` as the class name.
  + `import java.util.Scanner` at the beginning of our file, and then just use `Scanner`.
  + `import java.util.*`, which imports all the classes in this package.
  
  
---
### More 
+ Practically, just use `Scanner` normally, and eclipse will offer to add the import.
+ to make your own packages, add a `package` statement at the head of the file 
  + even better, let eclipse do it for you.





---
```java code-noblend
package spider;

public class Man {
	public void jump() { }
}
```

```java code-noblend
package bat;

public class Man {
	public void fly() { }
}
```

```java code-noblend
package cat;
import spider.Man;

public class Woman {
	public void run() {
		bat.Man x = new bat.Man();
		Man y = new Man(); // Which man?
	}
}
```


---
### Lastly
+ The directory structure reflects the package structure
  + A `.` signifies a sub-directory.
+ However, these are not really sub-packages!
  + `import a.b.*` will not import the classes of package `a.b.c`. 
  
  
  
---
### Access modifiers
+ `private` - only within the class.
+ `public` - everyone.
+ default, called **package private** - everyone within the package.
+ `protected` - everyone within the package, and all subclasses (next class).



---
*If time permits...*

### Exercise

Implement the singleton pattern with a twist. Instead of storing one instance, store two instances. 
+ In every even call of `getInstance()`, return the first instance.
+ In every odd call of `getInstance()`, return the second.

Note:
Go back to the singleton slide and let them try.



---
@code[java  code-noblend](src/Doubleton.java)
@[1-5](Why is even static?)
@[7-18](Instances are not created unless needed (lazy initialization))
