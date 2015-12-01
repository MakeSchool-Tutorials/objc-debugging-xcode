---
title: "Objective-C Primer"
slug: obj-c-primer
---     

This page is divided into two sections. The section What you need to know about Objective-C is a must read if you haven't coded in Objective-C before - it will explain the main idiosyncrasies of Objective-C.

The second part Cheat sheet is for you to refer back later so that you don't have to memorize all the syntactical details.

##What You Need To Know About Objective-C

**.h and .m files**
Objective-C separates the interface declaration and the implementation of a class in *.h* and *.m* files.

The *.h* files contains declarations of methods, properties and instance variables that are accessible by other classes.

The *.m* files contain the implementation of all methods declared in the *.h* files. Additionally it is possible to declare further properties, instance variables and methods in the *.m* file. These methods/properties/instance variables are called *private* because they can only be used within that *.m* file but not by any other class.

Conclusion: All functionality that shall be available to other classes needs to be placed in the *.h* file of a class, all implementations and all private functionality is placed in the *.m* file.

**@interface and @implementation**
Objective-C uses two further keywords to separate class interfaces and implementations: _@interface_ and _@implementation_. The scope of interface/implementation is defined by the keyword _@interface/@implementation_ followed by an _@end_.

Example of an interface block:

```
@interface Pet : NSObject

@property (nonatomic, strong) NSString *name;
@property (nonatomic, assign) BOOL canMakeNoise;
@property (nonatomic, strong) NSString *noise;

- (void)makeNoise:(NSInteger)times;
- (void)eat;

@end
```

By convention each Objective-C class has an _@interface_ block in it's _.h_ file. The _@interface_ block declares all publicly accessible methods, properties and instance variables.

Example of an implementation block:

```
@implementation Pet

- (void)makeNoise:(NSInteger)times {
    if (self.canMakeNoise) {
        for (int i=0; i < times; i++) {
            NSLog(@"%@:%@", self.name,self.noise);
        }
    } else {
        NSLog(@"%@ remains silent", self.name);
    }
}

- (void)eat {
    NSLog(@"%@ is eating.", self.name);
}

@end
```

By convention each Objective-C class has an _@implementation_ block in it's _.m_ file. The _@implementation_ block provides implementations for all methods declared in the _@interface_ block of the class.

**Properties and instance variables**
Objective-C provides two different ways to store information within objects: instance variables and Properties.

Instance variables should only be used to store objects/values that are exclusively used by the class itself. For any objects/values that should be accessible by other classes, a developer should use Properties.

Instance variables can be public or private depending on whether they have been declared as part of the _@interface_ block in the _.h_ file of a class or as part of the _@implementation_ block in the _.m_ file of a class. In most cases they should be private and declared as part of the _@implementatio_n__ block in the _.m_ file:

```
@implementation Pet {
    int _numberOfEars;
}
```

By convention leading underscores are used to name instance variables.

Properties are mostly used for publicly facing variables (however declaring private properties is possible as part of a _class extension_). By default Objective-C generates a getter a setter and an instance variable for every property you declare.

Here's an example of how a property can be defined in a _.h_ file of a class:

```
@interface Pet : NSObject

@property (nonatomic, strong) NSString *noise;

@end
```

This tells other classes, that the Pet class has a Property of type _NSString_ that can be accessed using _.noise_ (e.g. myCat.noise = @"meow"). Based on this property declaration Objective-C generates a getter method called _noise_, a setter method called _setNoise_ and a instance variable called *_noise*.

There are many different keywords that specify the behavior of properties, here are some important ones that define how memory management is handled for each property. For the time being, always use _nonatomic_ and _strong_. If the property is a primitive (int, BOOL, etc...) then you must also use _assign_.

##Cheat Sheet

**Initializing Objects**

The syntax is fairly straightforward:

```
MyObjectType *someName; //this declares the object
someName = [[MyObjectType alloc] init]; //this initializes it
//init can be replaced by any method in MyObjectType that returns itself

//Or, if you wanted to be compact about it
MyObjectType *someName = [[MyObjectType alloc] init];
```

**Method Syntax**
Methods in Objective C have a leading + or - when they are declared, + methods are class (static) methods and - ones instance methods. You typically will have all your methods be instance (-) methods. To declare a method:

```
-(typeOfWhatIWillReturn) firstPartOfMethodName:(typeOfFirstParameter)parameterName secondPartOfMethodName:(typeOfSecondParameter)secondParameterName
{
    //code here
}
```

For example:

```
-(BOOL) returnsTrueAndHasNoParameters
{
   return true;
}


-(void) doMeaninglessShufflingOfFirstNumber:(int)firstNumber andSecondNumber:(int)secondNumber
{
    int a = firstNumber;
    int b = a + secondNumber;
}
```

To call a method:

```
//general syntax without parameters
[myObject someMethod];

//general syntax with parameters
[myObject someOtherMethodWithFirstParameter:parameterOne andSecondParameter:parameterTwo];

//for example if I want to call a method in the class I'm in
BOOL myBoolean = [self returnsTrueAndHasNoParameters];

//or, if I wanted to do random number shuffling in some object I created
RandomObjectType *myRandomObject = [[RandomObjectType alloc] init];
[myRandomObject doMeaninglessShufflingOfFirstNumber:5 andSecondNumber:6];
```

