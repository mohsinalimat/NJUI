## UICollectionView,

### 基本`注意点`使用`UICollectionViewFlowLayout` 和 `registerClass` 属性的详细说明



-  0.`UICollectionViewController`层次结构：控制器View 上面`UICollectionView`
    -  `self.view != self.collectionView`

- 1.初始化的时候必须设置布局参数，通常使用系统提供的流水布局UICollectionViewFlowLayout

- 2.cell必须通过注册

- 3.自定义cell

- 4详细说明属性

```objc

static NSString *const ID = @"cell";
- (instancetype)init
{
    UICollectionViewFlowLayout *flowLayout = [[UICollectionViewFlowLayout alloc] init];

    flowLayout.itemSize = CGSizeMake(100, 100); // 每个item的大小
    // 每个item之间的间距, 竖直滚动就是竖{直}间距. 水平方向滚动就是水{平}方向的间距
    flowLayout.minimumInteritemSpacing = 0;

    // 每个组内行间距, 竖直滚动就是水平方向的间距, 水平滚动就是竖直方向的间距
    flowLayout.minimumLineSpacing = 60;

    // 每个组的四周的间距
    flowLayout.sectionInset = UIEdgeInsetsMake(44, 30, 200, 30);
    // 滚动方向
    flowLayout.scrollDirection = UICollectionViewScrollDirectionHorizontal;

   return [super initWithCollectionViewLayout:flowLayout];
}

- (void)viewDidLoad
{
    [super viewDidLoad];

    self.view.backgroundColor = [UIColor redColor];
    // 只能看到这个颜色
    self.collectionView.backgroundColor = [UIColor grayColor];
    // 翻页功能
    self.collectionView.pagingEnabled = YES;
    // 弹簧功能
    self.collectionView.bounces = NO;
    // 水平滚动条
    self.collectionView.showsHorizontalScrollIndicator = NO;
    // 使用的时候必须注册
    [self.collectionView registerClass:[LMJCollectionViewCell class] forCellWithReuseIdentifier:ID];
}

```



