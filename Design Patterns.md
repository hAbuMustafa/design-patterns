# Design Patterns

<div class="table-of-contents">

#### Content:

- [Creational Patterns](#creational-patterns)

  - [Singleton Pattern](#singleton-pattern)
  - [Factory Pattern](#factory-pattern)
  - [Builder Pattern](#builder-pattern)
  - [Constructor Pattern](#constructor-pattern)
  - [Prototype Pattern](#prototype-pattern)
  - [Module Pattern](#module-pattern)
  - [Abstract Pattern](#abstract-pattern)\*

- [Structural Patterns](#structural-patterns)

  - [Decorator Pattern](#decorator-pattern)
  - [Facade Pattern](#facade-pattern)
  - [Adapter Pattern](#adapter-pattern)
  - [Bridge Pattern](#bridge-pattern)
  - [Composite Pattern](#composite-pattern)
  - [Flyweight Pattern](#flyweight-pattern)\*
  - [Proxy Pattern](#proxy-pattern)\*

- [Behavioral Patterns](#behavioral-patterns)

  - [Observer Pattern](#observer-pattern)
  - [Strategy Pattern](#strategy-pattern)
  - [Command Pattern](#command-pattern)
  - [Iterator Pattern](#iterator-pattern)
  - [Mediator Pattern](#mediator-pattern)
  - [Visitor Pattern](#visitor-pattern)\*

\* Denotes articles from [Jaimal](https://dev.to/jaimaldullat/javascript-design-patterns-the-ultimate-guide-part-1-1ehd)'s series. Others are from [Tope](https://dev.to/topefasasi/js-design-patterns-a-comprehensive-guide-h3m)'s article.

</div>

## Creational Patterns:

Describes methods for creating new objects or instances of them.

### Singleton Pattern

Allows for only one instance of a class to exist. When creating an instance with the `new` keyword, the instance will refer to the first insance, if exists, or creates it otherwise.

```javascript
const a = new MyObj();
const b = new MyObj();

console.log(a === b); // <= true
```

This is because the class constructor returns the available instance if exists, or creates a new one.

```javascript
class MyObj {
  constructor() {
    if (!MyObj.instance) {
      MyObj.instance = this;
    }
    return MyObj.instance;
  }
}
```

### Factory Pattern

Allows for separation of original class logic, by creating a factory class that creates instances of the original class, without referencing the original class.

```javascript
class MyObj {
  constructor(name) {
    this.name = name;
  }
}

class MyObjFactory {
  createObj(name) {
    return new MyObj(name);
  }
}

//  Usage

const obj = new MyObjFactory().createObj('John');
```

### Builder Pattern

It uses a chaining pattern on a class that's created by chaining property setters. It's useful for creating complex objects with a lot of properties.

```javascript
class Person {
  constructor(name, age) {
    this.name = '';
    this.age = 0;
  }
}

class PersonBuilder {
  constructor(name, age) {
    this.person = new Person();
  }

  setName(name) {
    this.person.name = name;
    return this;
  }

  setAge(age) {
    this.person.age = age;
    return this;
  }

  build() {
    return this.person;
  }
}

// Usage

const personBuilder = new PersonBuilder();
const person = personBuilder.setName('John').setAge(25).build();
```

It differs from the factory pattern by the fact that the builder pattern creates a new object for each property set.

### Constructor Pattern

Using functions rather than classes, it creates instances by simply passing in the properties.

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}

// Usage
const me = new Person('Hosam', 31);
const zoz = new Person('Hazem', 30);
```

### Prototype Pattern

Uses `Object.create()` to create a new object, and then copies the properties from the original object.
This allows populating all instances with the same properties in their prototype.

```javascript
const person = {
  kingdom: 'animalia',
  phylum: 'homosapiens',
  limbs: {
    hands: 2,
    legs: 2,
  },
};

// Usage

const me = Object.create(person);
me.name = 'Hosam';
me.age = 31;

console.log(me.limbs.hands); // <= 2
```

### Module Pattern

It uses an IIFE to encapsulate private logic of the constructor from the public logic.

```javascript
const myClass = (function () {
  variable = "I'm private";

  return {
    variable: "I'm public",
  };
})();

// Usage
console.log(myClass.variable); // <= I'm public
```

Notice that we didn't do any instantiation.

## Structural Patterns:

### Decorator Pattern

It is used as a wrapper around other objects, to add new functionality to them.

```javascript
class Animal {
  get getLimbs() {
    return 2;
  }
}

// This is decorator class
class Lion {
  constructor(animal) {
    this.animal = animal;
  }

  getLimbs() {
    return this.animal.getLimbs() + 2;
  }
}

// Usage
const animal = new Animal();
const lion = new Lion(animal);
```

### Facade Pattern

Provides a unified and simplified interface to interact with complex subsystems, abstracting their complexities and making them easier to use.

```javascript
class ComplexA {
  complexProcess() {
    console.log('Complex operation A executed.');
  }
}

class ComplexB {
  complexProcess() {
    console.log('Complex operation B executed.');
  }
}

class Facade {
  constructor() {
    this.a = new ComplexA();
    this.b = new ComplexB();
  }

  Process() {
    this.a.complexProcess();
    this.b.complexProcess();
  }
}

// Usage

const facade = new Facade();
facade.Process(); // <= Complex operation A executed. Complex operation B executed.
```

### Adapter Pattern

It provides a way to accomodate/compensate for incompatible interfaces.

```javascript
class oldInterface {
  method1(param) {
    console.log(`Old method 1 ${param}`);
  }
}

class newInterface {
  method1(param) {
    console.log(`New method 1 ${param}`);
  }
}

class InterfaceAdapter extends newInterface {
  // <= Note the inheritance
  constructor() {
    super();
    this.oldInterface = new oldInterface();
  }

  method1(param) {
    this.oldInterface.method1(param);
  }
}

// Usage
const interface = new InterfaceAdapter();
interface.method1('Hello World'); // <= Old method 1 Hello World
```

### Bridge Pattern

Separates the abstraction and implementation of a system, allowing it to evolve independently.

```javascript
class Shape {
  constructor(color) {
    this.color = color;
  }

  draw() {}
}

// Concrete Abstraction
class Circle extends Shape {
  draw(radius) {
    this.radius = radius;
    return Math.PI * radius * radius;
  }
}

// Usage
const redCircle = new Circle('red');
redCircle.draw(2);
```

### Composite Pattern

Allows for the creation of hierarchical structures where each element can be treated as a single object or a collection of objects.

```javascript
class Employee {
  constructor(name) {
    this.name = name;
  }

  print() {
    console.log(`--- ${this.name}`);
  }
}

class Manager extends Employee {
  constructor(name) {
    super(name);
    this.employees = [];
  }

  add(employee) {
    this.employees.push(employee);
  }

  remove(employee) {
    const index = this.employees.indexOf(employee);
    this.employees.splice(index, 1);
  }

  print() {
    // <= Notice the method override
    console.log(`- ${this.name}`);
    for (const emp of this.employees) {
      emp.print();
    }
  }
}

// Usage
const hosam = new Employee('Hosam');
const madonna = new Employee('Madonna');

const shaimaa = new Manager('Shaimaa');
shaimaa.add(hosam);
shaimaa.add(madonna);

const reem = new Manager('Reem');
reem.add(shaimaa);

reem.print();
```

### Flyweight Pattern

### Proxy Pattern

## Behavioral Patterns:

### Observer Pattern

Provides coupling of the subject and the observer after any change in the subject.

```javascript
class Observer {
  constructor(name) {
    this.name = name;
  }

  update() {
    console.log(`${this.name} is updated.`);
  }
}

class Subject {
  constructor() {
    this.observers = [];
  }

  addObserver(observer) {
    this.observers.push(observer);
  }

  removeObserver(observer) {
    const index = this.observers.indexOf(observer);
    this.observers.splice(index, 1);
  }

  notifyObservers() {
    this.observers.forEach((observer) => observer.update());
  }
}

const subject = new Subject();
const pageTitleObserver = new Observer('page title');
const clientCountObserver = new Observer('client count');

subject.addObserver(pageTitleObserver);
subject.addObserver(clientCountObserver);

subject.notifyObservers();
/* <= 
  page title is updated.
  client count is updated.
 */
```

This is the pattern implemented in signals and state management systems to manifest reactivity

### Strategy Pattern

Allows for dynamic selection of behavior at runtime by encapsulating it in different strategy objects

```javascript
class Context {
  constructor(strategy) {
    this.strategy = strategy;
  }

  execute() {
    this.strategy.execute();
  }
}

class StrategyA {
  execute() {
    console.log('Strategy A executed.');
  }
}

class StrategyB {
  execute() {
    console.log('Strategy B executed.');
  }
}

const contextA = new Context(new StrategyA());
contextA.execute(); // <= Strategy A executed.
```

### Command Pattern

### Iterator Pattern

### Mediator Pattern

### Visitor Pattern

<span id="to-top"><a href="#design-patterns">Top</a></span>

<style type="text/css">
/* Counters */
  h2:first-of-type {
    counter-reset: h2;
  }

  h3:first-of-type, h2 + h3{
    counter-reset: h3;
  }

  h2::before { 
    counter-increment: h2;
    content: counter(h2) ") ";
  }

  h3::before {
    counter-increment: h3;
    content: counter(h2) "." counter(h3) " - ";
  }

/* Decoration */
  .table-of-contents {
    border: 1px solid #555;
    border-radius: 0.5rem;
    padding-left: 1rem;
    background-color: #333;
  }

  .table-of-contents h4{
    margin-top: 0.5rem;
    font-size: 2rem;
  }

  .table-of-contents ul{
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(12rem, 1fr));
  }

  .table-of-contents ul ul {
    margin-left: -1rem;
    margin-top: -0.75rem;
  }

  h3 { 
    margin-inline-start: 1rem;
  }

  h3::before {
    vertical-align: 30%;
    font-size: 0.7rem
  }

  #to-top {
    background-color: #333;
    border-radius: 50%;
    padding: 0.5rem;
    box-shadow: 0 0 0.5rem #555;

    position: fixed;
    inset-inline-end: 1rem;
    inset-block-end: 1rem;
  }
</style>
