# Task1

## OOP in python

Python is an object oriented programming language.

#### Almost everything in Python is an object, with its properties and methods.

A Class is like an object constructor, or a "blueprint" for creating objects.

```python
class Person:
  pass
p1 = Person()
p1.name = 'amir'
```

```python
class Person:
  def __init__(self, name, age):
    self.name = name
    self.age = age

p1 = Person("John", 36)

print(p1.name)
print(p1.age)
```
Note: The` __init__()` function is called automatically every time the class is being used to create a new object.

### The `__str__()` Function
The `__str__()` function controls what should be returned when the class object is represented as a string.

If the `__str__()` function is not set, the string representation of the object is returned:
```python
class Person:
  def __init__(self, name, age):
    self.name = name
    self.age = age

p1 = Person("John", 36)

print(p1)
```
````python
class Person:
  def __init__(self, name, age):
    self.name = name
    self.age = age

  def __str__(self):
    return f"{self.name}({self.age})"

p1 = Person("John", 36)

print(p1)
````
### The self Parameter
The self parameter is a reference to the current instance of the class, and is used to access variables that belongs to the class.

It does not have to be named self , you can call it whatever you like, but it has to be the first parameter of any function in the class:
```python
class Person:
  def __init__(mysillyobject, name, age):
    mysillyobject.name = name
    mysillyobject.age = age

  def myfunc(abc):
    print("Hello my name is " + abc.name)

p1 = Person("John", 36)
p1.myfunc()
```
Delete Object Properties

`del p1.age`

Delete Objects

`del p1`

### Inheritance
```python
class Person:
    def __init__(self, fname, lname):
        self.fname = fname
        self.lname = lname

    def __str__(self):
        return f'{self.fname}---{self.lname}'

class Student(Person):
    def __init__(self, fname, lname, stdCode):
      Person.__init__(self, fname, lname)
      self.stdCode = stdCode

p1 = Person('amir', 'khadem')
print(p1)
p2 = Student('amir', 'khadgggggggggggggggem', 1232143343)
print(p2)
```


### Python - Packages

1. Create a new folder named D:\MyApp.
2. Inside MyApp, create a subfolder with the name 'mypackage'.
3. Create an empty` __init__.py` file in the mypackage folder.

#### greet.py
```python
def SayHello(name):
    print("Hello ", name)
```
#### functions.py
```python
def sum(x,y):
    return x+y

def average(x,y):
    return (x+y)/2

def power(x,y):
    return x**y
```
<img src="https://www.tutorialsteacher.com/Content/images/python/package.png" >

Now, to test our package, navigate the command prompt to the MyApp folder and invoke the Python prompt from there.

`D:\MyApp>python`

`Import the functions module from the mypackage package and call its power() function.`

`>>> from mypackage import functions`

`>>> functions.power(3,2)` 

`9`

It is also possible to import specific functions from a module in the package.

`>>> from mypackage.functions import sum`

`>>> sum(10,20)`

`30`

### `__init__.py`

The package folder contains a special file called `__init__.py`, which stores the package's content. It serves two purposes:

1. The Python interpreter recognizes a folder as the package if it contains __init__.py file.
2. `__init__.py` exposes specified resources from its modules to be imported.

An empty `__init__.py` file makes all functions from the above modules available when this package is imported. Note that`__init__.py` is essential for the folder to be recognized by Python as a package. You can optionally define functions from individual modules to be made available.

#### Note : We shall also create another Python script in the MyApp folder and import the mypackage package in it. It should be at the same level of the package to be imported.

The `__init__.py` file is normally kept empty. However, it can also be used to choose specific functions from modules in the package folder and make them available for import. Modify `__init__.py` as below:


#### `__init__.py`
```python
from .functions import average, power
from .greet import SayHello
```


The specified functions can now be imported in the interpreter session or another executable script.

Create `test.py` in the `MyApp` folder to test mypackage.

#### `test.py`
```python
from mypackage import power, average, SayHello
SayHello()
x=power(3,2)
print("power(3,2) : ", x)
```
`D:\MyApp>python test.py`

