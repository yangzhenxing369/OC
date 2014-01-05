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

    
4. id类型 

    * 一个特殊的类型，类似Java的Object类型，可被转换为任何类型
    * id类型的变量可以存放任何数据类型的对象
    * 一个使用示例
    
    ``` objective-c
        id anObject;
        - (id) newObject: (int) type;
    ```
    
    * id的实质
        在 objc.h 文件中定义
    
    ``` objective-c
    typedef struct objc_class *Class;
    typedef struct objc_object{
        Class isa;
    } *id;
    ```
    在NSObject.h中，NSObject类的定义
    
    ``` objective-c
    @interface NSObject <NSObject>{
        Class isa;
    }
    + (void) load;
    + (void) initialize;
    - (void) int;
    
    + (void) new;
    + (id)allc;
    - (void)dealloc;
    
    //很多其他方法，这里省了。。
    
    @end
    ```
    
    * id是一个指向**struct objc_object** 的一个指针。即id是一个指向任何一个继承了Object/NSObject类的对象。
    * id和NSObject* 都是**指针**，都含有Class isa，但NSObject同时包含一些其他的方法，并需要实现**NSObject协议**。
    * NSObject* 可以由id来表示，但id不能用NSObject*来表示。
    * id是一个指针，不需要星号 `id foo = nil `。
    * id类型是OC默认的数据类型（C语言默认的数据类型是int)。
    * 关键字**nil** 被定义为空对象，也是值为0的**对象**。
    
    Student.h    
    ``` objective-c
    
    #import <Foundation/Foundation.h>
    @interface Student : NSObject {
        int sid;
        NSString *name;
    }
    
    @property int sid;
    @property (nonatomic,retain) NSString *name;
    
    - (void) print;
    - (void) setSid: (int)sid andName: (NSString*) name;
    
    @end
    ```
    
    Student.m
    
    ``` objective-c
    #import "Student.h"
    
    @implementation Student
    @synthesize sid, name;
    
    -(void) print{
        NSLog(@"我的学号：%i, 我的名字是：%@", sid, name);
    }
    
    -(void) setSid:(int) sid1 andName: (String*) name1 {
        self.sid = sid1;
        self.name = name;
    }
    ```
    
    测试文件
    
    ``` objective-c
    #import <Foundation/Foundation.h>
    #import "Student.h"
    
    int main(int argc, const char* argv[]){
        @autoreleasepool{
            id data;
            Student* stu = [[Student alloc] init];
            data = stu;
            [data print];
        }
    }
    ```
    
5. 字符串

    * 由`@`和一对双引号括起来的字符序列组成。
    * 例子：`@CHINA`, `@program`。
    * OC中的字符串，不能作为字符的数组被实现。
    * 字符串的类型为NSString，不是一个简单数据类型而是一个对象类型。
    
    
6. 类型转换


7. 枚举类型

    * 一个变量只有几个固定值时
    * 定义 `enum sex {male, female}`;
    * 使用 `enum sex student, teacher;`
    * 复制 `student = male;`

8. typedef

    * 为 **数据类型** 指派另一个名字

#### 其他数据类型

##### BOOL

##### SEL

##### Class

##### nil & Nil

#### 运算符和表达式


***

### 循环语句

#### 条件语句

#### 循环语句
##### while语句
    先判断，后执行
``` 
    while(表达式)
        语句
```

```
    int count = 1;
    while (count <= 4){
        NSLog(@"%i", count);
        ++count;
    }
```

##### do-while语句
    先执行，后判断
```
    do 
        语句
    while(表达式)
```
##### for语句
```
    for(循环变量 赋初值; 循环条件; 循环变量增量)
        语句;
```

```
    for(i=1 ; i <= 100; i++)
        sum = sum + 1;
```

#### 跳转语句
* `break;` => 用在switch语句中 以跳出switch执行该结构以后之后；用在循环语句中，跳转循环；

* `continue;` => 只跳出本次循环。

* `return;` 

***

### 一些其他语法

*** 

### 类

#### 协议--protocol
* 用来实现 **多继承** 功能，多个类共享方法
* 遵守协议，该类就必须实现所有方法
* 定义

    ``` objective-c
    @protocol NSCopying
    -(id) copyWithZone:(NSZone* ) zone;
    @end
    ```
* 使用

    ``` objective-c
    @interface Test: NSObject <NSCopying, NSCoding>
    //类Test 继承NSObject，遵守NSCopying协议
    @end
    ```
* 检查一个对象是否遵守特定协议

    ``` objective-c
    if( [someObject conformsToProtocol:@protocol (Fly)] == YES) {
        // 一些操作
    }
    ```

* `id <Fly> someObject` => 这个对象是遵守Fly协议的对象

* 拓展现有协议（协议实现协议）=> `@protocal Fly1 <Fly>`

##### 一个示例

*  定义协议
    
    ``` objective-c
    @protocol Fly
    - (void) go;
    - (void) stop;
    
    //遵守协议的类，可以不必实现
    @optional
    - (void) sleep;
    
    @end
    ```

* 定义一个类，遵守Fly协议

    ``` objective-c
    #import <Foundation/Foundation.h>
    #import "Fly.h"
    
    @interface FlyTest : NSObject<Fly>{
    }
    
    @end
    ```
* 实现类

    ``` objective-c
    # import "FlyTest.h"
    @implementation FlyTest
    - (void) go{
        NSLog(@"go");
    }
    
    - (void) stop{
        NSLog(@"stop");
    }
    @end
    ```

***

####







