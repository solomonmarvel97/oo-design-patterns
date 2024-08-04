### Behavioral Patterns in Python

#### 1. Chain of Responsibility

**Definition:** This pattern allows an object to pass a request along a chain of potential handlers until the request is handled.

**Explanation:** Each handler in the chain decides whether to process the request or pass it to the next handler.

**Example Code:**

```python
class Handler:
    def __init__(self, successor=None):
        self.successor = successor

    def handle(self, request):
        if self.successor:
            self.successor.handle(request)

class ConcreteHandler1(Handler):
    def handle(self, request):
        if request == "R1":
            print("Handler 1 handled the request")
        else:
            super().handle(request)

class ConcreteHandler2(Handler):
    def handle(self, request):
        if request == "R2":
            print("Handler 2 handled the request")
        else:
            super().handle(request)

handler_chain = ConcreteHandler1(ConcreteHandler2())
handler_chain.handle("R1")
handler_chain.handle("R2")
```

#### 2. Command

**Definition:** Encapsulates a request as an object, thereby allowing for parameterization of clients with queues, requests, and operations.

**Explanation:** This pattern allows the decoupling of sender and receiver, enabling operations like undo/redo.

**Example Code:**

```python
class Command:
    def execute(self):
        pass

class LightOnCommand(Command):
    def __init__(self, light):
        self.light = light

    def execute(self):
        self.light.turn_on()

class Light:
    def turn_on(self):
        print("Light is on")

class RemoteControl:
    def __init__(self):
        self.command = None

    def set_command(self, command):
        self.command = command

    def press_button(self):
        self.command.execute()

light = Light()
light_on = LightOnCommand(light)
remote = RemoteControl()
remote.set_command(light_on)
remote.press_button()
```

#### 3. Interpreter

**Definition:** Given a language, define a representation for its grammar along with an interpreter that uses the representation to interpret sentences in the language.

**Explanation:** Useful for simple grammar or language processing.

**Example Code:**

```python
class Expression:
    def interpret(self, context):
        pass

class Number(Expression):
    def __init__(self, number):
        self.number = number

    def interpret(self, context):
        return self.number

class Add(Expression):
    def __init__(self, left, right):
        self.left = left
        self.right = right

    def interpret(self, context):
        return self.left.interpret(context) + self.right.interpret(context)

# Usage
context = {}
expression = Add(Number(3), Number(5))
result = expression.interpret(context)
print(f"Result: {result}")
```

#### 4. Iterator

**Definition:** Provides a way to access elements of an aggregate object sequentially without exposing its underlying representation.

**Explanation:** Allows traversal of container/collection elements without exposing its internal structure.

**Example Code:**

```python
class Iterator:
    def __init__(self, collection):
        self.collection = collection
        self.index = 0

    def has_next(self):
        return self.index < len(self.collection)

    def next(self):
        if self.has_next():
            item = self.collection[self.index]
            self.index += 1
            return item
        else:
            raise StopIteration

collection = [1, 2, 3, 4]
iterator = Iterator(collection)
while iterator.has_next():
    print(iterator.next())
```

#### 5. Mediator

**Definition:** Defines an object that encapsulates how a set of objects interact. Promotes loose coupling by preventing objects from referring to each other explicitly.

**Explanation:** Centralizes complex communications and control logic between objects in a system.

**Example Code:**

```python
class Mediator:
    def notify(self, sender, event):
        pass

class ConcreteMediator(Mediator):
    def __init__(self, component1, component2):
        self.component1 = component1
        self.component1.set_mediator(self)
        self.component2 = component2
        self.component2.set_mediator(self)

    def notify(self, sender, event):
        if event == "A":
            print("Mediator reacts on A and triggers following operations:")
            self.component2.do_c()
        elif event == "D":
            print("Mediator reacts on D and triggers following operations:")
            self.component1.do_b()

class BaseComponent:
    def __init__(self, mediator=None):
        self._mediator = mediator

    def set_mediator(self, mediator):
        self._mediator = mediator

class Component1(BaseComponent):
    def do_a(self):
        print("Component 1 does A.")
        self._mediator.notify(self, "A")

    def do_b(self):
        print("Component 1 does B.")

class Component2(BaseComponent):
    def do_c(self):
        print("Component 2 does C.")

    def do_d(self):
        print("Component 2 does D.")
        self._mediator.notify(self, "D")

component1 = Component1()
component2 = Component2()
mediator = ConcreteMediator(component1, component2)

component1.do_a()
component2.do_d()
```

#### 6. Memento

**Definition:** Captures and externalizes an object's internal state so that it can be restored later without violating encapsulation.

**Explanation:** Useful for implementing undo operations.

**Example Code:**

```python
class Memento:
    def __init__(self, state):
        self._state = state

    def get_state(self):
        return self._state

class Originator:
    def __init__(self, state):
        self._state = state

    def set_state(self, state):
        self._state = state
        print(f"State set to: {self._state}")

    def save(self):
        return Memento(self._state)

    def restore(self, memento):
        self._state = memento.get_state()
        print(f"State restored to: {self._state}")

# Usage
originator = Originator("State1")
memento = originator.save()
originator.set_state("State2")
originator.restore(memento)
```

