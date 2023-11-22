## What is Generic Programming
**Definition:** (a style of) [computer programming](https://en.wikipedia.org/wiki/Computer_programming "Computer programming") in which [algorithms](https://en.wikipedia.org/wiki/Algorithm "Algorithm") are written in terms of [data types](https://en.wikipedia.org/wiki/Data_type "Data type") _to-be-specified-later_ that are then _instantiated_ when needed for specific types provided as [parameters](https://en.wikipedia.org/wiki/Parameter_(computer_programming) "Parameter (computer programming)"). This approach, pioneered by the [ML](https://en.wikipedia.org/wiki/ML_(programming_language) "ML (programming language)") programming language in 1973, permits writing common [functions](https://en.wikipedia.org/wiki/Function_(computer_science) "Function (computer science)") or [types](https://en.wikipedia.org/wiki/Type_(computer_science) "Type (computer science)") that differ only in the set of types on which they operate when used, thus reducing [duplicate code](https://en.wikipedia.org/wiki/Duplicate_code "Duplicate code").
- I assume algorithms are = functions
- What are data types & what is meant by to be specifiied later
	- E.g. int, char, bool
	- As in, the function will be given the data type to use
		- These are created when needed for that given type, provided from the parameters of the function
- Doing this allows for functions or types that do the same thing just on different types