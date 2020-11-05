# C# 9

## Record Types

From Claudio Bernasconi: https://www.youtube.com/watch?v=5GQ57TUqNoQ

### Overview

- Record Types are immutable reference types
- Property values cannot be changed after initialization

### Features

- Immutable through __read-only properties__
- A __constructor__ with properties as arguments
- __Value-based__ equality checks
- A __textual representation__ (ToString)
- A __Deconstruct__ method

### Value based equality

- Every record get's an Equals() implementation
- Equals() performs a value-based equality check

### Internals

- It's a compiler feature. Check generated C# code in https://sharplab.io/
- The compiler generates a class based on a template

### Example

''' c#

using System;

public record Person(String firstName, String lastName); // check this in https://sharplab.io/

namespace System.Runtime.CompilerServices{
    public sealed class IsExternalInit{}
}

'''

Execute RecordType:

''' c#

using System;

public record Person(String firstName, String lastName);
public record Employee(int employeeId, String firstName, String lastName) :  Person(firstName, lastName); // inheritance

namespace System.Runtime.CompilerServices{
    public sealed class IsExternalInit{}

    class Programm {
        static void Main(){
            // var person1 = new Person{firstName="Zeal", lastName="Ardor"}; // error CS7036: There is no argument given that corresponds to the required formal parameter 'firstName' of 'Person.Person(string, string)'
            var persion1 = new Person("Zeal", "Ardor");
            var persion2 = new Person("Ozzy", "Osbourne");
            // persion1.firstName = "test"; // error CS8852: Init-only property or indexer 'Person.firstName' can only be assigned in an object initializer, or on 'this' or 'base' in an instance constructor or an 'init' accessor.
        }
    }
}

'''

Initialization both ways: need 2 constructors (avoid this if not needed: you might break immutability if you are not careful):

''' c#

using System;

public record Car {
    public string Brand { get; init; }
    public string Name { get; init; }
    
    public Car(string brand, string name) {
        Brand = brand;
        Name = name;
    }
    public Car() {
    }
};

namespace System.Runtime.CompilerServices{
    public sealed class IsExternalInit{}

    class Programm {
        static void Main(){
            var car1 = new Car("Opel", "Corsa-E");
            var car2 = new Car { Brand = "Opel", Name = "Corsa-E" };
        }
    }
}

'''
