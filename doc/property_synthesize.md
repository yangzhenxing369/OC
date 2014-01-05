## Objective-C 之 @property和@synthesize

***

### 引言
在Java中，特别是一个标准的POJO类，我们定义了一些属性，然后针对每个属性生成相应的getter和setter.

例如：

``` java
package com.demo;
/**
 * 手机类
 */
public class Phone {
 
    private String color;  //颜色
    private String os;     //系统
    private String brand;  //品牌
 
    /******* Getter & Setter *******/
    public String getColor() {
        return color;
    }
    public void setColor(String color) {
        this.color = color;
    }
    public String getOs() {
        return os;
    }
    public void setOs(String os) {
        this.os = os;
    }
    public String getBrand() {
        return brand;
    }
    public void setBrand(String brand) {
        this.brand = brand;
    }
 }  
```

### OC--property
在Objective-C中, **类的属性默认是protected的**，即只有该类以及它的子类才能存取它，**如果要给外部使用的话，则需要来帮它加个setter/getter**。

使用Objective-C将上面的代码改写为：

``` objective-c
@implementation Phone{
    NSString* color;
    NSString* brand;
    NSString* os;
}
-(NSString*) color{
    return color;
}
-(NSString*) brand{
    return brand;
}
-(NSString*) os{
    return os;
}
-(void) setColor:(NSString*) _color{
    color=_color;
}
-(void) setBrand:(NSString*) _brand{
    brand=_brand;
 
}
-(void) setOs:(NSString*) _os{
    os=_os;
}
@end
```
`在Objective-C中get有着特殊的含义，所以getter方法直接使用属性名，而不是使用get然后再加上属性名。`


### @property & @synthesize 使用示例
* 上面仅仅只是三个属性，如果有10个、20个属性，是不是也需要写大量的代码呢
* Objective C 2.0 为我们提供了@property和@synthesize。简化了我们创建数据成员读写函数的过程，更为关键的是它提供了一种更为简洁，易于理解的方式来访问数据成员。

上面的代码我们可以简化为：

Phone.h

``` objective-c
@interface Phone : NSObject
 
@property NSString* color;
@property NSString* brand;
@property NSString* os;
 
@end
```

Phone.m

``` objective-c
#import "Phone.h"
 
@implementation Phone
 
@synthesize color;
@synthesize brand;
@synthesize os;
 
@end
```
### @property 
@property的语法：
`@property(attribute1, attribute2)type name`

* 其中attribute有如下几种取值,各个attribute的含义涉及到Objective-C中内存管理

* attribute主要分为三类：
    * 读写属性： （readwrite/readonly） 决定是否生成set访问器
    > readwrite:生成setter\getter方法 (默认)
    > readonly:只生成getter方法.
        * 此标记说明属性是只读的，如果你指定了readonly，在@implementation中只需要一个getter。或者如果你使用@synthesize关键字，也只会生成getter方法。如果你试图使用点操作符为属性赋值，你将得到一个编译错误。
        * readonly关键字代表setter不会被生成， 所以它不可以和 copy/retain/assign组合使用。
    
    * setter语意 （assign/retain/copy）set访问器的语义，决定以何种方式对数据成员赋予新值。
        > assign:简单赋值，**不更改索引计数**。这也是默认值。
    
        > retain:释放旧的对象，将旧对象的值赋予输入对象，再增加输入对象的索引计数为1。此属性只能用于Objective-C对象类型，而不能用于CoreFoundation对象。(原因很明显，retain会增加对象的引用计数，而基本数据类型或者Core Foundation对象都没有引用计数)。
    
        > copy:建立一个索引计数为1的对象，然后释放旧对象,它指出在赋值时使用传入值的一份拷贝。拷贝工作由copy方法执行，此属性只对那些实行了NSCopying协议的对象类型有效。
    
    * 原子性：  （atomic/nonatomic)
    > atomic/nonatomic:指出访问器不是原子操作，atomic表示属性是原子的,支持多线程并发访问,而默认是nonatomic。
    
    * 在iOS5引入了自动引用计算(ARC)之后,对象变量属性新增了strong和weak,strong与retain作用类似，可以说是用来代替retain.

***

###retain和copy还有assign的区别 

#### 问题
    假设你用malloc分配了一块内存，并且把它的地址赋值给了指针a，后来你希望指针b也共享这块内存，于是你又把a赋值给（assign）了b。此时a和b指向同一块内存，
    
    请问当a不再需要这块内存，能否直接释放它？答案是否定的，因为a并不知道b是否还在使用这块内存，如果a释放了，那么b在使用这块内存的时候会引起程序crash掉。 


#### 解决 
    了解到1中assign的问题，那么如何解决？
    
    最简单的一个方法就是使用引用计数（referencecounting），还是上面的那个例子，我们给那块内存设一个引用计数，当内存被分配并且赋值给a时，引用计数是1。
    
    当把a赋值给b时引用计数增加到2。这时如果a不再使用这块内存，它只需要把引用计数减1，表明自己不再拥有这块内存。b不再使用这块内存时也把引用计数减1。
    
    当引用计数变为0的时候，代表该内存不再被任何指针所引用，系统可以把它直接释放掉。 


#### 区分
    上面两点其实就是assign和retain的区别，assign就是直接赋值，从而可能引起上面问题，当数据为int,float等原生类型时，可以使用assign。
    
    retain就如所述，使用了引用计数，retain引起引用计数加1,release引起引用计数减1，当引用计数为0时，dealloc函数被调用，内存被回收。
    
    copy是在你不希望a和b共享一块内存时会使用到。a和b各自有自己的内存。 


``` objective-c
if (property != newValue) {   
    [property release];   
    property = [newValue retain];   
}  
```

#### 原子性 
atomic和nonatomic用来决定编译器生成的getter和setter是否为原子操作。在多线程环境下，原子操作是必要的，否则有可能引起错误的结果。

***

### 访问修饰符
    Objective-C提供了3条指令来控制对一个对象的实例变量的访问：

* **@protected**: 用此指令修饰的实例变量可以被该类和任何子类定的方法直接访问，这是**默认**情况。
* **@private**：用此指令修饰的实例变量可以被定义在该类的方法直接访问，但是不能被子类中定义的方法直接访问。
* **@public**：用此指令修饰的实例变量可以被该类中的方法直接访问，也可以被其它类定义的方法直接访问。
* **@package** 关键字是在Mac 10.5 Objective-C runtime中新添加的，用以支持64-bit系统。

