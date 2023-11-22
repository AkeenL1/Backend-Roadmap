## What are generics

**Definition:**  a facility of [generic programming](https://en.wikipedia.org/wiki/Generic_programming "Generic programming")
	1. A facility here meaning a concept of
	2. What is generic programming: [[Generic Programming]]
		1. Generic is programming, is a style of programming in which you write code in such a way that it can operate on many different data types without changing the core functionality of the code. Doing so allows for less code duplication.

So generics in Java are the programming languages implementation of the concept of [[Generic Programming]]

### How to use Generics in Java
**NOTE:** an entity ( class, interface, etc. ) that operates on a parameterized type ( a data type given through the parameters ) is called a *generic entity* 

#### There are two types of generic entities
**NOTE:** Type parameters in generics have a naming convention
- T – Type
	- This is the general "anything goes in here we don't know what the type will be"
- E – Element
	- The type of element a collection is meant to hold (E.g ArrayList< E >) (see [[Collections Framework]] for more detail)
- K – Key
	- This used in pair with V in key-value pair contexts as the key parameter
- V – Value
	- This is used in pair with K in key-value pair contexts as the value parameter
- N – Number
	- Indicated the parameter is specifically for numeric values, meaning its for parameters that are subtypes of java.lang.Number class
		- Basically all the number classes that aren't primitives. So
			- Integer
			- Long
			- Short
			- Float
			- Double
			- Etc.
##### Generic Method
Generic Java method takes a parameter and returns some value after performing a task. It is exactly like a normal function, however, a generic method has type parameters that are cited by actual type.
The compiler takes care of the type of safety which enables programmers to code easily since they do not have to perform long, individual type castings.

```
// Java program to show working of user defined
// Generic functions

class Test {
	// A Generic method example
	static <T> void genericDisplay(T element)
	{
		System.out.println(element.getClass().getName()
						+ " = " + element);
	}

	// Driver method
	public static void main(String[] args)
	{
		// Calling generic method with Integer argument
		genericDisplay(11);

		// Calling generic method with String argument
		genericDisplay("GeeksForGeeks");

		// Calling generic method with double argument
		genericDisplay(1.0);
	}
}

```
#### Generic Class
```
// To create an instance of generic class 
BaseType <Type> obj = new BaseType <Type>()
```
```
// Java program to show working of user defined
// Generic classes

// We use < > to specify Parameter type
class Test<T> {
	// An object of type T is declared
	T obj;
	Test(T obj) { 
		this.obj = obj; 
	} // constructor
	
	public T getObject() { 
		return this.obj; 
	}
}

// Driver class to test above
class Main {
	public static void main(String[] args)
	{
		// instance of Integer type
		Test<Integer> iObj = new Test<Integer>(15);
		System.out.println(iObj.getObject());

		// instance of String type
		Test<String> sObj
			= new Test<String>("GeeksForGeeks");
		System.out.println(sObj.getObject());
	}
}

```
**NOTE:** We wrap T in brackets <,> on instantiation of a generic method or class when instantiated to specify the type of the instantiation

#### Why do this?
There are 2 main reasons
1. The aforementioned code reusability 
	1. Instead of having an algorithm that takes in a specific type, we can use generics to 
2. Type Safety
	1. Generics make errors to appear compile time than at run time
		1. E.g if we have an ArrayList we can add a string or an integer to the list, but when we try to access the data we could have a runtime error
		2. By using generics we gain the ability to specify the type of the parameter allowing us to 
			1. See errors at compile time ( e.g. adding an int to a String ArrayList will error at compile time )
			2. We don't need to typecast our code, since the Generic is setup to return type T

**NOTE:** We can also bind the Type to specific entities
1. **Upper Bounded Wildcards**: These are specified using the `extends` keyword. This means that the type parameter must be a subclass of the specified class or an implementation of the specified interface. For example, `<T extends Comparable<T>>` means that `T` must implement the `Comparable` interface.
2. **Lower Bounded Wildcards**: Specified using the `super` keyword, these indicate that the type parameter must be a superclass of the specified class. This is less common than upper bounds. For example, `<T super Integer>` means that `T` must be a superclass of `Integer`.
3. **Purpose**: The main purpose of bounded type parameters is to provide tighter type checks at compile time, ensuring that the code is type-safe and that it operates only on a specific range of object types.
4. **Example**: Consider a generic class `Box<T>`. Without bounds, `T` can be any non-primitive type. However, if we declare it as `Box<T extends Number>`, then `T` can only be a class that is a subclass of `Number` (like `Integer`, `Double`, etc.).
### Summary
Generics in Java, specifically 'generic entities' are Java's implementation of generic programming. In generic programming code is effectively type agnostic, and coded in such a way that the program will perform the same algorithm an many different data types. Java accomplishes this by allowing the developer to specific if a class or method is a generic entity. This is done by using angled brackets and the type specific T ( although other specifiers are used conventially depening on the situation ) <> on declaration to create a generic class or method, that can receive any Non-primitive data type. 
	E.g 
		public static <T> void func(T element)
		public class Class<T>
	Here a method func is declared to take in some object T, and a class Class is declared to have a parameter of type T that can be used.
These are important as they improve code reusability by allowing for the same code to operate on many different types.
Generics also allow us to impose type safety on generic classes on declaration, we can declare a generic method or class on instantiation to only accept types we give it.
	Furthermore we can bind T itself to other entities requiring that whatever is passed into it either extends or is extended from the entity, increasing type safety
		e.g. N extends Number
			or T extends Comparable<T>