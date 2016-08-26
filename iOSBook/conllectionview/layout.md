# Layout - 自定义继承自`UICollectionViewLayout`

- 1, 必要要实现`- (void)prepareLayout`,在方法里边算好所有的`UICollectionViewLayoutAttributes`布局
    - 如果需要3实现后, 把单个的计算写到3方法里边, 直接调用3方法, 创建并计算设置布局属性, 添加到数组

- 2, 必须要实现`- (NSArray *)layoutAttributesForElementsInRect:(CGRect)rect`
    -  在这个方法中返回自己算好的布局`UICollectionViewLayoutAttributes`数组

- 3, 必须要实现 在这个方法中需要返回indexPath位置对应cell的布局属性, 最好被1方法调用
    - 并且外界切换布局的时候, 系统会调用这个方法<br>

```objc
~ (UICollectionViewLayoutAttributes *)layoutAttributesForItemAtIndexPath:(NSIndexPath *)indexPath

```

- 4, 选择实现`- (CGSize)collectionViewContentSize`返回collectionView的内容大小


### 格子布局

```objc

@interface LMJGirdLayout ()
/** 所有的模型数据 */
@property (nonatomic, strong) NSMutableArray *array;

@end

@implementation LMJGirdLayout


- (NSMutableArray *)array
{
    if(_array == nil)
    {
        _array = [NSMutableArray array];
    }
    return _array;
}

- (void)prepareLayout
{
    [super prepareLayout];

    NSLog(@"------------------");

    [self.array removeAllObjects];

    NSUInteger count = [self.collectionView numberOfItemsInSection:0];


        for (NSInteger i = 0; i < count; i++)
        {
            UICollectionViewLayoutAttributes *atrb = [self layoutAttributesForItemAtIndexPath:[NSIndexPath indexPathForItem:i inSection:0]];

            [self.array addObject:atrb];
        }

}

- (NSArray<UICollectionViewLayoutAttributes *> *)layoutAttributesForElementsInRect:(CGRect)rect
{
    return self.array;
}

- (CGSize)collectionViewContentSize
{
    UICollectionViewLayoutAttributes *lastAtrb = self.array.lastObject;

    UICollectionViewLayoutAttributes *preLastAtrb = self.array[self.array.count-2];



    return CGSizeMake(0, MAX(CGRectGetMaxY(lastAtrb.frame), CGRectGetMaxY(preLastAtrb.frame)));
}


- (UICollectionViewLayoutAttributes *)layoutAttributesForItemAtIndexPath:(NSIndexPath *)indexPath
{
    NSInteger i = indexPath.item;

    UICollectionViewLayoutAttributes *atrb = [UICollectionViewLayoutAttributes layoutAttributesForCellWithIndexPath:[NSIndexPath indexPathForItem:i inSection:0]];

    CGFloat width = self.collectionView.frame.size.width / 2;

    CGFloat height = width;

    CGFloat x = 0;
    CGFloat y = 0;
    if(i == 0)
    {
        x = 0;
        y = 0;
    }else
    {

        UICollectionViewLayoutAttributes *lastAtrb = self.array[i - 6];

        x = lastAtrb.frame.origin.x;
        y = lastAtrb.frame.origin.y + height * 2;
        width = lastAtrb.frame.size.width;
        height = lastAtrb.frame.size.height;

    }

    atrb.frame = CGRectMake(x, y, width, height);


    return atrb;
}

@end

```


### 圆形布局

```objc

#import "XMGCircleLayout.h"

@interface XMGCircleLayout()
/** 布局属性 */
@property (nonatomic, strong) NSMutableArray *attrsArray;
@end

@implementation XMGCircleLayout

- (NSMutableArray *)attrsArray
{
    if (!_attrsArray) {
        _attrsArray = [NSMutableArray array];
    }
    return _attrsArray;
}

- (void)prepareLayout
{
    [super prepareLayout];

    [self.attrsArray removeAllObjects];

    NSInteger count = [self.collectionView numberOfItemsInSection:0];
    for (int i = 0; i < count; i++) {
        NSIndexPath *indexPath = [NSIndexPath indexPathForItem:i inSection:0];
        UICollectionViewLayoutAttributes *attrs = [self layoutAttributesForItemAtIndexPath:indexPath];
        [self.attrsArray addObject:attrs];
    }
}

- (NSArray *)layoutAttributesForElementsInRect:(CGRect)rect
{
    return self.attrsArray;
}

/**
 * 这个方法需要返回indexPath位置对应cell的布局属性
 */
- (UICollectionViewLayoutAttributes *)layoutAttributesForItemAtIndexPath:(NSIndexPath *)indexPath
{
    NSInteger count = [self.collectionView numberOfItemsInSection:0];
    CGFloat radius = 70;
    // 圆心的位置
    CGFloat oX = self.collectionView.frame.size.width * 0.5;
    CGFloat oY = self.collectionView.frame.size.height * 0.5;

    UICollectionViewLayoutAttributes *attrs = [UICollectionViewLayoutAttributes layoutAttributesForCellWithIndexPath:indexPath];

    attrs.size = CGSizeMake(50, 50);
    if (count == 1) {
        attrs.center = CGPointMake(oX, oY);
    } else {
        CGFloat angle = (2 * M_PI / count) * indexPath.item;
        CGFloat centerX = oX + radius * sin(angle);
        CGFloat centerY = oY + radius * cos(angle);
        attrs.center = CGPointMake(centerX, centerY);
    }

    return attrs;
}

- (CGSize)collectionViewContentSize
{
    return self.collectionView.frame.size;
}

@end
```