`Hello world`

`power(3,2) : 9`


### Install a Package Globally

Once a package is created, it can be installed for system-wide use by running the setup script. The script calls `setup()` function from the `setuptools` module.

Let's install `mypackage` for system-wide use by running a setup script.

Save the following code as setup.py in the parent folder `MyApp`. The script calls the `setup()` function from the setuptools module. The `setup()` function takes various arguments such as name, version, author, list of dependencies, etc. The `zip_safe` argument defines whether the package is installed in compressed mode or regular mode.

#### setup.py
```python
from setuptools import setup
setup(name='mypackage',
version='0.1',
description='Testing installation of Package',
url='#',
author='auth',
author_email='author@email.com',
license='MIT',
packages=['mypackage'],
zip_safe=False)
```
`D:\MyApp>pip install mypackage`

`Processing d:\MyApp`

`Installing collected packages: mypack`

`Running setup.py install for mypack ... done`

`Successfully installed mypackage-0.1`

## SOLID in python
### Single Responsibility Principle
### Open/Closed Principle
### Liskov Substitute Principle
### Interface Segregation Principle
### Dependency Inversion Principle
____________________________________________________________________
#### 1. Single Responsibility Principle (SRP)


Solid rules were created in 2000 by Robert Martin. These five laws are used by programmers in object-oriented programming to create more understandable, scalable and flexible programs.

In this article, we examine the first of these five laws:

Single responsibility principle
In short, this rule says that each class should be responsible for doing only one thing. As a result, each class will be modified only to change its function. Classes can have different features, but all these features must be related to the main task of the class.

In the code below, we have a class that takes and stores the names and ages of people

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def show(self):
        return f'{self.name} is {self.age} years old'

    def db_save(self):
        pass
```
In the picture above, you can see that the Person class, in addition to being responsible for managing people's characteristics, also has the task of connecting to the database and storing information. This code violates the single responsibility law.

To solve this problem, we divide this code into two separate classes:
```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def show(self):
        return f'{self.name} is {self.age} years old'


class Db:
    def db_save(self):
        pass
```
now it's better.

In the code above, if the programmer needs to change one class, he doesn't have to worry about creating problems for other classes.

------------------------------------------------------------------------
#### 2. Open/Closed Principle (OCP)
This law says that you should program in such a way that if your program needs to be expanded, you can add new features to the program without changing the previous codes.
In other words, your code should be open for extension but closed for modification.

Example

In the code below, the Animal class takes the name of an animal and gives us its sound based on the type of animal in the sound method. so simple.
```python
class Animal:
	def __init__(self, name):
		self.name = name

	def sound(self):
		if self.name == 'cat':
			print('meow')
		elif self.name == 'dog':
			print('woof')

missy = Animal('cat')
jack = Animal('dog')

missy.sound()
jack.sound()
```
The above code is against the open/closed rule. Why?
Think that we want to develop our program and add the sound of the snake to it, in this case we have to change the sound method, which is against the law. as follows:
```python
class Animal:
	def __init__(self, name):
		self.name = name

	def sound(self):
		if self.name == 'cat':
			print('meow')
		elif self.name == 'dog':
			print('woof')
		elif self.name == 'snake':
			print('hiss')

missy = Animal('cat')
jack = Animal('dog')
mark = Animal('snake')

missy.sound()
jack.sound()
mark.sound()
```
Now, in order for our code to comply with this rule, we can change it as follows:
```python

class Animal:
	def __init__(self, name):
		self.name = name

	def sound(self):
		pass
	
class Cat(Animal):
	def sound(self):
		print('meow')
		
class Dog(Animal):
	def sound(self):
		print('woof')
		
class Snake(Animal):
	def sound(self):
		print('hiss')
