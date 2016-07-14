# Model use

- 注意在类工厂方法里边`创建对象`的时候用`self`<br>

    ![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160518_25.png)

# `.h`
````objc

#import <Foundation/Foundation.h>

@interface LMJShop : NSObject
/** 模型的名字字符串 */
@property (nonatomic, copy) NSString *name;
/** 模型的图片数据,字符串 */
@property (nonatomic, copy) NSString *icon;

+ (instancetype)shopWithDictionary:(NSDictionary *)dictionary;

- (instancetype)initWithDictionary:(NSDictionary *)dictionary;
@end

````

# `.m`
````objc

#import "LMJShop.h"

@implementation LMJShop

- (instancetype)initWithDictionary:(NSDictionary *)dictionary
{
    if(self = [super init])
    {
    // 注意用的是这个方法,或者可以直接赋值
        [self setValuesForKeysWithDictionary:dictionary];
    }
    return self;
}

+ (instancetype)shopWithDictionary:(NSDictionary *)dictionary
{
    return [[self alloc] initWithDictionary:dictionary];
}
@end
````
