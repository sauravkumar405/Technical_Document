# [SOLID Principles](#="")

## Introduction

***

* SOLID principles establish practices that lend to developing software with considerations for maintaining and extending as the project grows. Adopting these practices can also contribute to avoiding code smells, refactoring code, and Agile or Adaptive software development.

* SOLID principles are the design principles that enables us manage most of the software design problems.

* The term SOLID is an acronym for five design principles intended to make software designsmore understandable, flexible and maintainable

* The principles are a subset of many principles promoted by Robert C. Martin

* The SOLID acronym was first introduced by Michael Feathers

## SOLID Acronym

***

* S : Single responsibility Principle (SRP)

* O : Open closed Principle (OSP)

* L : Liskov substitutional Principle (LSP)

* I : Interface Segregation Principle (ISP)

* D : Dependency Inversion Principle (DIP)

## Principles

***

### 1.  Single Responsibility Principle
<br>

> A class should have one and only one reason to change.

* Every module of class should have a responsibility over a single part of the functionality provided by the software, and that responsibility should be encapsulated by the class

* Whether you are talking about a class, a component, a package or an application thay should all have one responsibility. You should be able to define what an application is doing in a single line nd when you achieve this principle the probability for code resuse increases.

Let's take an example,

```Java
public class Employee {\
     public Money calculatePay()...\
     public String reportHours()...\
     public void save()...\
 }
 ```

Here Employee class is designed to manage three calculations which includes calulation of the pay creating a report and saving the details of the employee. So clearly it is not meeting the single responsibility principle and we will not be able to use this code somewhere else.
***

### 2.  Open/Closed Principle
<br>
> "Software entities should be open for extension, but closed for modification"

* The design and writing of the code should be done in a way that the new functionality should be added with minimum changes in the existing code

* The design should be done in a way to allow the adding of new functionality as new classes, keeping as much as possible existing code unchanged

Let's take an example,

```Java
 public double Area(object[] shape){
    double area = 0;
    for each(var shape in shapes){
        if(shape is Rectangle)
        {
            Rectangle rectangle=(Rectangle) shape;
            area+=rectangle.Width*rectangle.Height;
        }
    }
 }
```

Here we can see a function which calculates the area of a given object, but to identify the shape of a given object if else statement is being used. Currently the function only checks for rectangle and circle, but what if we want to a new shape to calculate its area. In such case we need to modify the code of our function which violates the principle. So a better solution would be allowing each of the shape to define their own Area method.

```Java
public abstract class Shape
{
    public abstract double Area();
}

public class Rectangle : Shape
{
    public double Width { get; set;}
    public double Height { get; set; }
    public override double Area()
    {
        return Width*Height;
    }
}
public class Circle : shape
{
    public double Radius {get; set;}
    public override double Area()
    {
        return Radius*Radius*Math.Pi;
    }
}
public double Area(Shape[] shapes)
{
    double area = 0;
    foreach (var shape in shape)
    {
        area+=shape.Area();
    } 
    return area;
}
```

Now if we want to add an new shape we do not need to modify the Area function and hence it is closed for modification.
***

### 3.  Liskov Substitution Principle
<br>
> Introduced by Barbara Liskov state that "objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program"

* If a program module is using a base class, then the reference to the Base class can be replaced with a Derived class without affecting the functionality of the program module
* We can also state that Derived types must be substitutable for their base types
* In simple words we should use inheritance only when it is needed, when our super class is replacable by a sub-class in all the instances.

Let's take an example,

```Java
class Rectangle
{
    void setWidth(double w)
    void setHeight(double h)
    double getHeight()
    double getWidth();
}
class Square : Rectangle 
{
    void setWidth(double w) //set both height and width to w
    void setHeight(double h)//set height and width to h
    double getHeight()
    double getWidth()
}
```

 Here we have a class rectangle which has some functions which are intended to set the dimensions of the shape, similarly we have a class named Square which has been inherited from ractangle class. This is a bad implementation according to Liskov Substitution Principle.

 Here's why,

 ```Java
 void test(Rectangle r)
 {
     r.setWidth(5);
     r.setWidth(4);
     assertEquals(5*4, r.getWidth()*r.getHeight());
 }
 ```

Consider this function which is used to calculate the area of rectangle, this code will run perfect on the rectangle class but in the case of Square class it will not, beacuse in square class we are setting both height and width values as h, so we will get wrong answer.
***

### 4.  Interface Segregation Principle
<br>
> The dependency of one class to another one should depend on the smallest possible interface.

* Client should not be forced to implement interfaces they don't use.
* Instead of one fat interface many small interfaces are preffered based on the groups of methods, each one serving one submodule.

Let's take an example,

```Java
Animal 
{
    void feed(); //abstract
    void groom(); // abstract
}
Dog extends Animal
{
    void feed() //implementation
    void groom() //implementation
}
Tiger extends Animal
{
    void feed() //implementation
    void groom(); //DUMMY implementation -
}
```

In the given exam ple we have added a function groom to our parent class Animal, this function will be used by the objects of Dog class but in order to escape the compilation error we have added the groom method to the Tiger class as well. This is a bad implementation according to  Interface Segregation Principle.

A better implementation would be,

```Java
Animal 
{
    void feed(); //abstract
    void groom(); // abstract
}
Pet extends Animal
{
    void groom(); //abstract
}
Dog extends Animal
{
    void feed() //implementation
    void groom() //implementation
}
Tiger extends Animal
{
    void feed() //implementation
}
```

In the above example we have added a new class Pet that only contains the groom method.
***
### 5.  Dependency Inversion Principle
<br>
> One should "depend upon abstraction, and not on concretions"

* Abstractions should not depend on the details whereas the details should depend on abstraction

* High level modules should not depend on low level modules

Let's take an example,

```Java
enum OutputDevice {printer, disk};
void copy(OutputDevice dev)
{
    int c;
    while ((c=ReadKeyboard()!=EOF))
    {
        if( dev == printer)
            writePrinter(c);
        else
            writeDisk(c);
    }
}
```

In the above example when number of Output devices will increase we will nweed to modify the function. So according to the Dependency Inversion Principle this is a wrong implementation.Creating seperate  interfaces will solve our problem.

A better example would be,

```Java
interface Reader
    char read();

interface Writer
    char write(char ch);

void copy(Reader r, Writer w)
{
    char ch;
    while((ch = r.read())!=EOF)
    {
        w.write(ch);
    }
}

```

## References

***

* https://youtu.be/yxf2spbpTSw
* https://youtu.be/HLFbeC78YlU
* https://stackify.com/solid-design-principles/
* https://www.digitalocean.com/community/conceptual_articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design