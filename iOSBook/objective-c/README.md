# `Objective-C` `总结`
---

```objc


1.<NSString>
======================
基本概念
======
//通过字符串常量创建的对象,储存在常量区,如果内容一样则地址一样
//常量区的字符串引用计数器为垃圾值
NSString *str = @"lnj";
NSString *str2 = @"lnj";

//通过alloc/init创建 或者通过类工厂方法创建的字符串//注释:类工厂内部封装了alloc/init
//储存在堆区,并且相同内容的字符串,地址一样.
NSString *str = [[NSString alloc] initWithFormat:@"lmj"];
NSString *str2 = [[NSString alloc] initWithFormat:@"lmj"];
NSString *str3 = [NSString stringWithFormat:@"lmj"];

//注意initWithString方法是浅拷贝,返回字符串地址给我们
// 一般不这样用字符串
NSString *str = [[NSString alloc] initWithString:@"nj"];

/*
 如果是在Xcode6以下,并且是在iOS平台,每次alloc的空间都不一样,虽然字符串内容一样,但是地址也不一样
 */
=======================
字符串文件读写
======
//读取文件内容,error保存的是错误信息,读取错误或者文件不存在
NSString *path = @"/Users/apple/Desktop/a.txt";
NSError *error = nil;
NSString *str = [NSString stringWithContentsOfFile:path encoding:NSUTF8StringEncoding error:&error];
//获得简要的描述信息
NSLog(@"%@", [error localizedDescription]);

//写入文件
//atomically:如果是YES那么如果写入失败不会生成新的文件,如果是NO,写入失败也会生成新的文件
BOOL flag = [str writeToFile:path atomically:YES encoding:NSUTF8StringEncoding error:&error];

//NSURL
//URL组成,协议头 + 主机地址 + 文件路径
//如果加载的是本机的资源,那么主机地址可以省略,但是/绝对路径符号不能省略
NSString *path0 = @"file://192.168.199.199/Users/NJ-Lee/Desktop/lnj.txt";
NSString *path1 = @"file:///Users/apple/Desktop/a.plist";
//如果用这种方式创建URL,当路径中包含中文的时候,可能出现错误
NSURL *url1 = [NSURL URLWithString:path1];

//为了避免出现错误,用这种方式创建出来的URL,自动添加URL协议头
//并且路径中如果有中文也可以解决编码问题
NSString *path2 = @"/Users/apple/Desktop/a.plist";
NSURL *url2 = [NSURL fileURLWithPath:path2];

// URLWithString中文路径编码破解,=============     @    ======没记住
path1 = [path1 stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];

//读取文件内容
NSString *str = [NSString stringWithContentsOfURL:path2 encoding:NSUTF8StringEncoding error:nil];

//写入文件
BOOL flag = [对象 writeToURL:url2 atomically:YES encoding:NSUTF8StringEncoding error:&error];

=======================
字符串比较
======
BOOL flag = [str1 isEqualToString:str2];

if(str1 == str2)

//不忽略大小写
[str1 compare:str2]
//忽略大小写比较
// 返回值是NSOrderedAscending:升序
// NSOrderedSame:
//NSOrderedDescending:降序
[str1 caseInsensitiveCompare:str2]

=======================
字符串查找
======

BOOL flag = [str hasPrefix:@"http://"];
[str hasSuffix:@"jpg"];
NSRange range = [str rangeOfString:@"search"];
range.location = NSNotFound;

=======================
字符串截取
======
NSRange range = [str rangeOfString:@"search" options:NSBackwardsSearch];
// substring  是小写
[str substringWithRange:range1];
[str substringToIndex:range.location]; // 不包括
[str substringFromIndex:range.location]; // 包括

=======================
字符串替换
======
// 这个方法会生成新的字符串
NSString *newStr = [str stringByReplacingOccurrencesOfString:@"&&" withString:@"//"];

NSCharacterSet *set = [NSCharacterSet whitespaceCharacterSet];
NSCharacterSet *set1 = [NSCharacterSet uppercaseLetterCharacterSet];

NSString *newStr = [str stringByTrimmingCharactersInSet:set];

=======================
字符串与路径
======
//绝对路径判断
BOOL flag = [str isAbsolutePath];
//获取文件中最后一个目录
NSString *newStr = [str lastPathComponent];
//删除最后一个目录
NSString *newStr = [str stringByDeletingLastPathComponent];
// 添加目录,智能添加最后一个"/"
NSString *newStr = [str stringByAppendingPathComponent:@"addcomponent"];
// 获取,删除,添加扩展名
NSString *newStr = [str pathExtension];
NSString *newStr = [str stringByDeletingPathExtension];
NSString *newStr = [str stringByAppendingPathExtension:@"jpg"];

=======================
字符串的转换
======
// 大小写转换
NSString *newStr = [str uppercaseString];
NSString *newStr = [str lowercaseString];
//首字母大写
NSString *newStr = [str capitalizedString];

NSString *a1 = @"110";
int a = [a1 intValue];
[double1 doubleValue];

// c and oc字符串转换
char *cs = "abcdefg";
NSString *str = @"lkjh";

const char *strS = [str UTF8String];
NSString *csStr = [NSString stringWithUTF8String:cs];

===========================================================================
2. <NSMutableString>
=======================
NSMutableString 基本
======
NSMutableString *strM = [NSMutableString string]
| [NSMutableString alloc] init];
| [NSMutableString stringWithFormat:@"adfs"]
| [[NSMutableString alloc] initWithFormat:@"asdf"];

[strM appendString:@"addstring"];
[strM appendFormat:@"asdf,%d", 10];

//删除
NSRange range = [strM rangeOfString:@"520"];
[strM deleteCharactersInRange:range];

// 插入
[strM insertString:@"250" atIndex:range.location];

//stringByReplacingOccurrencesOfString: withString:会生成新的字符串
NSString *newStr = [strM stringByReplacingOccurrencesOfString:@"520" withString:@"250"];

// replaceOccurrencesOfString: withString: options: range: 只会替换,不会生成
[strM replaceOccurrencesOfString:@"520" withString:@"250" options:0 range:NSMakeRange:(0, strM.length)]; // 不包括最后一个

// 替换指定范围的字符
newStr = [newStr stringByReplacingCharactersInRange:range withString:@"ab"];
| [newStr stringByTrimmingCharactersInSet:[NSCharacterSet whitespaceCharacterSet]];


===========================================================================
3. <NSArray>
=======================
=======================
NSArray 基本
======
NSArray *arr = [NSArray arrayWithObject:@"one"];
| [NSArray arrayWithObjects:@"noe", @"teo", @"thee", nil];
int count = [arr count] | arr.count;

NSObiect *obj = [arr firstObject] | [arr lastObject] | [arr objectAtIndex:1];

if([arr containsObject:@"isExist"]) // 判断是否含有这个对象

NSArray *array = @[@"aafs",@"asd", @"adfs", @"adfs"];
getter方法  array[0]. array[1], array[2];

=======================
NSArray 遍历
======
Person *p = [Person new]; - (void)makeObject:(id)obj doSelector:(SEL)sel;
- (void)say;
- (void)sayWithName:(NSString *)name;

for(id obj in arr)
{
    NSLog(@"%@", obj);
}

[arr enumerateObjectsUsingBlock:^(id obj, NSUInteger idx, BOOL *stop)
 {
     NSLog(@"%lu==%@", idx, obj);
 }];

NSArray *arr = @[p1, p2, p3, p4];
// 遍历数组,让数组的对象执行方法
[arr enumerateObjectsUsingBlock:^(Person *obj, NSUInteger idx, BOOL *stop)
 {
     [obj say];
 }];
[arr makeObjectsPerformSelector:@selector(say)];
[arr makeObjectsPerformSelector:@selector(sayWithName:) withObject:@"lnj"];

/** 补充 SEL 的用法*/
[p1 makeObject:car doSelector:@selector(run)]; // 作为参数传递
=======================
NSArray 排序
======

NSArray *arr = @[@10, @5, @7, @3];
//Foundation中的对象可以种这个@selector(compare:)这个方法
NSArray *newArr = [arr sortedArrayUsingSelector:@selector(compare:)];

// 自定义的对象要使用comparator
NSArray *arr = @[p1, p2, p3, p4];
NSArray *newArr = [arr sortedArrayWithOptions:NSSortStable usingComparator:^NSComparisonResult(Person *obj1, Person *obj2) {
    return obj1.age > obj2.age;
}];

NSSArray *newArr = [arr sortedArrayUsingComparator:^NSComparisionResult(Person *obj1, Person *obj2){
    return obj1.age > obj2.age;
}];

=======================
NSArray和NSString之间转换
======

NSArray *arr = @[@"ab", @"cd", @"ef", @"gh"];
NSString *str = @"ab-cd-ef-gh"

NSString *newAStr = [arr componentsJoinedByString:@"-"]; = @"ab-cd-ef-gh"

NSArray *ArrStr = [str componentsSeparatedByString:@"-"]; = @[@"ab", @"cd", @"ef", @"gh"];

=======================
NSArray文件读写
======
//writeToFile: atomically:  只能写入Foundation框架的对象
BOOL flag = [arr writeToFile:@"/Users/apple/Desktop/a.txt" atomically:YES];

NSArray *newArr = [NSArray arrayWithContentsOfFile:@"/Users/apple/Desktop/a.plist"];

=======================
NSMutableArray 基本和应用
======
NSMutableArray *arrM = [NSMutableArray array];
//添加
[arrM addObject:@"newobj"];
[arrM insertObject:@"obj2" atIndex:1];

arrM.array = @[@"df", @"fds", @"fd"];====== ======  ============

//注意下边2种添加元素的区别
[arrM addObject:@[@"ds", @"fas", @"df"]];
[arrM addObjectsFromArray:@[@"ds", @"fas", @"df"]]


// 插入一组数据, 指定数组需要插入的位置, 和插入多少个
NSRange range = NSMakeRange(2, 2);
NSIndexSet *set = [NSIndexSet indexSetWithIndexesInRange:range];========    ======
[arrM insertObjects:@[@"A", @"B"] atIndexes:set];======== ======       =======

// 删除
[arrM removeObject:@"ds"];
[arrM removeObjectAtIndex:1];
[arrM removeLastObject];

//替换
[arrM replaceObjectAtIndex:1 withObject:@"new"];
arrM[1] = @"new";

//获取
[arrM objectAtIndex:2];
arrM[2];


===========================================================================
3. <NSDictionary>
=======================

//创建
NSDictionary *dict = [NSDictionary dictionaryWithObject:@"lnj" forKey:@"name"];

| [NSDictionary dictionaryWithObjectsAndKeys:
   @"jack", @"name",
   @"北京", @"address",
   @"32423434", @"qq", nil];

| [NSDictonary dictionaryWithObjects:@[@"lnj", @"15"] forKeys:@[@"name", @"age"]];


| @{@"name" : @"jack", @"age" : @"18", @"address" : @"paking"};


// 小练习,取出来"5分钟突破iOS编程"这本书
NSArray *persons = @[
                     @{@"name" : @"jack", @"qq" : @"432423423", @"books": @[@"5分钟突破iOS编程", @"5分钟突破android编程"]},
                     @{@"name" : @"rose", @"qq" : @"767567"},
                     @{@"name" : @"jim", @"qq" : @"423423"},
                     @{@"name" : @"jake", @"qq" : @"123123213"}
                     ];
NSString *book = persons[0][@"books"][0];

//获取元素
NSString *str = [dict objectForkey:@"name"];
| dict[@"name"];
| dict[@"age"];
| dict[@"address"];

// 遍历
for(id key in dict)
{
    id obj = dict[key];
    NSLog(@"key = %@ ==== %@", key, obj);
}

[dict enumerateKeysAndObjectsUsingBlock:^(id key, id obj, BOOL *stop)
{
    NSLog(@"key = %@ ==== %@", key, obj);
}];

//写入文件, 写如文件的时候是"""""""无序的无序的无序的
[dict writeToFile:@"/Users/apple/Desktop/dict.plist" atomically:YES];

//从文件获取
NSDictionary *dict = [NSDictionary dictionaryWithContentsOfFile:@"/Users/apple/Desktop/dict.plist"];


=======================
NSMutableDictionary 基本和应用
======
NSMutableDictionary *dictM = [NSMutableDictionary dictionary];

//添加
[dictM setObject:@"jack" forKey:@"name"];
[dictM  setValue:@"18" forKey:@"age"];

[dictM setValuesForKeysWithDictionary:@{@"age" : @"18", "addreess" : @"paking"}];

dictM.dictionary = @{@"age" : @"18", "addreess" : @"paking"};

//删除
[dictM removeObjectForKey:@"name"];

[dictM removeObjectsForKeys:@[@"name", @"age", @"address"]];

//替换, 相同的key,新的value会替换旧的value
[dictM setObject:@"lnj" forKey:@"name"]
dictM[@"name"] = @"lnj";

/*
 小总结:在不可变字典中,如果有相同的key,只保留第一个key的value,后边的key对应的value不会被保存

 在可变字典中,如果有相同的key, 只保留最后一个key对应的value,前边的value不会被保存
 */


===========================================================================
4.OC中常用的结构体
=======================
NSRange r1 = NSMakeRange(1, 2);
r1.location = 1;
r1.length = 2;

CGPoint p1 = CGPointMake(3, 4);
p1.x = 3;
p1.y = 4;

CGSize s1 = CGSizeMake(20, 50);
s1.width = 20;
s1.height = 50;

CGRect r1 = CGRectMake(3, 4, 20, 50);
r1.origin = CGPointMake(3, 4);
r1.size = CGSizeMake(20, 50);

CGRect r2 = {CGPointZero, CGSizeMake(20, 50)};
CGRect r3 = {CGPointZero, CGSizeZero};
CGRect r2 = { {0, 0}, {100, 90}};

CGRect r3 = {p1, s1};

// 判断,
BOOL flag1 = CGPointEqualToPoint(p1, p2);
BOOL flag2 = CGRectContainsPoint(r1, p1);
| CGSizeEqualToSize(s1, s2);
| CGRectEqualToRect(r1, r2);


===========================================================================
5.<NSNumber>
=======================
//基本数据类型转换为对象
int iint = 10;
float ffloat = 11.5;
NSNumber *iintNum = [NSNumber numberWithInt:iint];
NSNumber *ffloatNum = [NSNumber numberWithFloat:ffloat];

//对象转换为基本数据类型
int i = [iintNum intValue];
float f = [ffloatNum floatValue];
//变量加 ()
NSNumber *inew = @10 | @(iint);

===========================================================================
6.<NSValue>
=======================
//包装常用的结构体
CGPoint p1 = CGPointMake(2, 3);
NSValue *vp1 = [NSVlaue valueWithPoint:p1];
//然后就可以放入数组了
NSArray *arr = @[vp1];

// 包装自定义的结构体
typedef struct{
    int age;
    char *name;
    double height;
} Person;
Person p1 = {18, "jack", 1.75};

// valueWithBytes: 接收一个指针, 传递需要包装的结构体的变量的地址
// objCType: 需要传递需要包装的数据类型
NSValue *vp1 = [NSValue valueWithBytes:&p1 objCType:@encode(Person)];

// 从NSValue中取出自定义包装的结构体
//注意是地址
Person res;
[vp1 getValue:&res];


===========================================================================
6.<NSDate>
=======================
//创建
//获取当前时间
NSDate *now = [NSDate date];
//获取当前时区
NSTimeZone *zone = [NSTimeZone systemTimeZone];
//过去当前时区和格林纸时区的时间差,秒
NSUInteger seconds = [zone secondsFromGMTForDate:now];
// 计算出当前时区的时间, 现在时间加上间隔的秒数
NSDate *newDate = [now dateByAddingTimeInterval:seconds];

// NSDate - >NSString,自动转换为当前时区的时间
NSDate *date = [NSDate date]; // 2016年7月8日 7时14分13秒
NSDateFormatter *formatter = [[NSDateFormatter alloc] init];
formatter.dateFormat = @"yyyy年MM月dd日 HH时mm分ss秒";
NSString *dateStr = [formatter stringFromDate:date]; // GMT时间自动转换为当前时区的时间
输出的是2016年7月8日 15时14分13秒

// NSString - > NSDate,自动转换为了GMT的时间
NSString *dateStr = @"2016-4-28 15:15:16";
NSDateFormatter *formatter = [[NSDateFormatter alloc] init];
formatter.dateFormat = @"yyyy-MM-dd HH-mm-ss";
NSDate *date = [formatter dateFromString:dateStr]; // 自动转换为了GMT的时间
输出的是2016-4-28 7:15:16

===========================================================================
7.<NSCalendar>
=======================
//获取当前时间的年月日时分秒
//获取当前时间
NSDate *now = [NSDate date];

//获取日历对象
// 利用日历类从当前时间对象中获取 年月日时分秒(单独获取出来)
NSCalendar *calendar1 = [NSCalendar currentCalendar];

// components: 参数的含义是, 问你需要获取什么?
// 一般情况下如果一个方法接收一个参数, 这个参数是是一个枚举 , 那么可以通过|符号, 连接多个枚举值
NSCalendarUnit type = NSCalendarUnitYear |
                        NSCalendarUnitMonth |
                        NSCalendarUnitDay |
                        NSCalendarUnitHour |
                        NSCalendarUnitMinute |
                        NSCalendarUnitSecond;
// 获取的元素放到哪个日历对象里边, 组成的有哪些部件, 从哪个时间获取
NSDateComponents *cmps = [calendar1 components:type fromDate:now];

NSLog(@"year = %ld", cmps.year);
NSLog(@"month = %ld", cmps.month);
NSLog(@"day = %ld", cmps.day);
NSLog(@"hour = %ld", cmps.hour);
NSLog(@"minute = %ld", cmps.minute);
NSLog(@"second = %ld", cmps.second);

//比较过去的时间和现在时间,相差多少年,多少哦月,多少天,多少时分秒
NSString *dateStr = @"2016-4-28 15:15:16";
NSDateFormatter *formatter = [NSDateFormatter alloc] init];
formatter.dateFormat = @"yyyy-MM-dd HH-mm-ss";
NSDate *date = [formatter dateFromString:dateStr]; // 自动把字符串的时间转化为GMT时间
//相差的时间组成部件
NSDateComponents *cmps2 = [calendar1 components:type fromDate:date toDate:now options:0];

NSLog(@"%ld年%ld月%ld日%ld小时%ld分钟%ld秒钟",cmps2.year, cmps2.month, cmps2.day, cmps2.hour, cmps2.minute, cmps2.second);

===========================================================================
8.<NSFileManager>
=======================
//单例对象
NSFileManager *manager = [NSFileManager defaultManager];

//判断文件或者文件夹是否存在
BOOL flag = [manager fileExistsAtPath:@"/Users/apple/Desktop/a.plist"];

// 判断文件或者文件夹存在的同时,判断是不是文件夹
BOOL dir = NO;
BOOL flag = [manager fileExistsAtPath:@"/Users/apple/Desktop/a.plist" isDirectory:&dir];

// 获取文件或者文件夹的属性// 放到字典中
NSDictionary *info = [manager attributesOfItemAtPath:@"/Users/apple/Desktop/" error:nil];

//获取文件夹下边所有的文件,只获取当前文件夹下的文件,不获取子文件夹
NSArray *dirarr = [manager contentsOfDirectoryAtPath:@"/Users/apple/Desktop/" error:nil];

// 获取子文件和当前文件的名字
NSArray *dirarr = [manager subpathsAtPath:@"/Users/apple/Desktop/"];
NSArray *dirarr = [manager subpathsOfDirectoryAtPath:@"/Users/apple/Desktop/" error:nil];

//创建文件夹
// createDirectoryAtPath: 告诉系统文件夹需要创建到什么位置
// withIntermediateDirectories: 如果指定的文件路径中有一些文件夹不存在, 是否自动创建不存在的文件夹
// attributes: 指定创建出来的文件夹的属性
// error: 是否创建成功, 如果失败会给传入的参数赋值
// 注意: 该方法只能用于创建文件夹, 不能用于创建文件
BOOL flag = [manager createDirectoryAtPath:@"/Users/apple/Desktop/newnew" withIntermediateDirectories:NO attributes:nil error:nil];

// 创建文件
// createFileAtPath: 指定文件创建出来的位置
// contents : 文件中的内容
// attributes: 创建出来的文件的属性
// NSData : 二进制数据
// 注意: 该方法只能用于创建文件, 不能用于创建文件夹
NSString *str = @"江哥真帅";
NSData *data = [str dataUsingEncoding:NSUTF8StringEncoding]; // 把字符串以UTF8编码转化为二进制数据
[manager createFileAtPath:@"/Users/apple/Desktop/newnew.plist" contents:data attributes:nil];













===========================================================================
===========================================================================
=======================
copy @property中用于String 和 block,
block-
======
遵守<NSCopying, NSMutableCopying>
- (instancetype)copyWithZone:(NSZone *)zone
{
    1.创建新对象
    Person *copy = [[[self class] allocWithZone:zone] init];
    2.复制属性
    copy.age = self.age;
    3.返回新对象
    return copy;

    子类中是[super copyWithZone:zone];然后复制子类的属性，并返回复制好的对象
}
MRC 下,栈中block用到对象直接用,堆中block用到对象加__block;   Block_release(_inb);要记得释放对中的block

ARC 下,全部正常使用,特例是当对象自己的block用到自己,需要中转{__weak Person *pWeak = p };

    <NSString *str = @"dsdsf", @"dsdsf"是常量, str是变量>
    堆block:block可以访问外部局部变量,但是不能修改,它是拷贝了常量的地址.只读, 变量的值拷贝
    如过外部局部变量加上了__block, 那么就是拷贝了变量的地址, 可以通过指针修改str保存的地址(值).

    静态变量 和 全局变量 在加和不加 __block 都会直接引用 变量地址。也就意味着 可以修改变量 的值。
    1.在block内部可以访问定义在外部的局部变量和全局变量
    2.在block内部可以修改全局变量的值,但是不能修改定义在外部 的局部变量
    3.要修改,加__block

    MRC下外部栈block用到对象:正常使用
    MRC下外部堆block用到对象:__block Person
    MRC下自己的block用到自己:__block Person

    ARC下外部栈block用到对象:正常使用对象
    ARC下外部堆block用到对象:正常使用对象
    ARC下自己的block用到自己:{ Person *p = [[Person alloc] init];
        __weak Person *pWeak = p;  }中转



    =======================
    singleton
    ======
    遵守<NSCopying, NSMutableCopying>
#if __has_feature(objc_arc)
    NSLog(@"ARC");
#else
    NSLog(@"MRC");
#endif

#define interfaceSingleton(name) +(instancetype)share##name

#define implementationSingleton(name) \
static name *_instance = nil; \


    + (instancetype)shareTools
    {
        return [[[self class] alloc] init];
    }

    static Tools *_instance = nil;
    - (instancetype)allocWithZone:(NSZone *)zone
    {
        //        if(_intance == nil)
        //        {
        //            _intance = [[super allocWithZone:zone] init];
        //        }
        //            return _intance;
        static dispatch_once_t onceToken;
        dispatch_once(&onceToken, ^{
            _instance = [[super allocWithZone:zone] init];
        });
        return _instance;
    }

    - (instancetype)copyWithZone:(NSZone *)zone
    {
        return _instance;
    }
    - (instancetype)mutableCopyWithZone:(NSZone *)zone
    {
        return _instance;
    }


    MRC 添加内存管理代码
    - (oneway void)release
    {

    }
    - (instancetype)retain
    {
        return _intance;
    }

    - (NSUInteger)retainCount
    {
        return MAXFLOAT;
    }
    - (id)autorelease
    {
        return _instance;
    }

    =======================
    protocol 协议 @required, @optional;.h中声明协议@protocol, .m中import""
    ======

    协议属于谁就写到谁的头文件.h 中
    命名规则 如下 注意 <协议名字> , <方法名字>
    谁触发该协议将谁发送出去,
    @class Baby;
    @protocol BabyProtocol <NSObject> // BabyDelegate

    - (void)babyCry:(Baby *)baby; // 谁触发该协议将谁发送出去,

    @end
    注意声明里边的id协议限定 ,还有代理的名字
    @interface Baby : NSObject
@property (nonatomic, strong) id<BabyProtocol> delegate;
- (void)cry;
@end

    @implementation Baby
- (void)cry
{   调用代理方法的时候要进行判断,并将self发送出去
    if([_delegate responceToSelector:@selector(babyCry:)])
    {
        [_delegate babyCry:self];
    }
}

@end


    =======================
    <Category> 为系统自带的类增加分类叫<非正式协议>
    为自己的类增加分类叫<分类>
    还有一种特殊的是<类扩展> <class Extension> 匿名分类,类延展
    ======
    第一种分类有声明.h和实现.m,只能增加方法,不能增加属性
    如果分类中有和原来类相同的方法,那么就调用分类的,
    如果有多个分类有相同的方法,那么久调用最后编译的
    分类中的@property, 只会生成setter/getter方法的声明, 不会生成实现,以及私有的成员变量
    @interface NSString (NJ)
    +
    -
    @end

    @implementation NSString (NJ)
    +
    -
    @end

    第二种分类,<类扩展> <class Extension> 匿名分类, 类延展,声明和实现直接写在本类的.m中,可以增加私有属性和方法
    @interface Person ()
    {
        int _double;
    }
    - (void)run;
    @end

    @implementation
    - (void)run
    {
        _double++;
        NSLog(@"%s", __func__);
    }
    @end

    类扩展里边的方法,只能通过
    if([p1 responceToSelector:@selector(私有方法)])
    {
        [p1 performSelector:@selector(私有方法)];
    }
    不能通通过动态数据类型来指向对象调用 id p = [Person new];
    错误 :[id对象 私有方法];
    匿名分类生成的属性,只能在.m中访问,就是在本类中访问

    /*
     总结主函数.m中包含的.h文件中声明的方法,它才知道啊啊,错误 :[id对象 私有方法];
     */



    =======================
    MRC ARC 混编
    flag -fno-objc-arc
    ======

    =======================
    ARC 判断释放对象的唯一标准:有没有强指针指向
    ARC: Automatic(自动) Reference(引用) Counting(计数)
    ======
    ARC 默认__strong;
    @property 中写对象用
    1>strong , weak,
    2>copy,用于字符串和block
    3>基本数据类型用assign
    4>循环引用的时候strong weak,两端....
    5>strong相当于MRC中的retain;
    6>weak 相当于MRC中的assign




    =======================
    autoreleasepool 自动释放池中不能放入 大 的对象,或者一起放入 很多 对象
    ======
    对象只有调用了autorelease,才会加入自动释放池子,自动释放池相当于延迟了对象的释放,只要在自动释放池内,
    可以随意使用对象....
    autoreleasepool是栈模式,先进后出, 一步步走哦哦哦哦.对象总是放在每个栈顶的池子.
    autorelease的应用
    // 注意: Foundation框架的类, 但凡是通过类工厂方法创建的对象都是autorelease的
    // 开发中MRC中,类工厂方法中都自动添加了autorelease
    // 注意: 自定义构造方法中,不能有autorelease;

    + (instancetype)person
    {
        return  [[[[self class] alloc] init] autorelease];
    }

    + (instancetype)personWithAge:(int)age
    {
        return [[[[self class] alloc] initWithAge:age] autorelease];
    }


    =======================
    MRC  MRC: Manul(手动) Reference(引用) Counting(计数)
    copy retain alloc 对应一次 release,或者autorelease;
    野指针和僵尸对象, 空指针
    ======
    @class Room;
    @interface Person : NSObject
{
    Room *_room;
    NSString *_name;
}
- (void)setRoom:(Room *)room;
- (void)setName:(NSString *)name;

- (Room *)room;
- (NSString *)name;
@property (nonatomic, assign) int age;

@end

    @implementation Person
- (void)setRoom:(Room *)room
{
    if(_room != room)
    {
        [_room release];
        _room = [room retain];
    }
}
- (void)setName:(NSString *)name
{
    if(_name != name)
    {
        [_room release];
        _name = [name copy];
        // copy方法copyWithZone:(NSZone *)里边包含了allocWithZone:(NSZone *)zone
    }
}

- (Room *)room
{
    return _room;
}
- (NSString *)name
{
    return _name;
}



- (void)dealloc
{
    [_name release];
    [_room release];

    装逼写法如下 // 调用setter方法,先release旧对象, 再赋值一个空对象
    /*
     self.name = nil;
     self.room = nil;
     */
    注意block的release
    Block_release(_pBlock);//block释放的时候会给它里边的对象发送一条release消息
    [super dealloc];  // 最后调用dealloc
}
@end



    =======================
    @property修饰符
    ======
    nonatomic, atomic,
    readwrite, readonly,

    retain,自动生成内存管理的对象方法release旧值,retain新值,循环retain的时候另一端用assign
    strong,weak;
    copy,自动自动生成内存管理的对象方法release旧值,copy新值,

    assign;自动生成基本的setter和getter方法,不过滤数据

    getter=isVip;BOOL 类型
    setter=注意方法后边必须加上::::::


    =======================
    @class
    1>告诉编译器这是一个类, 不拷贝,.m中用到类的时候在.m中import"", 提升编译效率,以免连锁拷贝
    2>防止循环#import""
    ======


    =================================================================
    NSArray, NSMutableArray, MRC 下集合的内存管理

    当添加一个对象到集合中的时候,会retain对象
    当移除对象的时候,会release对象,
    当release集合的时候, 集合会对所有的对象发送一条release消息;


    copy对 对象的内存管理
    如果是浅拷贝,就会retain对象
    如果是深拷贝,不会retain旧对象, 只是新对象的引用计数器为1,所以最后要release新的对象,
    copy里边有copyWithZone, copyWithZone里边有allocWithZone;

    一次alloc 对应一次release
    一次retain 对应一次 release
    一次copy 对应一次release

    - (void)dealloc
    {
        // 只要给block发送一条release消息, block中使用到的对象也会收到该消息
        Block_release(_pBlock);
        [super dealloc];
    }
    =================================================================


    =======================
    SEL 也属于对象
    ======
    1>[p1 responceToSelector:@selector(run)]  判断作用
    2>[p1 performSelector:@selector(run)] 调用SEL方法
    先把方法包装成SEL数据
    根据SEL数据找到对应的方法地址
    调用方法
    3.配合对象将SEL类型作为方法的形参
    Car *c = [Car new];

    SEL sel = @selector(run);

    Person *p = [Person new];
    - (void)makeObject:(id)obj andSel:(SEL)sel;

    [p makeObject:c andSel:sel]; ====== ==== = = = = =  = = = = = = ==

    performSelector: 可以传入     对象参数,   最多可以有2个
    [p performSelector:@selector(say:) withName:@"jack"];


    ===========================================================================
    类的本质   和    类的启动过程:
    类其实也是一个对象, 这个对象会在这个类  第一次被使用  的时候创建
    只要有了类对象, 将来就可以通过类对象来创建实例对象
    实例对象中有一个isa指针, 指向创建自己的类对象

    1>类也是对象,[Person class] [p class];可以通过类对象创建实例对象
    实例对象(实例变量)isa->类对象(对象方法)isa->
    元类对象(类方法)isa->
    根元类对象NSObject isa指向自己<-isa NSObject类对象<-isa NSObject实例对象
    根元类对象NSObject,它继承NSObject类对象

    2>load
    当程序启动的时候,会先把所有类的代码加载到代码区,
    先加载父类,然后子类
    3>initialize
    会在类第一次使用的时候调用,仅仅会调用一次,初始化作用,
    先初始化父类,然后初始化子类
    ===========================================================================











    ================================================
    自定义构造方法(init) 和 自定义类工厂方法
    两个要配对使用
    alloc做了3件事情
    1>开辟储存空间
    2>将所有的属性设置为0, 初始化isa指针
    3>返回实例对象的地址

    init
    1>初始化成员变量,默认什么都没做,
    2>返回初始化后的实例对象的地址
    ======

    @interface Person : NSObject

    @property (nonatomic, assign) int age;
    + (instancetype)person; // 对应了init方法

    - (instancetype)initWithAge:(int)age;

    + (instancetype)personWithAge:(int)age;
    @end

    @implementation Person
+ (instancetype)person // 对应了init方法
{
    return [[[self class] alloc] init];
}

- (instancetype)initWithAge:(int)age
{
    if(self = [super init])
    {
        _age = age;
    }
    return self;
}

+ (instancetype)personWithAge:(int)age
{
    return [[[self class] alloc] initWithAge:age];
}
@end





    @interface Student : Person
@property (nonatomic, assign) int no;

+ (instancetype)student;

- (instancetype)initWithAge:(int)age andNo:(int)no;

+ (instancetype)studentWithAge:(int)age andNo:(int)no;
@end

    @implementation Student
+ (instancetype)student
{
    return [[[self class] alloc] init];
}

- (instancetype)initWithAge:(int)age andNo:(int)no
{
    if(self = [super initWithAge:age])
    {
        _no = no;
    }
    return self;
}

+ (instancetype)studentWithAge:(int)age andNo:(int)no
{
    return [[[self class] alloc] initWithAge:age andNo:no];
}
@end



    ================================================
    id动态数据类型
    运行时才知道对象的真实类型

    静态数据类型
    在编译的时候就知道哪些方法和属性成员变量
    ==================================================
    默认情况下所有的一般数据类型都是静态数据类型

    多态情况下,父类指针指向子类对象,不能调用子类特有的方法,但是可以用id动态数据类型来指向子类对象就可以调用

    ARC id指向的对象,不能调用对象的私有方法,会经常报错!!!!! ARC实现真正的私有方法

    MRC id 调用私有方法不会报错, MRC下没有真正的私有方法

    ARC私有方法不能用id类型来调,特有方法才可以

    [p isKinOfClass:Person];
    [p isMemberOfClass:Student];
    [Student isSubclassOfClass:Person];


    ====================================================
    @porperty是一个编译器指令
    在<Xocde4.4>之前, 可以使用@porperty来代替getter/setter方法的声明
    也就是说我们只需要写上@porperty就不用写getter/setter方法的声明

    @synthesize是一个编译器指令, 它可以简化我们getter/setter方法的实现
    @synthesize age = _age;  标准写法
    1.在@synthesize后面告诉编译器, 需要实现哪个@property生成的声明
    2. 告诉@synthesize, 需要将传入的值赋值给谁和返回谁的值给调用者

    ====================================================








    ===================================

    description 打印对象的时候默认打印<类名:对象地址>
    重写
    - (NSString *)description
    {
    return [NSString stringWithFormat:@"age = %d, name=  %@", _age,_name];
}

====================================


======================================================================
@public 本类,子类,其他类都可以访问
@private 本类可以访问, 子类和其他类都不能访问
@protected 本类,子类可以访问,其他类不能访问,默认的
@package  如果是在其它包中访问那么就是private的
如果是在当前代码所在的包种访问就是public的
======================================================================


======================================================================
什么是多态:
事物的多种表现形态

在程序中如何表现:
父类指针指向子类对象

优点:
提高了代码的扩展性

注意点:
如果父类指针指向子类对象, 如果需要调用子类特有的方法, 必须先强制类型转换为子类才能调用
注意点: 在多态中, 如果想调用子类特有的方法必须强制类型转换为子类才能调用
注意点: 在多态中, 如果想调用子类特有的方法必须强制类型转换为子类才能调用
注意点: 在多态中, 如果想调用子类特有的方法必须强制类型转换为子类才能调用
注意点: 在多态中, 如果想调用子类特有的方法必须强制类型转换为子类才能调用
注意点: 在多态中, 如果想调用子类特有的方法必须强制类型转换为子类才能调用
注意点: 在多态中, 如果想调用子类特有的方法必须强制类型转换为子类才能调用
注意点: 在多态中, 如果想调用子类特有的方法必须强制类型转换为子类才能调用
======================================================================

======================================================================
super
1>调用父类的方法
2>调用自己方法的同时,调用父类的方法
3>重写父类的方法,然后在这个方法中又想调用父类的同名方法, // 或者必须调用父类的方法
======================================================================


======================================================================
继承是:抽取重复代码
优点:
提高代码的复用性
可以让类与类之间产生关系, 正是因为继承让类与类之间产生了关系所以才有了多态

======================================================================


======================================================================
如果self在对象方法中, 那么self就代表调用当前对象方法的那个对象
如果self在类方法中, 那么self就代表调用当前类方法的那个类
总结:
我们只用关注self在哪一个方法中 , 如果在类方法那么就代表当前类, 如果在对象方法那么就代表"当前调用该方法的对象"


注意:
>self会自动区分类方法和对象方法, 如果在类方法中使用self调用对象方法, 那么会直接报错

>不能在对象方法或者类方法中利用self调用当前self所在的方法,死循环
>不能在对象方法或者类方法中利用self调用当前self所在的方法
>不能在对象方法或者类方法中利用self调用当前self所在的方法

使用场景:
可以用于在对象方法之间进行相互调用
可以用于在类方法之间进行相互调用

可以用于区分成员变量和局部变量同名的情况
======================================================================


======================================================================

封装: 屏蔽内部实现的细节, 仅仅对外提供共有的方法/接口
好处: 保证数据的安全性,将变化隔离
规范: 一般情况下不会对外直接暴露成员变量, 都会提供一些共有的方法进行赋值
      成员变量都需要封装起来

- (void)setMin:(int)min
{
    // setter方法还有一个好处: 监听属性的变化，重点！！！！！！！
    _min = min;
    // 每次重新设置最小值, 那么就重新计算平均值
    _average = (_min + _max) / 2;
}
======================================================================


======================================================================

1.什么是面向对象?
找对象使用对象的方法(功能)

2.对象
3.什么是类?
类就是用于描述对象的共性特征
主要用于描述对象的属性和行为

======================================================================


======================================================================

修改项目模板以及main函数中的内容
/Applications/Xcode.app/Contents/Developer/Library/Xcode/Templates/Project Templates/Mac/Application/Command Line Tool.xctemplate/

修改OC文件头部的描述信息
/Applications/Xcode.app/Contents/Developer/Library/Xcode/Templates/File Templates/Source/Cocoa Class.xctemplate




Xcode文档安装的位置1:
/Applications/Xcode.app/Contents/Developer/Documentation/DocSets
注意: 拷贝之前最好将默认的文档删除, 因为如果同时存在高版本和低版本的文档, 那么低版本的不会显示
Xcode文档安装的位置2:
/Users/你的用户名/Library/Developer/Shared/Documentation/DocSets
如果没有该文件夹可以自己创建一个



======================================================================


======================================================================
匿名就是没有名字, 匿名对象就是没有名字的对象

有名字的对象
只要用一个指针保存了某个对象的地址, 我们就可以称这个指针为某个对象

因为类的本质是一个结构体, 所以我们是用一个指向结构体的指针保存了对象的地址,
所以我们可以通过指针操作结构体的方式来操作对象

Foundation.h我们称之为主头文件, 主头文件中又拷贝了该工具箱中所有工具的头文件, 我们只需要导入主头文件就可以使用该工具箱中所有的工具, 避免了每次使用都要导入一个工具对应的头文件
工具箱的地址: /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/System/Library/Frameworks
规律: 所有的主头文件的名称都和工具箱的名称一致
所有的主头文件都是导入了该工具箱中所有工具的头文件

import 的功能和 include一样, 是将右边的文件拷贝到当前import的位置
为了降低程序员的负担, 防止重复导入, 避免程序员去书写 头文件声明, 那么OC给出来一个新的预处理指令import
import优点: 会自动防止重复拷贝

因为OC完全兼容C, 所以可以在OC程序中编写C语言代码
并且可以将C语言的源文件和OC的源文件组合在一起生成可执行文件


======================================================================


======================================================================
类方法的应用场景
如果方法中没有使用到属性(成员变量), 那么能用类方法就用类方法
类方法的执行效率比对象方法高
+ (NSString *)colorWithNumber:(IColor)number;
{
    NSString *name;
    switch (number) {
        case 0:
            name = @"黑色";
            break;
        case 1:
            name = @"白色";
            break;
        case 2:
            name = @"土豪金";
            break;
        default:
            name = @"华强北";
            break;
    }
    return name;
}


创建对象的时候返回的地址其实就是对象的第0个属性的地址
但是需要注意的是: 对象的第0个属性并不是我们编写的_age, 而是一个叫做isa的属性
isa是一个指针, 占8个字节
======================================================================

======================================================================
写在函数和大括号外部的变量, 我们称之为全局变量
作用域: 从定义的那一行开始, 一直到文件末尾
全区变量可以先定义在初始化, 也可以定义的同时初始化
存储: 静态区
程序一启动就会分配存储空间, 直到程序结束才会释放
int a;
int b = 10;

int main(int argc, const char * argv[]) {
    写在函数或者代码块中的变量, 我们称之为局部变量
    作用域: 从定义的那一行开始, 一直到遇到大括号或者return
    局部变量可以先定义再初始化, 也可以定义的同时初始化
    存储 : 栈
    存储在栈中的数据有一个特点, 系统会自动给我们释放
    int num = 10;
    {
        int value;
    }
    ======================================================================



```
