# Constructors

## Learning Goals

- Explain constructors
- Create constructors in Java

## Introduction

A **constructor** is a specific kind of method that is called when an object is
instantiated.  Recall that we create an instance of `Dog` with the following code snippet:

```java
new Dog()
```

The code consists of 2 parts:

- Instantiation − The `new` operator is used to create an instance of `Dog` by allocating memory
  to store the instance variables.

- Initialization − The `new` operator is followed by a call to a constructor `Dog()`.  The
  constructor is a method that initializes the new object by assigning a value to each instance variable.

General rules about constructor methods:

- A constructor has the same name as the class.
- A constructor can have parameters.
- A constructor does not have a return type, not even void.
- A constructor implicitly returns a reference to the newly created object.  Do not use an explicit return statement.
- A class may have multiple constructors, called **constructor overloading**, as long as the number or type of parameters differ.

## Default Constructor 

![noargs signature](https://curriculum-content.s3.amazonaws.com/6676/java-methods/noargs_signature.png)

A constructor with no arguments such as `Dog()` is called the *default* constructor or *no-args* constructor.

- If you *do not* define a constructor, Java implicitly defines a default no-args constructor.
- If you *do* define a constructor with or without parameters, Java *does not* generate a default constructor.

The default constructor for `Dog` initializes instance variables to default values
based on their data type:

```java
public class Dog {

    // Instance Variables
    private String name;
    private String breed;
    private int age;
    private boolean waggingTail;

    // Constructor
    public Dog() {
        this.name = null;
        this.breed = null;
        this.age = 0;
        this.waggingTail = false;
    }

    // Other methods
    ...
}
```

1. The constructor method name matches the class name `Dog`.
2. The signature does not specify a return type.
    - The return type is implied by the class, i.e. `Dog`
    - There is no return statement.  A new  `Dog` object is automatically returned.
3. Instance variables are initialized to default values based on declared type.


Let's create two instances of `Dog` in the `main` method:

```java
public class Dog {

    private String name;
    private String breed;
    private int age;
    private boolean waggingTail;

    public static void main(String[] args) {
        Dog bigDog = new Dog();
        Dog smallDog = new Dog();
    }

}
```

The expression `new Dog()` calls the default constructor.
Both `Dog` instances are initialized to the same default state:


![dog#2 created](https://curriculum-content.s3.amazonaws.com/6676/java-mod2-oop-fundamentals/new_dog2.png)


## Constructor Parameters

We've seen how to use dot notation to assign new values to the fields after creating
the object, for example:

```java
// Step #1: create the new Dog object, assigning default values to the instance variables
Dog bigDog = new Dog();

// Step #2: assign new values to the instance variables
bigDog.name = "Fido";
bigDog.breed = "Great Dane";
bigDog.age = 8;
bigDog.waggingTail = true;
```

We can implement a constructor to combine the two steps into one reusable method.
The constructor will take parameters used for initializing the instance variables.

![args signature](https://curriculum-content.s3.amazonaws.com/6676/java-methods/args_signature.png)

```java
// Constructor
public Dog(String name, String breed, int age, boolean waggingTail) {
    this.name = name;
    this.breed = breed;
    this.age = age;
    this.waggingTail = waggingTail;
}
```

1. The constructor method name matches the class name `Dog`.
2. The signature does not specify a return type.
   - The return type is implied by the class, i.e. `Dog`
   - There is no return statement.  A new  `Dog` object is automatically returned.
3. The parameters have the same names as the instance variables.
   - The parameters shadow the instance variables, thus each
     instance variable must be qualified using the implicit parameter `this`.


We'll update the `main` method to call this new constructor:

```java
public static void main(String[] args) {
    Dog bigDog = new Dog("Fido", "Great Dane", 8, true);
    Dog smallDog = new Dog("Sweetie", "Miniature Poodle", 2, false);
}
```

![constructor 2 dogs](https://curriculum-content.s3.amazonaws.com/6676/java-methods/constructor_2dogs.png)

Try setting a breakpoint in the `main` method at the following line that creates the first dog.  

```java
Dog bigDog = new Dog("Fido", "Great Dane", 8, true);
```

Press step-into to transfer control into the constructor method.
You'll notice IntelliJ adds a frame to
the call stack named `<init>`.  The frame represents the constructor method
and shows the constructor parameters. The implicit parameter `this`
is automatically passed to the constructor using the memory address that
was allocated by the `new` operator.

![constructor init](https://curriculum-content.s3.amazonaws.com/6676/java-methods/constructor_on_stack.png)

## Parameter Names

As mentioned, constructor parameters usually use the same variable names as the
properties in the class they are meant to initialize, which is why we need to
use the `this` keyword to clearly indicate which variable we are referring to in
the constructor. It is possible, however, to use different names for the
parameters into the constructor, in which case the `this` keyword does not need
to be used because there is no ambiguity in the variable names.

Here is a version of the constructor that does that, just for reference. 

```java
// Constructor
public Dog(String dogName, String dogBreed, int dogAge, boolean dogWaggingTail) {
    name = dogName;
    breed = dogBreed;
    age = dogAge;
    waggingTail = dogWaggingTail;
}
```

Note that we will continue to use the generally accepted naming convention (i.e. parameter
names should match instance variable names) in all subsequent examples.

```java
// Constructor
public Dog(String name, String breed, int age, boolean waggingTail) {
    this.name = name;
    this.breed = breed;
    this.age = age;
    this.waggingTail = waggingTail;
}
```

## Multiple Constructors

We can define multiple constructors as long as the number or types of parameters differs.
Typically, this is done to provide default values for one or more instance variables.
For example, we might add a second constructor that takes only the name and age as parameters.
The constructor will assign the breed to `unknown` as a default value. As the
constructor does not assign `waggingTail` to a value, it is initialized
to the boolean default value of `false`.

```java

// Constructor #1
public Dog(String name, String breed, int age, boolean waggingTail) {
    this.name = name;
    this.breed = breed;
    this.age = age;
    this.waggingTail = waggingTail;
}

// Constructor #2
public Dog(String name, int age) {
    this.name = name;
    this.age = age;
    this.breed = "unknown";
}
```

We'll update the `main` to create a third dog using the new constructor:

```java
public static void main(String[] args) {
    Dog bigDog = new Dog("Fido", "Great Dane", 8, true);             // Constructor #1
    Dog smallDog = new Dog("Sweetie", "Miniature Poodle", 2, false); // Constructor #1
    Dog mediumDog = new Dog("Honey", 10);                            // Constructor #2
}
```

![3 dogs](https://curriculum-content.s3.amazonaws.com/6676/java-methods/constructor_3dogs.png)

There are 2 important things to note when defining constructors:

1. Not all fields can have default values - for example, what could a "default"
   name be for a dog? 
2. Some fields do make sense to have default value - for example, we often don't know
   the breed, so the second constructor defaults the `breed` field to "unknown".



## Comprehension Check

<details>
    <summary>What is printed when the <code>main</code> method executes?</summary>

  <p>Whiskers,true,squeaky mouse</p>
  <p>Constructor #1 is executed since only two arguments are passed into the method.</p>

</details>

<br>

```java
public class Cat {
    private String name;
    private boolean purring;
    private String favoriteToy;

    // Constructor #1
    public Cat(String name, boolean purring) {
        this.name = name;
        this.purring = purring;
        this.favoriteToy = "squeaky mouse";
    }

    // Constructor #2
    public Cat(String name, boolean purring, String favoriteToy) {
        this.name = name;
        this.purring = purring;
        this.favoriteToy = favoriteToy;
    }

    public static void main(String[] args) {
        Cat kitty = new Cat("Whiskers", true);
        System.out.println(kitty.name + "," + kitty.purring + "," + kitty.favoriteToy);
    }
}
```

## Constructor Chaining

We have some code duplication between the two constructors, specifically the
two statements that assign `name` and `age`.  We can do something
called *constructor chaining*, where one constructor calls another to initialize
the variables.

We can have the constructor that takes two parameters (Constructor #2) call the
constructor that takes four parameters (Constructor #1).  This must be done as
the *first line of code* in the constructor body and the method name `this`
is used to call the constructor, i.e.  `this(name, "unknown", age, false);`.

```java
// Constructor #1
public Dog(String name, String breed, int age, boolean waggingTail) {
    this.name = name;
    this.breed = breed;
    this.age = age;
    this.waggingTail = waggingTail;
}

// Constructor #2
public Dog(String name, int age) {
    this(name, "unknown", age, false);  // Call constructor #1
}
```

## Default Constructor Revisited

If we define a constructor that has one or more parameters,
the compiler does not generate a default no-args constructor.
Thus, we will get a compile-time error if we try to call the no-args constructor `new Dog()`:

```java
public static void main(String[] args) {
    Dog bigDog = new Dog("Fido", "Great Dane", 8, true);
    Dog smallDog = new Dog("Sweetie", "Miniature Poodle", 2, false);
    Dog mediumDog = new Dog("Honey", 10);
    Dog xlargeDog = new Dog();  //ERROR: no default constructor
}
```

We would need to explicitly add a no-args constructor  to the `Dog` class
to avoid the error.


## Code Check

Our final version of the `Dog` class is:

```java
public class Dog {

    private String name;
    private String breed;
    private int age;
    private boolean waggingTail;

    // Constructor #1
    public Dog(String name, String breed, int age, boolean waggingTail) {
        this(name, age); //calls Constructor #2
        this.age = age;
        this.waggingTail = waggingTail;
    }

    // Constructor #2
    public Dog(String name, int age) {
        this.name = name;
        this.age = age;
        this.breed = "unknown";
    }

    public static void main(String[] args) {
        Dog bigDog = new Dog("Fido", "Great Dane", 8, true);             //Constructor #2
        Dog smallDog = new Dog("Sweetie", "Miniature Poodle", 2, false); //Constructor #2
        Dog mediumDog = new Dog("Honey", 10);                            //Constructor #1
        //Dog xlargeDog = new Dog();                      //ERROR: no default constructor
    }

}
```


## Conclusion

- A constructor has the same name as the class.
- A constructor can have parameters.
- A constructor does not have a return type, not even void.
- A constructor implicitly returns a reference to the newly created object.  Do not use an explicit return statement.
- A class may have multiple constructors, called **constructor overloading**, as long as the number or type of parameters differ.
