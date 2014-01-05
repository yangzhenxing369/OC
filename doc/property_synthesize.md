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


### @property & @synthesize
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


> Written with [StackEdit](https://stackedit.io/).