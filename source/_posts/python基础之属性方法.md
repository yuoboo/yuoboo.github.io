---
title: python基础之属性方法
date: 2018-06-07 23:58:29
tags: Python
categories: Python
---
[id1]: https://images.pexels.com/photos/256219/pexels-photo-256219.jpeg?auto=compress&cs=tinysrgb&h=650&w=940
[id2]: https://images.pexels.com/photos/39349/teens-robot-future-science-39349.jpeg?auto=compress&cs=tinysrgb&h=650&w=940
<!-- ![picture][id]   -->

###### 类属性和实例属性
  - 类属性是 *类对象* 所拥有的属性,它被所有的类对象的实例对象所共有,在内存中只存在一个副本
  - 对于公有的类属性在类外可以通过类对象和实例对象访问, 私有类属性不可以

```python
class People(object):
    address = '湖北'   #公有的类属性
    __country = 'china'    #私有的类属性

    def __init__(self):
        self.name = 'xiaowang'   #实例属性
        self.age = 20   #实例属性
p = People()
print(p.__country)        #错误，不能在类外通过实例对象访问私有的类属性
print(People.__country)   #错误，不能在类外通过类对象访问私有的类属性
p.age =12 #实例属性
print(p.name)    #正确  实例属性
print(p.age)     #正确  实例属性
print(People.address) #正确  类对象访问类属性
print(People.name)    #错误  类对象无法访问实例属性
p.address = '广东'    #新建同名实例属性, 并不会更改同名类属性, 实例属性会屏蔽掉同名的类属性
del p.address    #删除实例属性
People.address = '广东'  # 类对象才会修改雷属性
```
  - 如果需要在类外修改类属性，必须通过类对象去引用然后进行修改。如果通过实例对象去引用，会产生一个同名的实例属性，这种方式修改的是实例属性，不会影响到类属性，并且之后如果通过实例对象去引用该名称的属性，实例属性会强制屏蔽掉类属性，即引用的是实例属性，除非删除了该实例属性  


###### 类方法,实例方法和静态方法  
  - 类方法: 是类对象所拥有的方法，需要用修饰器`@classmethod`来标识其为类方法，对于类方法，第一个参数必须是类对象，一般以`cls`作为第一个参数（当然可以用其他名称的变量作为其第一个参数，但是大部分人都习惯以`cls`作为第一个参数的名字，就最好用`cls`了），能够通过 *实例对象和类对象* 去访问  
  - 类方法还有一个用途就是可以对类属性进行修改  

```python
class People(object):
    __country = 'china'   # 私有类属性在类外部无法访问和修改
    #类方法，用classmethod来进行修饰
    @classmethod
    def getCountry(cls):
        return cls.__country

    @classmethod
    def setCountry(cls,country):
        cls.__country = country

    p = People()
    print p.getCountry()    #可以用过实例对象引用
    print People.getCountry()    #可以通过类对象引用
    p.setCountry('japan')   
    print p.getCountry()   
    print People.getCountry()
```
  - 静态方法: 需要通过修饰器`@staticmethod`来进行修饰，静态方法不需要多定义参数

```python
class People(object):
    country = 'china'

    @staticmethod
    def getCountry():   #静态方法
        return People.country

    print People.getCountry()
```
  - 从类方法和实例方法以及静态方法的定义形式就可以看出来，类方法的第一个参数是类对象cls，那么通过cls引用的必定是类对象的属性和方法；而实例方法的第一个参数是实例对象self，那么通过self引用的可能是类属性、也有可能是实例属性（这个需要具体分析），不过在存在相同名称的类属性和实例属性的情况下，实例属性优先级更高。静态方法中不需要额外定义参数，因此在静态方法中引用类属性的话，必须通过类对象来引用
  - 两者最明显的区别是:类方法必须有一个类对象参数,静态方法不需要任务参数  


###### `__init__` 和 `__new__` 的区别:  
  - `__new__`在`__init__`之前执行  
  - `__new__`至少要有一个参数cls，代表要实例化的类，此参数在实例化时由Python解释器自动提供   
  - `__new__`必须要有返回值，返回实例化出来的实例，这点在自己实现`__new__`时要特别注意，可以`return`父类`__new__`出来的实例，或者直接是`object`的`__new__`出来的实例  
  - `__init__`有一个参数self，就是这个`__new__`返回的实例，`__init__`在`__new__`的基础上可以完成一些其它初始化的动作，`__init__`不需要返回值
  - `__new__` 来实现单例模式  

```python
class A(object):
  def __init__(self):
      print("这是 init 方法")

  def __new__(cls):
      print("这是 new 方法")
      return object.__new__(cls)
```
-  调用父类初始化方法的三种方式:

```python
class B(A):
    def __init__(self):
      # 调用父类的__init__方法1(python2)
      A.__init__(self)
      # 调用父类的__init__方法2
      super(B,self).__init__()
      # 调用父类的__init__方法3
      super().__init__()
```

###### 单例模式  
  - 单例:确保某一个类只有一个实例,而且自行实例化并向整个系统提供这个实例,这个类称为单例类,单例模式是一种对象创建型模式

```python
class Single(object):
    __instance = None
    __first_init = False

    def __new__(cls):
      if not __instance:
        cls.__instance = object.__new__(cls)
      return cls.__instance

    def __init__(self, name, age):
      if not __first_init:
        self.name = name
        self.age = age
        Single.__first_init = True
```
