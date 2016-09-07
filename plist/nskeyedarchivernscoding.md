# NSKeyedArchiver(NSCoding)

- 归档存储
    - 自定义对象

```objc
#import <Foundation/Foundation.h>

// 如果一个自定义对象想要归档，必须遵守NSCoding协议，实现协议方法。
@interface Person : NSObject<NSCoding>
@property (nonatomic, assign) int age;
@property (nonatomic, strong) NSString* name;
@end


// 什么时候调用：自定义对象归档的时候

// 作用：用来描述当前对象里面的哪些属性需要归档
- (void)encodeWithCoder:(NSCoder *)aCoder
{
    // name
    [aCoder encodeObject:_name forKey:@"name"];

    // age
    [aCoder encodeInt:_age forKey:@"age"];

}


// 什么时候调用:解档对象的时候调用

// 作用：用来描述当前对象里面的哪些属性需要解档
// initWithCoder：就是用来解析文件的。
- (id)initWithCoder:(NSCoder *)aDecoder
{
    // super:NSObject
#warning 什么时候需要调用initWithCoder
    if (self = [super init]) {

        // 注意：一定要给成员变量赋值
        // name
       _name = [aDecoder decodeObjectForKey:@"name"];

        // age
       _age = [aDecoder decodeIntForKey:@"age"];

    }
    return self;

}

```


```objc

@implementation ViewController
- (IBAction)save:(id)sender {

    Person *p = [[Person alloc] init];
    p.age = 18;

    // 获取cache
    NSString *cachePath = NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES)[0];

    // 获取文件的全路径
    NSString *filePath = [cachePath stringByAppendingPathComponent:@"person.data"];

    // 把自定义对象归档
    [NSKeyedArchiver archiveRootObject:p toFile:filePath];

}
- (IBAction)read:(id)sender {

    // 获取cache
    NSString *cachePath = NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES)[0];

    // 获取文件的全路径
    NSString *filePath = [cachePath stringByAppendingPathComponent:@"person.data"];

    // 解档
    Person *p = [NSKeyedUnarchiver unarchiveObjectWithFile:filePath];

    NSLog(@"%d",p.age);

}

```

![
](../LibrarypPictures/Snip20160601_13.png)
