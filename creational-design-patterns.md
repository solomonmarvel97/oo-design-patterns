**1. Abstract Factory**

The Abstract Factory pattern provides a way to create families of related objects without specifying their concrete classes. It allows you to create objects without specifying the exact class of object that will be created.

Example:
```
from abc import ABC, abstractmethod

# Abstract classes
class AbstractFactory(ABC):
    @abstractmethod
    def create_product_a(self):
        pass

    @abstractmethod
    def create_product_b(self):
        pass

class ConcreteFactoryA(AbstractFactory):
    def create_product_a(self):
        return ProductA()

    def create_product_b(self):
        return ProductB()

class ConcreteFactoryB(AbstractFactory):
    def create_product_a(self):
        return ProductC()

    def create_product_b(self):
        return ProductD()

# Concrete products
class ProductA:
    def __init__(self):
        print("Creating Product A")

    def operation(self):
        print("Operating on Product A")

class ProductB:
    def __init__(self):
        print("Creating Product B")

    def operation(self):
        print("Operating on Product B")

class ProductC:
    def __init__(self):
        print("Creating Product C")

    def operation(self):
        print("Operating on Product C")

class ProductD:
    def __init__(self):
        print("Creating Product D")

    def operation(self):
        print("Operating on Product D")

# Client code
def client_code(factory: AbstractFactory):
    product_a = factory.create_product_a()
    product_b = factory.create_product_b()

    product_a.operation()
    product_b.operation()

# Usage
client_code(ConcreteFactoryA())
client_code(ConcreteFactoryB())
```
Scenario: When you need to create a family of related objects without specifying the exact class of object that will be created.

**2. Builder**

The Builder pattern separates the construction of an object from its representation, allowing you to create complex objects step-by-step.

Example:
```
class Car:
    def __init__(self):
        self.wheels = 0
        self.doors = 0
        self.color = ""

    def set_wheels(self, wheels):
        self.wheels = wheels

    def set_doors(self, doors):
        self.doors = doors

    def set_color(self, color):
        self.color = color

    def __str__(self):
        return f"Car with {self.wheels} wheels, {self.doors} doors, and {self.color} color"

class CarBuilder:
    def __init__(self):
        self.car = Car()

    def set_wheels(self, wheels):
        self.car.set_wheels(wheels)
        return self

    def set_doors(self, doors):
        self.car.set_doors(doors)
        return self

    def set_color(self, color):
        self.car.set_color(color)
        return self

    def build(self):
        return self.car

# Client code
builder = CarBuilder()
car = builder.set_wheels(4).set_doors(2).set_color("red").build()
print(car)
```
Scenario: When you need to create complex objects step-by-step, without exposing the underlying construction process.

**3. Factory Method**

The Factory Method pattern provides a way to create objects without specifying the exact class of object that will be created, while still allowing subclasses to override the creation process.

Example:
```
from abc import ABC, abstractmethod

class AbstractProduct(ABC):
    @abstractmethod
    def operation(self):
        pass

class ConcreteProductA(AbstractProduct):
    def operation(self):
        print("Operating on Product A")

class ConcreteProductB(AbstractProduct):
    def operation(self):
        print("Operating on Product B")

class AbstractFactory(ABC):
    @abstractmethod
    def create_product(self):
        pass

class ConcreteFactoryA(AbstractFactory):
    def create_product(self):
        return ConcreteProductA()

class ConcreteFactoryB(AbstractFactory):
    def create_product(self):
        return ConcreteProductB()

# Client code
def client_code(factory: AbstractFactory):
    product = factory.create_product()
    product.operation()

# Usage
client_code(ConcreteFactoryA())
client_code(ConcreteFactoryB())
```
Scenario: When you need to create objects without specifying the exact class of object that will be created, while still allowing subclasses to override the creation process.

**4. Prototype**

The Prototype pattern provides a way to create objects by copying an existing object, rather than creating a new object from scratch.

Example:
```
class Prototype:
    def __init__(self):
        self._objects = {}

    def register_object(self, name, obj):
        self._objects[name] = obj

    def unregister_object(self, name):
        del self._objects[name]

    def clone(self, name, **attr):
        obj = self._objects.get(name)
        if not obj:
            raise ValueError(f"No object registered with name {name}")
        obj.__dict__.update(attr)
        return obj

class Car:
    def __init__(self):
        self.color = "red"
        self.wheels = 4

    def __str__(self):
        return f"Car with {self.wheels} wheels and {self.color} color"

# Client code
prototype = Prototype()
car = Car()
prototype.register_object("car", car)

new_car = prototype.clone("car", wheels=5)
print(new_car)
```
Scenario: When you need to create objects by copying an existing object, rather than creating a new object from scratch.

**5. Singleton**

The Singleton pattern ensures that only one instance of a class is created, and provides a global point of access to that instance.

Example:
```
class Singleton:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super(Singleton, cls).__new__(cls)
        return cls._instance

    def __init__(self):
        if not hasattr(self, 'initialized'):
            self.initialized = True
            print("Initializing Singleton")

    def do_something(self):
        print("Doing something")

# Client code
singleton = Singleton()
singleton.do_something()

another_singleton = Singleton()
print(another_singleton is singleton)
```
Scenario: When you need to ensure that only one instance of a class is created, and provide a global point of access to that instance.

**6. Lazy Initialization**

Lazy Initialization is a technique used to delay the initialization of an object until it is actually needed.

Example:
```
class LazyInitialization:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super(LazyInitialization, cls).__new__(cls)
            cls._instance.__init__()
        return cls._instance

    def __init__(self):
        if not hasattr(self, 'initialized'):
            self.initialized = True
            print("Initializing LazyInitialization")

    def do_something(self):
        print("Doing something")

# Client code
lazy_initialization = LazyInitialization()
lazy_initialization.do_something()
```
Scenario: When you need to delay the initialization of an object until it is actually needed.