**Classes**
To create a new class, ctrl-click on projectfiles and select "New File" and then select "Objective C Class", name it, and pick what it is a subclass of. Each class is divided into a .h and .m, referred to as the header and implementation files respectively.

The header file is a summary of what the class contains, effectively a table of contents. You want to declare all public properties and methods in the .h file. Here is a general format of a header file we'll call MyClass.h:

```
//If you are declaring an object (see below) in this .h file and it isn't a built-in Objective C (NSSomething), Cocos2D (CCSomething), or Kobold2D (KKSomething) class but rather a class you created, you'll have to tell the compiler it exists

@class anotherClassIWillUseHere;

//this is how you specify if your class is a subclass of an existing one.
//NOTE: you cannot use @class for the ClassFromWhichIInherit. You must import its .h file

#import ClassFromWhichIInherit.h

@interface NameOfMyClass : ClassFromWhichIInherit 

@property (nonatomic) NSArray* arr; //Objects need a * when you declare them
@property (nonatomic) anotherClassIWillUseHere *obj; //Because of the @class move we did above, we can use a class we created in another file

//this is where you declare your methods. IF YOU DON'T YOU MAY GET ERRORS WHEN YOU TRY CALLING A METHOD
-(BOOL) returnsTrueAndHasNoParameters; //don't forget the semicolon here
-(void) doMeaninglessShufflingOfFirstNumber:(int)firstNumber andSecondNumber:(int)secondNumber;
-(void) multiplyArrayValuesByFive:(NSArray*)arr;

@end //if you forget this, you'll get errors in your .m file! This will confuse you. So don't forget it. Or delete it. Ever.
```

To make a method public, declare it in the header. Any class that imports that header will be able to call that method on an object.

Now let's look at what an implementation file should look like. At the core, all our .m, MyClass.m, needs is the following:

```
#import "MyClass.h" //this should be done automatically when you create a new class
#import "anotherClassIWillUseHere.h" //with our @class line in the .h, we basically promised the compiler that we would import the relevant .h in our implementation file

@implementation MyClass
//all your methods have to be between @implementation and @end

//you'll need this in your classes. every bit of it
-(id) init
{
    if ((self = [super init]))
    {
        //initialize your class here

        obj = [[anotherClassIWillUseHere alloc] init]; //here we're initializing the object we declared in our header
    }
    return self;
}

@end
```

That should just about cover it! To summarize:

In the .h file, declare all public methods and properties of the class.

In the .m file, import any other classes you'll need, initialize the current class, implement all methods you declared in the header, and initialize all variables and properties as necessary. Careful - forgetting to initialize variables is a very common error.

**Implementing initializers in custom classes**
Once you add custom classes to your program you also need to provide custom initializers. The default initializer for Objective-C objects (subclasses of NSObject) is a method called *init*. If a developer wants to add a custom initializer for his class it has to follow a couple of conventions:

* Always invoke the superclass (super) initializer first
* Check if the super initializer returned nil and only proceed if that is not the case

An example initializer could look like this:

```
- (id)init {
    self = [super init];

    if (self) {
        self.name = @"Default Name";
    }

    return self;
}
```

It is also possible and very common to provide initializers with one or multiple parameters. All initializer methods need to begin with *init*.... One example could be an *initWithName* method:

```
- (id)initWithName:(NSString *)nameParam {
    self = [super init];

    if (self) {
        self.name = nameParam;
    }

    return self;
}
```

If you provide multiple initializers you should avoid duplicating the code within them. A common pattern is that the less specific initializers (the one that take fewer parameters) call the more specific ones with default values.

For this example you could chain the initializers as following:

```
- (id)init {
    self = [self initWithName:@"Default Name"];

    return self;
}

- (id)initWithName:(NSString *)nameParam {
    self = [super init];

    if (self) {
        self.name = nameParam;
    }

    return self;
}
```

The default *init* method calls the *initWithName* method with a default name.

**Class variables**
Technically Objective-C doesn't know the concept of class variables, however you can define *static* variables in the *.m* file of your class which will result in a behaviour very similar to class variables:

```
static NSInteger counter;

@implementation Human

...

@end
```

**Subclassing**
The superclass of an Objective-C object is defined as part of the _@interface_ block:

```
#import "LivingObject.h"

@interface Human : LivingObject
```

In order to create a subclass of an existing class the *.h* file needs to be imported. A subclass inherits all properties, methods and instance variables of its super class. However, subclasses can only see whatever is declared in the *.h* file of the superclass - this means it isn't possible to override private methods.

In the subclass one can access method implementations of the superclass by using the *super* keyword:

```
- (void)setPetName:(NSString *)petName {
    [super setPetName:petName];

    NSLog(@"Additional Behaviour here");
}
```

**Private properties and methods**
Private properties and methods can be declared in *Class extensions*. Class extensions should be part of the *.m* file and look as follows:

```
@interface Pet ()

@property (nonatomic, strong) NSString *privateName;

- (void)privateMethod;

@end

@implementation Pet

...

@end
```

_@interface_ followed by empty parentheses indicate a class extension. Since this class extension is part of the _.m_ file all methods and properties declared here are not visible to other classes.

**Printing to the console**
Use the line:

```
NSLog(@"print this");
```

To print an int:

```
int k = 3;
NSLog(@"Int k is %i", k);
```

To print a float:

```
float k = 1.583;
NSLog(@"Float k is %f", k);
```

To print a string:

```
NSString* s = @"world";
NSLog(@"Hello %@", s);
```