#### 7. Observer

**Definition:** Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

**Explanation:** Useful for implementing distributed event-handling systems.

**Example Code:**

```python
class Observer:
    def update(self, subject):
        pass

class ConcreteObserver(Observer):
    def __init__(self, name):
        self._name = name

    def update(self, subject):
        print(f"{self._name} received update: {subject.state}")

class Subject:
    def __init__(self):
        self._observers = []
        self._state = None

    def attach(self, observer):
        self._observers.append(observer)

    def detach(self, observer):
        self._observers.remove(observer)

    def notify(self):
        for observer in self._observers:
            observer.update(self)

    @property
    def state(self):
        return self._state

    @state.setter
    def state(self, state):
        self._state = state
        self.notify()

# Usage
subject = Subject()
observer1 = ConcreteObserver("Observer 1")
observer2 = ConcreteObserver("Observer 2")
subject.attach(observer1)
subject.attach(observer2)

subject.state = "State 1"
subject.state = "State 2"
```

#### 8. State

**Definition:** Allows an object to alter its behavior when its internal state changes. The object will appear to change its class.

**Explanation:** Useful for implementing state machines and workflows.

**Example Code:**

```python
class State:
    def handle(self, context):
        pass

class ConcreteStateA(State):
    def handle(self, context):
        print("State A handling request.")
        context.state = ConcreteStateB()

class ConcreteStateB(State):
    def handle(self, context):
        print("State B handling request.")
        context.state = ConcreteStateA()

class Context:
    def __init__(self, state):
        self._state = state

    @property
    def state(self):
        return self._state

    @state.setter
    def state(self, state):
        self._state = state

    def request(self):
        self._state.handle(self)

# Usage
context = Context(ConcreteStateA())
context.request()
context.request()
```

#### 9. Strategy

**Definition:** Defines a family of algorithms, encapsulates each one, and makes them interchangeable. Lets the algorithm vary independently from clients that use it.

**Explanation:** Useful for implementing different variations of an algorithm.

**Example Code:**

```python
class Strategy:
    def execute(self, data):
        pass

class ConcreteStrategyA(Strategy):
    def execute(self, data):
        return sorted(data)

class ConcreteStrategyB(Strategy):
    def execute(self, data):
        return sorted(data, reverse=True)

class Context:
    def __init__(self, strategy):
        self._strategy = strategy

    def set_strategy(self, strategy):
        self._strategy = strategy

    def execute_strategy(self, data):
        return self._strategy.execute(data)

# Usage
data = [1, 3, 5, 7, 9, 2, 4, 6, 8, 0]
context = Context(ConcreteStrategyA())
print(context.execute_strategy(data))

context.set_strategy(ConcreteStrategyB())
print(context.execute_strategy(data))
```

#### 10. Template Method

**Definition:** Defines the skeleton of an algorithm in an operation, deferring some

 steps to subclasses. Allows subclasses to redefine certain steps of an algorithm without changing its structure.

**Explanation:** Useful for defining the outline of an algorithm while letting subclasses implement specific steps.

**Example Code:**

```python
class AbstractClass:
    def template_method(self):
        self.step1()
        self.step2()
        self.step3()

    def step1(self):
        pass

    def step2(self):
        pass

    def step3(self):
        pass

class ConcreteClassA(AbstractClass):
    def step1(self):
        print("Class A Step 1")

    def step2(self):
        print("Class A Step 2")

    def step3(self):
        print("Class A Step 3")

class ConcreteClassB(AbstractClass):
    def step1(self):
        print("Class B Step 1")

    def step2(self):
        print("Class B Step 2")

    def step3(self):
        print("Class B Step 3")

# Usage
class_a = ConcreteClassA()
class_a.template_method()

class_b = ConcreteClassB()
class_b.template_method()
```

#### 11. Visitor

**Definition:** Represents an operation to be performed on the elements of an object structure. Lets you define a new operation without changing the classes of the elements on which it operates.

**Explanation:** Useful for adding operations to complex class hierarchies without modifying the classes.

**Example Code:**

```python
class Visitor:
    def visit(self, element):
        pass

class ConcreteVisitor1(Visitor):
    def visit(self, element):
        print(f"Visitor 1 visiting {element.name}")

class ConcreteVisitor2(Visitor):
    def visit(self, element):
        print(f"Visitor 2 visiting {element.name}")

class Element:
    def accept(self, visitor):
        pass

class ConcreteElementA(Element):
    def __init__(self):
        self.name = "Element A"

    def accept(self, visitor):
        visitor.visit(self)

class ConcreteElementB(Element):
    def __init__(self):
        self.name = "Element B"

    def accept(self, visitor):
        visitor.visit(self)

# Usage
elements = [ConcreteElementA(), ConcreteElementB()]
visitor1 = ConcreteVisitor1()
visitor2 = ConcreteVisitor2()

for element in elements:
    element.accept(visitor1)
    element.accept(visitor2)
```

These examples demonstrate the implementation of various behavioral patterns in Python, showing how to encapsulate different behaviors and enable flexible and maintainable code.