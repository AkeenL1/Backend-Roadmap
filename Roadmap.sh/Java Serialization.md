### Serialization in Java
What is Serialization
* the process of translating a [data structure](https://en.wikipedia.org/wiki/Data_structure "Data structure") or [object](https://en.wikipedia.org/wiki/Object_(computer_science) "Object (computer science)") state into a format that can be stored (e.g. [files](https://en.wikipedia.org/wiki/Computer_file "Computer file") in [secondary storage devices](https://en.wikipedia.org/wiki/Secondary_storage_devices "Secondary storage devices"), [data buffers](https://en.wikipedia.org/wiki/Data_buffer "Data buffer") in primary storage devices) or transmitted (e.g. [data streams](https://en.wikipedia.org/wiki/Data_stream "Data stream") over [computer networks](https://en.wikipedia.org/wiki/Computer_networks "Computer networks")) and reconstructed later (possibly in a different computer environment).
	* The reconstruction of this data is known as **deserialization**
	* Some use-cases include
		* Serializing data for transfer across wires and networks ([messaging](https://en.wikipedia.org/wiki/Messaging "Messaging")).
		- storing data (in [databases](https://en.wikipedia.org/wiki/Database "Database"), on [hard disk drives](https://en.wikipedia.org/wiki/Hard_disk_drive "Hard disk drive")).
		- [remote procedure calls](https://en.wikipedia.org/wiki/Remote_procedure_call "Remote procedure call"), e.g., as in [SOAP](https://en.wikipedia.org/wiki/SOAP "SOAP").
		- distributing objects, especially in [component-based software engineering](https://en.wikipedia.org/wiki/Component-based_software_engineering "Component-based software engineering") such as [COM](https://en.wikipedia.org/wiki/Component_Object_Model "Component Object Model"), [CORBA](https://en.wikipedia.org/wiki/CORBA "CORBA"), etc.
		- detecting changes in time-varying data.
- Effectively, it's converting data into a byte stream through some format, then reconstructing it at a later time

### Doing this in Java
In Java this "data" we convert and de-convert is an Object
The byte stream created is platform independent. So, the object serialized on one platform can be deserialized on a different platform.
### How to do so
There's 2 classes in Java for serialization. One for output to the chosen source ( serialization ), and one for input from the chosen source ( deserialization )
1. ObjectOutputStream
```      
FileOutputStream fos = new FileOutputStream("t.tmp");
ObjectOutputStream oos = new ObjectOutputStream(fos);

oos.writeInt(12345);
oos.writeObject("Today");
oos.writeObject(new Date());

oos.close();
```
* This code creates a new ObjectOutputStream `oos` that outputs to a chosen `OutputStream` class ( in this case FileOutputStream)
* For an object to be serializable it must implement the Serializable interfrace
	* This interface has no data member and method, rather it simply is a marker that allows the class to be serizalied
	* E.g. "Today" is part of the class String which is Serializable

2. ObjectInputStream
	1. This is the same as output stream except it implements functions that allow for reading from an inputstream ( e.g. FileInputStream)
E.X
```// Java code for serialization and deserialization 
// of a Java object
import java.io.*;

class Demo implements java.io.Serializable
{
	public int a;
	public String b;

	// Default constructor
	public Demo(int a, String b)
	{
		this.a = a;
		this.b = b;
	}

}

class Test
{
	public static void main(String[] args)
	{ 
		Demo object = new Demo(1, "geeksforgeeks");
		String filename = "file.ser";
		
		// Serialization 
		try
		{ 
			//Saving of object in a file
			FileOutputStream file = new FileOutputStream(filename);
			ObjectOutputStream out = new ObjectOutputStream(file);
			
			// Method for serialization of object
			out.writeObject(object);
			
			out.close();
			file.close();
			
			System.out.println("Object has been serialized");

		}
		
		catch(IOException ex)
		{
			System.out.println("IOException is caught");
		}


		Demo object1 = null;

		// Deserialization
		try
		{ 
			// Reading the object from a file
			FileInputStream file = new FileInputStream(filename);
			ObjectInputStream in = new ObjectInputStream(file);
			
			// Method for deserialization of object
			object1 = (Demo)in.readObject();
			
			in.close();
			file.close();
		}
	}
}

```
In this example we make a class called demo, and mark it as serializable, we then create a new instance and save that instance as a byte stream to a file, we then read the file from an inpute stream created from the file, get the object instance back and are able to see what the object held

**NOTE**
1. Each serializeable class has its own SerialVersionUID which is used during deserialization to make sure the classes of the sender and receiver are the compatible objects, if they aren't we get an InvalidClassException
	1. The SerialVersionUID can be a default calculated at runtime, however its recommended to have an explicitly declared SerialVersionUID, since the default computation can differ depending on hardware
		1. There is a tool `serialVer` to see the serial version of classes in Java
	2. To explicitly decalre a SerialVersionUID you must use the name as a static final long
		1. `static final long serialVersionUID`
2. If we store sensitive information and send it through a network for example, it will also be serialized and sent out, to avoid this Java has the **transient** key word
	1. Transient ( among other things ) mean that the data will not be serialized, we we deserialize the object, it will result in the default value for that data being used.
	2. If we use final in conjuction with transient final takes precedence, this is because the final keyword is serialized directly by its value, so if its value is a primitive that primitive gets passed.

Summary: Serialization is the process of converting data into a bytestream to be written to or sent through something and later re-constructed at a possible different time or place. In Java we can accomplish serialization by marking object as serializable through extending the Serializeable interface. From there we can declare an output stream and use the ObjectOutputStream class to write to that stream. We can also declare an input stream and Use the ObjectInputStream class to read from that stream. For this to work properly Java checks for a SerialVersionUID that declares compatibility between the serialized object and the object we are attempting to deserialize it to. Serialization allows for efficient transport of data, however there is some data we don't want to serialize, in this case we use the **transient** key word to prevent serialization, when accessed after deserialization transient data is set to its default.