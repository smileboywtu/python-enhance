# 面向对象的编程思想

面向对象编程——Object Oriented Programming，简称OOP，是一种程序设计思想。OOP把对象作为程序的基本单元，一个对象包含了数据和操作数据的函数。

面向过程的程序设计把计算机程序视为一系列的命令集合，即一组函数的顺序执行。为了简化程序设计，面向过程把函数继续切分为子函数，即把大块函数通过切割成小块函数来降低系统的复杂度。

而面向对象的程序设计把计算机程序视为一组对象的集合，而每个对象都可以接收其他对象发过来的消息，并处理这些消息，计算机程序的执行就是一系列消息在各个对象之间传递。

## 面向过程示例

``` c

#include "stdio.h"

struct Student {
    char* name;
    int age;
    char* address;
}

char* get_name(struct Student someone) {
    return someone.name
}

int get_age(struct Student someone) {
    return someone.age
}

char* get_address(struct Student someone) {
    return someone.address
}

// show student
void main(void) {
    struct Student someone = {"John", 32, "sun road 114"};
    printf("name: %s\n", get_name(someone))
    printf("age: %d\n", get_age(someone))
    printf("address: %s\n", get_address(someone))
}

```
通过定义结构体，创建包含有对象属性的集合，然后通过额外的方法体来操作集合，方法体和集合之间存在若关系对。

## 面向对象示例

``` python

class Student:
    """show Student information"""

    def __init__(self, name, age, address):
        """init the student information"""
        self.name = name
        self.age = age
        self.address = address

    def get_name(self):
        return self.name

    def get_age(self):
        return self.age

    def get_address(self):
        return self.address

if __name__ == "__main__":
    someone = Student("John", 32, "sun road 114")
    print "name: ", someone.get_name()
    print "age: ", someone.get_age()
    print "address: ", someone.get_address
```
面向对象的编程过程中，对象的方法和属性封装在同一个集合中，不像面向过程那么的松散，方法和属性的关系更加紧密。

## 面向对象和面向过程对比

`面向过程`是一种以事件为中心的编程思想。就是分析出解决问题所需要的步骤，然后用函数把这些步骤一步步实现，使用的时候一个个依次调用。

`面向对象`是一种以事物为中心的编程思想。将数据和对数据的操作放在一起，作为一个相互依存的整体，就是所谓的对象。对同类对象抽象出其共性，就是类，类中的大多数数据只能被本类的方法进行处理。类通过一个简单的外部接口与外界发生关系，对象与对象之间通过消息进行通信。

面向过程其实是最为实际的一种思考方式，就是面向对象的方法也是含有面向过程的思想。可以说面向过程是一种基础的思想，它考虑的是实际的实现。一般的面向过程是从上往下步步求精，所以面向过程最重要的是模块化的思想。对比面向对象，面向对象的方法主要是把事物给对象化，对象包括属性和行为。当程序规模不是很大时，面向过程的方法还会体现出一种优势，因为程序的流程很清楚，按着模块与函数的方法可以很好的组织。

# 面向对象的特点

Python是一门面向对象的语言，其具备面向对象的封装，继承，多态。

## 什么是封装？
封装，也就是把客观事物封装成抽象的类，并且类可以把自己的数据和方法只让可信的类或者对象操作，对不可信的进行信息隐藏。

``` python

class Person(object):
    """define a person class"""

    def __init__(self, name, age, salary):
        """init the Person"""
        self. name = name
        self. age = age
        self.__salary = salary

    def get_name(self):
        return self.name

    def get_salary(self):
        return self.__show_salary()

    def __show_salary(self):
        return self.__salary * 0.8
```

这个地方我们定义了一个人，我们指定了姓名，年龄和薪水，我们可以通过`get_name`获取这个人的名字，但是我们并不能通过`get_salary`或者`someone.salary`获得这个人的薪水，因为薪水都是保密的，每个人并不想把薪水告诉其他人，这个时候`__salary`就是人这个类的私有属性，当然也可以拥有私有方法类似`__show_salary`。在Python中私有方法和属性是通过两个下划线开始的。

## 什么是继承？

继承是从一般到特殊的过程，一个类继承另一个类，继承后的类拥有所有基础类的特性，但是有些基础类私有的方法，继承类也是不可见的。在这个过程中还可以出现组合的形式：

``` 
A -> B

A --->
       \
         C
       /
B --->

```

继承可能发生在多个类之间，产生比较复杂的机制。但是类继承讲究的是复用、扩展、特化：

``` python

class Animal(object):

    def walk(self):
        return "walk on road"

    def run(self):
        pass

    def howl(self):
        pass

class Monkey(Animal):

    def run(self):
        return "crawl on the branch"

    def howl(self):
        return "bray"

    def eat(self):
        return "eat banana"
```
这个地方`Monkey`特化了`Animal`并且新增了`eat`方法，Monkey是一种动物（Animal），他继承了动物的基本的特性`walk`。

## 什么是多态？

多态是在继承的基础上的，继承过程中，我们不仅可以继承父类的特性，我们也可以覆盖父类的方法，实现子类特有的方法，这样反过来我们可以通过父类作为容器承载不同的子类对象，调用不同子类的同一个方法。

```python

class Dog(Animal):
    def run(self):
        print 'Dog is running...'

class Cat(Animal):
    def run(self):
        print 'Cat is running...'

def run_twice(animal):
    animal.run()
    animal.run()

>>> run_twice(Animal())
Animal is running...
Animal is running...

>>> run_twice(Cat())
Cat is running...
Cat is running...

class Tortoise(Animal):
    def run(self):
        print 'Tortoise is running slowly...'

>>> run_twice(Tortoise())
Tortoise is running slowly...
Tortoise is running slowly...

```

你会发现，新增一个Animal的子类，不必对run_twice()做任何修改，实际上，任何依赖Animal作为参数的函数或者方法都可以不加修改地正常运行，原因就在于多态。

多态的好处就是，当我们需要传入Dog、Cat、Tortoise……时，我们只需要接收Animal类型就可以了，因为Dog、Cat、Tortoise……都是Animal类型，然后，按照Animal类型进行操作即可。由于Animal类型有run()方法，因此，传入的任意类型，只要是Animal类或者子类，就会自动调用实际类型的run()方法，这就是多态的意思：

对于一个变量，我们只需要知道它是Animal类型，无需确切地知道它的子类型，就可以放心地调用run()方法，而具体调用的run()方法是作用在Animal、Dog、Cat还是Tortoise对象上，由运行时该对象的确切类型决定，这就是多态真正的威力：调用方只管调用，不管细节，而当我们新增一种Animal的子类时，只要确保run()方法编写正确，不用管原来的代码是如何调用的。这就是著名的“开闭”原则：
- 对扩展开放：允许新增Animal子类；
- 对修改封闭：不需要修改依赖Animal类型的run_twice()等函数。


## 总结

封装可以隐藏实现细节，使得代码模块化；继承可以扩展已存在的代码模块（类）；它们的目的都是为了——代码重用。而多态则是为了实现另一个目的——接口重用！多态的作用，就是为了类在继承和派生的时候，保证使用“家谱”中任一类的实例的某一属性时的正确调用。

