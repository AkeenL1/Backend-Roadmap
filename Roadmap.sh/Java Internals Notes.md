## Question: How does Java work under the hood?

### So the main question is: What is the JVM?

[Java virtual machine](https://en.wikipedia.org/wiki/Java_virtual_machine)

1. Its not _the_ JVM its **a** JVM
    1. Wikipedia defines a JVM as: a [virtual machine](https://en.wikipedia.org/wiki/Virtual_machine) that enables a computer to run [Java](https://en.wikipedia.org/wiki/Java_(software_platform)) programs as well as programs written in [other languages](https://en.wikipedia.org/wiki/List_of_JVM_languages) that are also compiled to [Java bytecode](https://en.wikipedia.org/wiki/Java_bytecode).
2. What other languages
    1. Other implementations of the language syntax compiling into Java Bytecode, running on the JVM
3. what does ‘running mean’
    1. virtual machine
        1. We think of virtual machine as virtualbox but its really something different?
            1. But this is in fact a “System Virtual Machine” they provide ability to run the whole system
            2. The virtual machine we care about is a Process Virtual Machine or _application virtual machine_, or _Managed Runtime Environment_ (MRE)
                1. Defined as: (an operating system that? ) runs as a normal application inside a host OS and supports a single process. It is created when that process is started and destroyed when it exits. Its purpose is to provide a [platform](https://en.wikipedia.org/wiki/System_platform)-independent programming environment that abstracts away details of the underlying hardware or operating system and allows a program to execute in the same way on any platform.[_[citation needed](https://en.wikipedia.org/wiki/Wikipedia:Citation_needed)_]
                2. Application
                    1. is created when that process is started and destroyed when it exists
                    2. refers to a software program that performs specific tasks for the user, running within an operating system (OS).
                3. Process
                    1. "process" is an instance of the application that is being executed by the system.
    2. So a JVM is a process virtual machine that supports a single process
        1. What process does it support?
            1. Running your Java Code ( and running all the other things needed to run the java code )
        2. We can find the details of this in the specification
            1. what is a specification
                
                1. Defined as: a set of documented requirements to be satisfied by a material, design, product, or service
                2. Basically the tech specs/ documentation
                    1. Decsrcibes both what things do and how the work and the necessary things to make it work
                
                [Specification (technical standard)](https://en.wikipedia.org/wiki/Specification_(technical_standard))
                
4. So we know the JVM is a process virtual machine that allows for running java programs.
    1. How?
        1. What happens after you write some java code
            1. The java compiler ( javac ) converts the human readable code into java bytecode
                Java Compiler - (JavaC)
                1. What is a compiler?
                    1. Basically a program that translates code in one language to another
                        1. Most commonly source code of a high level language to low level code so we can execute a program
                        2. Three general levels
                            1. the _front end_ scans the input and verifies syntax and semantics according to a specific source language, transforms the input program into an [intermediate representation](https://en.wikipedia.org/wiki/Intermediate_representation) (IR) for further processing by the middle end. This IR is usually a lower-level representation of the program with respect to the source code.
                            2. _middle end_ performs optimizations on the IR that are independent of the CPU architecture being targeted. This source code/machine code independence is intended to enable generic optimizations to be shared between versions of the compiler supporting different languages and target processor
                            3. The back end generates the target-dependent assembly code
                2. There are other compilers not just for java, but the most common java compiler is java c
            2. This is Java Bytecode, which the other languages get compiled to, this bytecode is specifically for the jvm
                
                1. Java Bytecode is an intermediate representation
                2. JVM interprets this bytecode or compiles it on-the-fly into native machine code using a Just-In-Time (JIT) compile
                3. Its an ‘instruction set’ for the jvm
                    1. Instruction Set - instructions for the jvm to execute
                4. Basically Java Bytecode is in the form of 1’s and 0’s in a .class file
                    1. B/c of this you can use a disassembler to convert bytecode back to source code
                5. Each bytecode operation in the JVM represented by a byte ( 8 bits ), thus bytecode
            3. This bytecode is saved in .class files which get read by the jvm
                
            4. The JVM either
                
                1. Interprets this bytecode
                2. Compiles it on the fly
            5. JVM is both a [stack machine](https://en.wikipedia.org/wiki/Stack_machine) and a [register machine](https://en.wikipedia.org/wiki/Register_machine), of which Java Bytecode operates as a set of instructions, utilizing an operand stack and local variables for executing operation, but its primarily a stack machine
                
                1. Stack Machine: a cpu or vm thats primary interaction is moving short-lived temporary values to and from a push down [stack](https://en.wikipedia.org/wiki/Stack_(abstract_data_type))
                    1. Pushes to stack and pops off stack for its core functionality
                2. Register machine: model uses **multiple, uniquely addressed registers**, each of which holds a single positive [integer](https://en.wikipedia.org/wiki/Integer).
            6. In the JVM each method call has some frames, describing the subroutines of the method that gets put on the top of the call stack
                
                1. Call stack maintains the stack of frames for all active method calls. with the current running method on top of the stack
                2. E.g if you had a drawsquare function that called draw line youd push the necessary operations for drawsquare all inside a single frame, then push the same for draw line and draw line would be at the top of the stack
                3. Each of these frames has an operand stack and an array of local variables
                    1. The operand stack is for operands to computations and for receiving the return value of a called method, each frame has its own operand stack, the JVM manages transfer of values between frame operand stacks
                    2. The local variables array serves the same purpose as a register, holding a single positive integer, are also used to pass method arguments
                    3. So in action if we have function add(a,b)
                        1. Once the JVN loads this class file
                            1. A new frame is created for the add function, this frame gets pushed onto the call stack
                            2. Inside this frame theres an operand stack and local variable array
                            3. The JVM executes the bytecode instructions for add, storing the result
                                1. Here the value is popped off the operand stack
                                2. the current frame passes back the result of its method invocation
                            4. the ‘add’ frame is popped off the call stack
                4. Each bytecode is made up of a byte that represents the opcode along w/ 0 or more bytes for the operands
                    1. opcode - the code that specifies what operation to be performed
                        
                    2. There 256 different opcodes
                        
                        [List of Java bytecode instructions](https://en.wikipedia.org/wiki/List_of_Java_bytecode_instructions)
                        
        2. What is a .class file?
            
            1. What is a .class file
                1. Once the Java code you write gets compiled to bytecode the compiler saves that bytecode to .class files
                2. The class files as one might expect represent a class or interface
            2. So a .class file is effectively a java bytecode representation of a class
            3. Its in binary form, e.g. not ascii therefore text programs cant convert it to human readable, but you can edit it
            4. There are 10 basic sections to the Java class file structure:
                - **[Magic Number](https://en.wikipedia.org/wiki/Magic_number_(programming))**: 0xCAFEBABE
                - **Version of Class File Format**: the minor and major versions of the class file
                - **Constant Pool**: Pool of constants for the class
                - **Access Flags**: for example whether the class is abstract, static, etc.
                - **This Class**: The name of the current class
                - **[Super Class](https://en.wikipedia.org/wiki/Superclass_(computer_science))**: The name of the super class
                - **[Interfaces](https://en.wikipedia.org/wiki/Interface_(object-oriented_programming))**: Any interfaces in the class
                - **Fields**: Any fields in the class
                - **[Methods](https://en.wikipedia.org/wiki/Method_(computing))**: Any methods in the class
                - **Attributes**: Any attributes of the class (for example the name of the sourcefile, etc.)
        3. JVM Recieves these class files from a classloader that does 3 different things
            
            1. Loading: finds and imports the binary data for a type ( for a class )
            2. Linking: performs verification, preparation, and (optionally) resolution
                - Verification: ensures the correctness of the imported type
                - Preparation: allocates memory for class variables and initializing the memory to default values
                - Resolution: transforms symbolic references from the type into direct references.
            
            3.Initialization: invokes Java code that initializes class variables to their proper starting values
            
            1. These classes are dynamically loaded, e.g. loaded on demand
                
            2. 3 class loaders
                
                1. **Bootstrap Class Loader**:
                    - **Primary Function**: It loads the core Java API classes. These are the fundamental classes from the Java Runtime Environment (JRE) that are necessary for running any Java program.
                    - **Example of What It Loads**: Classes from the `java.lang` package, such as `String`, `System`, and other classes available in the `rt.jar` file (runtime library) of the Java installation directory.
                    - **bootstrap class loader is part of the core JVM and is written in native code,**
                        - Effectrively it loads the other class loaders, its witten in native code
                2. **Extensions Class Loader**:
                    - **Primary Function**: This loader is responsible for loading extension classes. These are classes that are not part of the core Java API but are provided as extensions to the standard core Java libraries.
                    - **Example of What It Loads**: Classes that are located in the extensions directory of the JRE (`jre/lib/ext` directory, or specified in the `java.ext.dirs` system property). These could include additional APIs provided by Java or third-party libraries designed as standard extensions.
                3. **System Class Loader**:
                    - **Primary Function**: Also known as the application class loader, it loads classes that are found on the Java application's classpath. This includes classes and JAR files that are not part of the core Java platform but are part of your application or third-party libraries your application uses.
                    - **Example of What It Loads**: Your application’s classes and JARs specified in the classpath environment variable, or the `classpath` (or `cp`) command-line option when running the Java application.
            3. Its also possible to write your own custom class loader.
                
            
            - it’s important to note here that you aren’t necessarily adding it to the JVM, really a custom class loader extends the other class loaders ( system or extensions ) and performs some operation. The JVM executes your custom class loader in the same way it executes any other class you make, in these cases we use the system class loader
    2. So now what happens once the class is loaded and execution begins
        
    3. the Main thread is instantiated, this is the thread that runs the source code we compiled and loaded
        
        1. If multithreading is used a new thread is created that runs independantly of the Main thread
        2. Other threads are ustilized mainly for the JVM to handle background processes, like memory management & garbage collection
    4. The main thread gets executed
        
        1. This thread has a call stack where each method is a frame pushed to the stack
            1. Each frame has an operand stack & a local variable array
            2. The operand stack is for operands to computations and for receiving the return value of a called method, each frame has its own operand stack, the JVM manages transfer of values between frame operand stacks
            3. The local variables array serves the same purpose as a register, holding a single positive integer, are also used to pass method arguments
            4. So in action if we have function add(a,b)
                1. Once the JVM loads this class file, in the Main thread
                    1. A new frame is created for the add function, this frame gets pushed onto the call stack
                    2. Inside this frame theres an operand stack and local variable array
                    3. The JVM executes the bytecode instructions for add, storing the result
                        1. Here the value is popped off the operand stack
                        2. the current frame passes back the result of its method invocation, if necessary (e.g. assigned to a variable )
                    4. the ‘add’ frame is popped off the call stack
            5. Does this until execution is finished either programtically or systemically ( power off )
        2. During this main thread execution there are some background processes we care about
        3. More Importantly JVM memory uses multiple different components illustrated here

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/348dcaec-ab81-4cf3-8e9c-8c45c2e6cb0e/b53ff1fa-8e50-427a-825e-0ad797d0bb68/Untitled.png)

1. [JVM memory is divided in 5 parts: Method Area, Heap, Stack, PC Register and Native Memory.]
2. What is heap memory ( aka dynamic memory )
    1. NOT a heap DS
    2. Unlike the stack, where memory is allocated and deallocated in a very structured order (last-in, first-out), memory in the heap can be allocated and freed in an arbitrary order. The analogy is that objects are piled up haphazardly as they are allocated and removed, much like a "heap" of objects would be in the physical world.
3. Execution is handeled in the JVM Execution Engine, but first we need to remember what we’re executing hence, JVM Memory
4. **Method Area**: This is a part of the JVM memory where class-level information is stored. It includes data such as the runtime constant pool, field and method data, the code for methods and constructors (including the special methods used in class and instance initialization), and more. It's not tied to any particular thread and is shared among all threads running in the JVM.
    1. Its seperate from the heap, however it does similar things to the heap specifically for class level infomration
5. **Heap**: As previously discussed, this is where all the objects are allocated. It is also shared among all threads.
    1. Note: There is a single heap for a JVM Process
    2. in this code Scanner sc = new Scanner([System.in](http://system.in/)); Scanner class is added to the heap, the sc ( which is a refernce to the class ) gets put onto the stack
6. **JVM Language Stacks (Java Stack)**: Each thread in the JVM has its own JVM stack, created when the thread is created. A JVM stack stores frames. A frame is created every time a method is invoked and is destroyed when its method invocation completes. It holds local variables, the operand stack, and a reference to the runtime constant pool of the class of the current method.
7. **PC Registers**: Each thread also has a PC (program counter) register. This contains the address of the JVM instruction currently being executed. If the method being executed is native, the PC register is undefined.
    1. The non native method has a PC which stores the address of the available JVM instruction whereas in a native method, the value of program counter is undefined. PC register is capable of storing the return address or a native pointer on some specific platform.
8. **Native Method Stacks**: These stacks are similar to JVM Language Stacks but are used for native methods—methods written in a language other than Java, like C or C++, which are called from Java code.
9. So the flow is
    1. The JVM initiliazes all the necessary stuff such as method area and heap
    2. The necessary threads are created and the execution engine begins running the program
        1. During this execution a Call stack( JVM Language stack ) and PC register is created along with the necessary frames for each method
            1. If the java code calls other native code ( such as a c++ library ) it also creates and utilizes a native method call stack in the same fashion
        2. Inside each frame an operand stack and local variable array is used to handle the operations
10. Execution Engine ( or how the JVM runs the loaded class )

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/348dcaec-ab81-4cf3-8e9c-8c45c2e6cb0e/20bd60f4-23f0-45cf-8264-66dfd2cdba3c/Untitled.png)

1. 3 main components
    
    1. **Interpreter:** It reads the byte code and executes them in a sequential manner. This component runs the application from the command line by accepting a filename argument. The problem with the interpreter is that it interprets every time, even the same method multiple times, which reduces the performance of the system. To overcome this problem JIT Compilers is introduced in 1.1 version.
        1. The interpreter in the JVM directly executes the bytecode instructions sequentially. It reads the bytecode, interprets it, and executes the corresponding operations. This process is relatively slower compared to JIT compilation because each bytecode instruction is interpreted every time it's executed.
        2. This runs the interpeted instructions directly on the cpu using native methods ( more in a moment)
        3. This can be slow because everytime instructions are run they need to follow this pattern of interpretation → native code → execution so use a jit
    2. JIT ( Just In Time compiler )
        1. The just in time compiler compiles commonly used code, from the hotspot profiler, into native machine code which the JVM directly calls instead of interpreting it
    3. Garbage Collector
        1. two types of garbage collection activity usually happen in Java. These are:
            
            1. **Minor or incremental Garbage Collection:** It is said to have occurred when unreachable objects in the young generation heap memory are removed.
            2. **Major or Full Garbage Collection:** It is said to have occurred when the objects that survived the minor garbage collection are copied into the old generation or permanent generation heap memory are removed. When compared to the young generation, garbage collection happens less frequently in the old generation.
        2. An object is unreachable if it doesnt contain any refernece to it, e.g. i = new Person, i= null, ← there’s no longer a reference to Person object
            
            There are generally four ways to make an object eligible for garbage collection.
            
            1. Nullifying the reference variable
            2. Re-assigning the reference variable
            3. An object created inside the method
            4. Island of Isolation
                1. Circular referencing e.g x = y, y = x
        3. We can manually instantiate a garbage collector using, .finalize or .runFinalization,
            
            1. .finalize is a class method that will finalize the class before garbage collection
                1. Its implementation in JVM is empty, but you can override it in a class for some custom logic
            2. .runFinalization will run finalize for every garbage collection eligible object
            3. See for an example
            
            [Garbage Collection in Java - GeeksforGeeks](https://www.geeksforgeeks.org/garbage-collection-java/)
            
            1. Here they Basically override the finalize command then on two classes make them eligible for garbage collection, then force the garbage collector to run them
        4. NOTE: the garbage collection is non deterministic and we dont know when itll run, meaning we probably shouldnt use these functions, just override should be enough
            
2. Why?
    
    1. Because abstracting this way allows for hardware, platform independance, amongst other things ( performance & security )
        1. Its why you can play Minecraft Java on any OS, but bedrock is not officially supported on mac or linux
3. Summary:
    1. The JVM is a process virtual machine that runs java programs. It does this by recieving java bytecode files ( .class files ) that are the java source code files compiled to java bytecode. When the programs are ran first a class loader loads the class into the JVM for the interpreter and the JVM also sets up the heap and method area along with necessary threads such as the main thread and garbage collection thread. From there the JVM interpreter begins reading and executing the .class file instruction, when methods are called a frame is put onto the JVM call stack and a PC register points to that reference in memory. The frame contains local variable and operand stack for the method running, as the method runs a hotspot profiler tries to locate commonly used areas of the method or the entire method itself to compile into native machine code, that is used later in place of the interpretation. As the program executres the garbage collector looks at the heap to find any eligible objects stored based on some criteria and tries to free them up, this freeing is non-deterministic.