```
In the code above, we create a separate class for each animal, in this case, if we want to add a new animal, we don't need to change our previous codes.

------------------------------------------------------------------------
#### 3. Liskov Substitute Principle (LSP)

Liskov Substitution Principle
A sub-class must be substitutable for its super-class.
The aim of this principle is to ascertain that a sub-class can assume the place of its super-class without errors. 
If the code finds itself checking the type of class then, it must have violated this principle.
Let’s use our Animal example.
```python
def animal_leg_count(animals: list):
    for animal in animals:
        if isinstance(animal, Lion):
            print(lion_leg_count(animal))
        elif isinstance(animal, Mouse):
            print(mouse_leg_count(animal))
        elif isinstance(animal, Pigeon):
            print(pigeon_leg_count(animal))
        
animal_leg_count(animals)
```
To make this function follow the LSP principle, we will follow this LSP requirements postulated by Steve Fenton:
If the super-class (Animal) has a method that accepts a super-class type (Animal) parameter. 
Its sub-class(Pigeon) should accept as argument a super-class type (Animal type) or sub-class type(Pigeon type).
If the super-class returns a super-class type (Animal). 
Its sub-class should return a super-class type (Animal type) or sub-class type(Pigeon).
Now, we can re-implement animal_leg_count function:
```python
def animal_leg_count(animals: list):
    for animal in animals:
        print(animal.leg_count())
        
animal_leg_count(animals)
```
The animal_leg_count function cares less the type of Animal passed, it just calls the leg_count method. 
All it knows is that the parameter must be of an Animal type, either the Animal class or its sub-class.
The Animal class now have to implement/define a leg_count method.
And its sub-classes have to implement the leg_count method:
```python
class Animal:
    def leg_count(self):
        pass


class Lion(Animal):
    def leg_count(self):
        pass
```
When it’s passed to the animal_leg_count function, it returns the number of legs a lion has.
You see, the animal_leg_count doesn’t need to know the type of Animal to return its leg count, 
it just calls the leg_count method of the Animal type because by contract a sub-class of Animal class must implement the leg_count function.


------------------------------------------------------------------------
#### 4. Interface Segregation Principle (ISP)

Interface Segregation Principle
Make fine grained interfaces that are client specific
Clients should not be forced to depend upon interfaces that they do not use.
This principle deals with the disadvantages of implementing big interfaces.
Let’s look at the below IShape interface:
```python
class IShape:
    def draw_square(self):
        raise NotImplementedError
    
    def draw_rectangle(self):
        raise NotImplementedError
    
    def draw_circle(self):
        raise NotImplementedError
```
This interface draws squares, circles, rectangles. class Circle, Square or Rectangle implementing the IShape 
interface must define the methods draw_square(), draw_rectangle(), draw_circle().
```python
class Circle(IShape):
    def draw_square(self):
        pass

    def draw_rectangle(self):
        pass
    
    def draw_circle(self):
        pass

class Square(IShape):
    def draw_square(self):
        pass

    def draw_rectangle(self):
        pass
    
    def draw_circle(self):
        pass

class Rectangle(IShape):
    def draw_square(self):
        pass

    def draw_rectangle(self):
        pass
    
    def draw_circle(self):
        pass

```
It’s quite funny looking at the code above. class Rectangle implements methods (draw_circle and draw_square) it has no use of, 
likewise Square implementing draw_circle, and draw_rectangle, and class Circle (draw_square, draw_rectangle).
If we add another method to the IShape interface, like draw_triangle(),
```python
class IShape:
    def draw_square(self):
        raise NotImplementedError
    
    def draw_rectangle(self):
        raise NotImplementedError
    
    def draw_circle(self):
        raise NotImplementedError
    
    def draw_triangle(self):
        raise NotImplementedError

```
the classes must implement the new method or error will be thrown.
We see that it is impossible to implement a shape that can draw a circle but not a rectangle or a square or a triangle. 
We can just implement the methods to throw an error that shows the operation cannot be performed.
ISP frowns against the design of this IShape interface. clients (here Rectangle, Circle, and Square) should not be forced to depend on methods that they do not need or use. 
Also, ISP states that interfaces should perform only one job (just like the SRP principle) any extra grouping of behavior should be abstracted away to another interface.
Here, our IShape interface performs actions that should be handled independently by other interfaces.
To make our IShape interface conform to the ISP principle, we segregate the actions to different interfaces.
Classes (Circle, Rectangle, Square, Triangle, etc) can just inherit from the IShape interface and implement their own draw behavior.

```python
class IShape:
    def draw(self):
        raise NotImplementedError

