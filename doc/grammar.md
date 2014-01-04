## Objective-C学习之旅

***

### Objective-C程序结构

#### 结构
1. 类接口 => 定义类的数据和方法， 放在.h文件中
2. 类实现 => 实现类方法的代码， 放在.m文件中
3. 应用程序 => 调用类完成一些实际操作

#### 一个示例
* 类接口

``` objective-c
#import <Foundation/Foundation.h>

//注意继承机制
@interface Member : NSObject {
    //变量定义
    NSString* name;
    int age;
}

//方法定义

// 减号-实例方法；加号-类方法
// 返回类型用括号
-(NSString*) name;
-(int) age;
//以上为取值方法，没有get

// 形式参数名input，注意前加冒号
// 参数类型用括号
-(void) setName:(NSString*)input;
-(void) setAge:(int)input;

@end

```

* 类实现

``` objective-c
@implementation Member
-(NString*) name {
    return name；
}

-(int) age {
    return age;
}

-(void) setName:(NSString*)input {
    // 应用对象要及时释放
    // autorelease-会在将来某时删除引用
    // release-直接删除引用
    [name autorelease];
    name = [input retain];
    
    //OC有release/autorelease/retain机制
}

-(void) setAge:(int)input {
    agee = input;
}

@end
```

* 应用程序
main方法是整个应用程序的入口，首先被调用。

``` objective-c
int main (int argc, const char* argv[]){
    //设置自动释放池
    NSAutoreleasePool* pool = [[NSAutoreleasePool alloc] init];
    
    // 构造一个Member对象，并调用它的方法
    
    //OC中对象变量都是指针类型
    //左边只是定义一个对象，没有分配空间
    Member* member = [[Member alloc]init];
    //嵌套的方法调用
    //调用Member的alloc方法，为Membe类对象变量申请内存空间
    //抵用新创建对象的init的方法，初始化变量值
    
    //方法调用-对象+方法名
    [member setName:@"dadashazhu"];
    [member setAge:2];
    
    //打印用NSLog
    // %@ 用于字符串；%i用于整型
    NSLog(@"%@", [member name]);
    NSLog(@"%i", [member age]);
    
    //释放member对象的内存空间
    [member release]
    //清空池
    [pool drain]
    //返回0
    return 0；
    
}
```

* 输入输出
    使用NSLog输出
    使用scanf输入

``` objective-c
#import <Foundation/Foundation.h>
int main( int argc, const char * argv[] ) {
    NSAutoreleasePool * pool = [[NSAutoreleasePool alloc] init];
    
    int n;
    NSLog(@"请输入一个整数: ");
    scanf("%i", &n);
    NSLog(@"%i", n);
    
    [pool drain]
    return 0;
}


```

* 标识符
    1. 区分大小写
    2. @开头 => 指令符
    @“char" => 字符串常量
    
***

### 数据类型

#### 基本数据类型

1. 整型
    * 短整型 short int / short (2B)
    * 整型 int(4B)
    * 长整形 long int / long (4B)
    * 无符号型 unsigned
        * unsigned short 
        * unsigned int / unsigned
        * unsigned long 
    * 实例
    ` int x,y;`
    ` NSLog => %i `
    

2. 实型

    ######常量
+ 浮点数，两种形式：小数形式和指数形式
+ 小数=> `3.14 , NSLog=> %f`
+ 指数=> `2.1E2=2.1*100 NSLog=> %e`
        `NSLog=> %g` => 最短的方式表示一个浮点数
        
    ######变量
+ 单精度 float 4B 
+ 双精度 double 8B
+ 长双精度 long double 16B


3. 字符型
    ######常量
    * 单引号 括起来 一个字符 `'c'`
    * 不能用双引号
    * 任一字符都可用转义字符来表示，如`\101=A`,`\102=B`
    
    ######变量
    * 字符变量 => 用来存储字符常量，即单个字符
    * char ` char a = 'A' ` `NSLog => %c`
    * 可以把字符型当做整型，反之不能；字符型只有8个位

    ###### 一个例子
    ``` objective-c
    import <Foundation/Foundation.h>
    int main( int argc, const char * argv[]){
        @autoreleasepool {
            char a = 120;
            char b = 121;
            
            NSLog(@"%c,%c",a,b);
            NSLog(@"%i,%i",a,b);
            
            /******************/
            /* 
                x,y
               120,121
            */
            /******************/
            
            char c = 'a';
            char d = 'b';
        
            a = a - 32;
            b = b - 32;
            
            NSLog(@"%c,%c", c,d);
            NSLog(@"%i,%i", c,d);
            
            /******************/
            /*
                A,B
                65,66
            */
            /******************/
        }
    }
    ```


4. 字符串

    * 由`@`和一对双引号括起来的字符序列组成
    * 例子：`@CHINA`, `@program`
    * OC中的字符串，不能作为字符的数组被实现
    * 字符串的类型为NSString，它不是一个简单数据类型，而是一个对象类型

5. id类型

    * 一个特殊的类型，类似Java的Object类型，可被转换为任何类型
    * id类型的变量可以存放任何数据类型的对象
    * 一个使用示例
    ``` objective-c
    
    ```

6. 类型转换

7. 枚举类型

8. typedef

#### 其他数据类型

#### 运算符和表达式




### 循环语句

if
switch

三目

布尔

循环

跳转



### 一些其他语法

### 类

