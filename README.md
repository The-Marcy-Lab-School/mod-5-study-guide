# Mod-5-study-guide

OOP-study guide for object-oriented programming.

## The `this` keyword

When we invoke a method on an object, `this` refers to the object itself

This keeps our code DRY. It lets multiple objects use the same method.

Example:

```js
const speak = function() {
  console.log(`My name is ${this.name} and I have ${this.eyeColor} eyes and ${this.hairColor} hair`);
}

const person1 = {
    name: "Itzel",
    eyeColor: "green",
    hairColor: "purple",
    speak: speak
}

const person2 = {
    name: "Gonzalo",
    eyeColor: "blue",
    hairColor: "red",
    speak: speak
}

person1.speak(); // My name is Itzel and I have green eyes and purple hair
person2.speak(); // My name is Gonzalo and I have blue eyes and red hair
```

We also use `this` when making a `class` inside the constructor function.

When we use the `new` keyword in a class constructor, `this` becomes the object returned by the constructor

```js
class Person {
  constructor(name, eyeColor, hairColor) {
    this.name = name;
    this.eyeColor = eyeColor;
    this.hairColor = hairColor;
  }
}
const person1 = new Person("ben", "brown", "black");

// this becomes person1
```

## Closures

What is it?

Here are some of the main points:

* Two functions: inner and outer
* Outer function doesn't have access to inner
* Inner function has access to outer

Why is it useful?

* Allows us to have private data
* Functions can reference data outside of it's scope

Example

```js
let age = 15;

const outer = () => {
  let canCode = true;
  
  const inner = () => {
    console.log(canCode);
  }
  
  return inner;
}

const innerFunc = outer();
innerFunc(); //true


const makeAdder = (numToAdd) => {
  // debugger;
  const adder = (num) => {
    // debugger;
    return num + numToAdd;
  }
  return adder;
}

const add10 = makeAdder(10);
const add20 = makeAdder(20);

console.log(add10(5)); // 15
console.log(add10(10)); 
console.log(add10(100)); 
console.log(add20(5)); // 25
```

## Factory Functions

What is it?

A factory function is a regular function that returns an object. It can encapsulate object creation and initialization logic, making it easy to create new objects with similar properties and methods.

Why is it useful?

Factory functions can encapsulate the object creation logic, keeping the code organized and easier to manage. This helps to make a consistent and predictable interface

They can also return different types of objects based on conditions, making them more flexible compared to constructors.

Simplified Inheritance: By using factory functions, you can use object composition over inheritance, which can lead to more flexible and maintainable code.

No `this` binding issues

Example:

```js

const createCar = (make, model, year) => ({
    make,
    model,
    year,
    getCarInfo: function() {
      return `${year} ${make} ${model}`
    },
    startEngine: function() {
        console.log(`${this.getCarInfo()} engine started.`);
    }
});

const car1 = createCar('Toyota', 'Corolla', 2021);
const car2 = createCar('Honda', 'Civic', 2020);

car1.startEngine(); // 2021 Toyota Corolla engine started.
car2.startEngine(); // 2020 Honda Civic engine started.

```

## Constructor Functions

What is it

When using prototype-based methods, we generally move away from factory functions to constructor functions or classes. However, we can still simulate a factory function style by using a helper function to manage prototype methods.

Why is it useful

Using prototype-based methods with constructor functions provides significant advantages in terms of memory efficiency, performance, consistency, and scalability. By simulating a factory function style with a helper function, you can combine the ease of use of factory functions with the performance benefits of prototypes.

```js
// Using the constructor function.

const Car = function(make, model, year) {
    this.make = make;
    this.model = model;
    this.year = year;
}

Car.prototype.getCarInfo = function() {
    return `${this.year} ${this.make} ${this.model}`;
};

Car.prototype.startEngine = function() {
    console.log(`${this.getCarInfo()} engine started.`);
};

const car1 = new Car('Toyota', 'Corolla', 2021);
const car2 = new Car('Honda', 'Civic', 2020);

car1.startEngine(); // 2021 Toyota Corolla engine started.
car2.startEngine(); // 2020 Honda Civic engine started.
```

## ES6 Classes

What is it?

Classes in ES6+ provide a more concise and clear syntax for creating constructor functions and managing prototypes.

Why is it useful?

Better readability, easier to write

Example

```js
class Car {
    constructor(make, model, year) {
        this.make = make;
        this.model = model;
        this.year = year;
    }

    getCarInfo() {
        return `${this.year} ${this.make} ${this.model}`;
    }

    startEngine() {
        console.log(`${this.getCarInfo()} engine started.`);
    }
}

const car1 = new Car('Toyota', 'Corolla', 2021);
const car2 = new Car('Honda', 'Civic', 2020);

car1.startEngine(); // 2021 Toyota Corolla engine started.
car2.startEngine(); // 2020 Honda Civic engine started.
```

## Inheritance

Inheritance defines a relationship between two classes, a superclass (parent class) and a subclass (a child class). The subclass will inherit all of the properties and methods in the superclass.

`extends` is the keyword that makes the subclass inherit methods from the superclass
`super()` invokes the superclass constructor, setting properties on `this`

Inheritance is useful because:

* It helps keep our code DRY by not having to repeat methods in the superclass

Example:

```js
class Person {
  constructor(name) {
    this.name = name;
    this.friends = [];
  }
  makeFriend(friend) {
    this.friends.push(friend)
    console.log(`Hi ${friend}, my name is ${this.name}, nice to meet you!`);
  }
  doActivity(activity) {
    console.log(`${this.name} is ${activity}`);
  }
}

class Programmer extends Person {
  constructor(name, language) {
    // invoke the Person constructor, setting the name and friends properties on `this`
    super(name); 
    
    // add a favoriteLanguage property only for Programmers
    this.favoriteLanguage = language; 
  }
  
  // makeFriend is inherited
  // doActivity is inherited
  
  code() { // a new method only Programmer instances can use
    this.doActivity(`writing some ${this.favoriteLanguage} code.`);
  }
}

const ben = new Programmer("Ben", "JavaScript");
console.log(ben);
ben.code();
ben.makeFriend("Clifford");
ben.doActivity("Running");
```

## Polymorphism

What is it?

* A parent and child class
* Each class has the same method
* When you implement the method, they have different behavior

Why is it useful?

* Calling code doesn't need to worry about implementation details

Example

```js
class Car {
  constructor(make, model) {
    this.make = make;
    this.model = model;
  }
  
  makeSound() {
    console.log("Vrooom");
  }
}

class RaceCar extends Car {
  constructor(make, model) {
    super(make, model);
  }
  
  makeSound() { // Method Override
    console.log("Vah... Vah...");
    super.makeSound();
    console.log("WHEEEEEEE!!!!");
  }
}

const car1 = new Car("Chevy", "Cobalt");
const car2 = new RaceCar("Ferrari", "Portofino");

car1.makeSound();
car2.makeSound();
```