class Circle(IShape):
    def draw(self):
        pass

class Square(IShape):
    def draw(self):
        pass

class Rectangle(IShape):
    def draw(self):
        pass
```
We can then use the I -interfaces to create Shape specifics like Semi Circle, Right-Angled Triangle, Equilateral Triangle, Blunt-Edged Rectangle, etc.

------------------------------------------------------------------------
#### 4. Dependency Inversion Principle (DIP)

Dependency Inversion Principle
Dependency should be on abstractions not concretions
A. High-level modules should not depend upon low-level modules. Both should depend upon abstractions.
B. Abstractions should not depend on details. Details should depend upon abstractions.
There comes a point in software development where our app will be largely composed of modules. 
When this happens, we have to clear things up by using dependency injection. 
High-level components depending on low-level components to function.


```python
class XMLHttpService(XMLHttpRequestService):
    pass

class Http:
    def __init__(self, xml_http_service: XMLHttpService):
        self.xml_http_service = xml_http_service
    
    def get(self, url: str, options: dict):
        self.xml_http_service.request(url, 'GET')

    def post(self, url, options: dict):
        self.xml_http_service.request(url, 'POST')
```

Here, Http is the high-level component whereas HttpService is the low-level component. 
This design violates DIP A: High-level modules should not depend on low-level level modules. It should depend upon its abstraction.
This Http class is forced to depend upon the XMLHttpService class. 
If we were to change to change the Http connection service, maybe we want to connect to the internet through cURL or even Mock the http service. 
We will painstakingly have to move through all the instances of Http to edit the code and this violates the OCP principle.
The Http class should care less the type of Http service you are using. We make a Connection interface:

```python
class Connection:
    def request(self, url: str, options: dict):
        raise NotImplementedError
```

The Connection interface has a request method. With this, we pass in an argument of type Connection to our Http class:

```python
class Http:
    def __init__(self, http_connection: Connection):
        self.http_connection = http_connection
    
    def get(self, url: str, options: dict):
        self.http_connection.request(url, 'GET')

    def post(self, url, options: dict):
        self.http_connection.request(url, 'POST')
```

So now, no matter the type of Http connection service passed to Http it can easily connect to a network 
without bothering to know the type of network connection.
We can now re-implement our XMLHttpService class to implement the Connection interface:
```python
class XMLHttpService(Connection):
    xhr = XMLHttpRequest()

    def request(self, url: str, options:dict):
        self.xhr.open()
        self.xhr.send()
```
We can create many Http Connection types and pass it to our Http class without any fuss about errors.
```python
class NodeHttpService(Connection):
    def request(self, url: str, options:dict):
        pass

class MockHttpService(Connection):
    def request(self, url: str, options:dict):
        pass
```
Now, we can see that both high-level modules and low-level modules depend on abstractions. 
Http class(high level module) depends on the Connection interface(abstraction) and 
the Http service types(low level modules) in turn, depends on the Connection interface(abstraction).
Also, this DIP will force us not to violate the Liskov Substitution Principle: 
The Connection types Node-XML-MockHttpService are substitutable for their parent type Connection.

## Lambda Function
```python
x = [0, 1, 3, 7, 4, 3, 8, 4]
def power(i):
  return i*i

print(list(map(power, x)))
print(list(map(lambda i : i*i , x)))
```
```python
x = [0, 1, 3, 7, 4, 3, 8, 4]
def even(i):
  return i%2 == 0

print(list(filter(even, x)))
print(list(filter(lambda i:i%2==0, x)))
```
## List Compensations 
```python
list = []
for i in range(1, 11):
    list.append(i*i)
print(list)
list2 = [i*i for i in range(1,11)]
print(list2)
```









