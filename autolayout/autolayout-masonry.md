# Autolayout-Masonry

- 使用步骤
     - 添加Masonry文件夹的所有源代码到项目中
    - 添加2个宏、导入主头文件
    ```objc
    // 只要添加了这个宏，就不用带mas_前缀
    #define MAS_SHORTHAND
    // 只要添加了这个宏，equalTo就等价于mas_equalTo
    #define MAS_SHORTHAND_GLOBALS
    // 这个头文件一定要放在上面两个宏的后面
    #import "Masonry.h"
    ```

- 添加约束的方法

    ```objc
    // 这个方法只会添加新的约束
     [view makeConstraints:^(MASConstraintMaker *make) {

     }];

    // 这个方法会将以前的所有约束删掉，添加新的约束
     [view remakeConstraints:^(MASConstraintMaker *make) {

     }];

     // 这个方法将会覆盖以前的某些特定的约束
     [view updateConstraints:^(MASConstraintMaker *make) {

     }];
    ```

- 约束的类型
    ```objc
    1.尺寸：width\height\size
    2.边界：left\leading\right\trailing\top\bottom
    3.中心点：center\centerX\centerY
    4.边界：edges
    ```

# `示例代码`

- mas_equal 和 equal 的区别代码
    - mas_equalTo：这个方法会对参数进行包装
    - equalTo：这个方法不会对参数进行包装
    - mas_equalTo的功能强于 > equalTo

## `注意点`
#### `约束blueView 的边框 距离父控件的边缘的距离` 前边用的是edges`边缘` 后边的是insets`嵌入物,插入`

```objc
    /** 约束blueView 的边框 距离父控件的边缘的距离 */
    [blueView makeConstraints:^(MASConstraintMaker *make) {
    // 我的边缘等于父控件的边缘``加上一个间隔边距``
        make.**edges**.equalTo(self.view).**insets****(UIEdgeInsetsMake(20, 20, 20, 20));
    }];

```
---

```objc

    [blueView mas_makeConstraints:^(MASConstraintMaker     *make) {
        // 宽度高度约束
        make.height.mas_equalTo(self.view).multipliedBy(    0.5).offset(-50);
        // 右边
        make.right.mas_equalTo(self.view).offset(-20);
        //        make.right.offset(-20);
        // 顶部
        make.top.mas_equalTo(self.view).offset(20);
        //        make.top.offset(20);
    }];

===========================================
        [blueView mas_makeConstraints:^(MASConstraintMaker *make) {
            // 宽度高度约束
    //        make.size.equalTo([NSValue valueWithCGSize:CGSizeMake(100, 100)]);
    //        make.size.mas_equalTo(CGSizeMake(100, 100));
            make.size.mas_equalTo(100);
            // 右边
            make.right.equalTo(self.view).offset(-20);
            // 顶部
            make.top.equalTo(self.view).offset(20);
        }];
```

---

### 添加`2个宏`之后的区别代码

```objc

- (void)viewDidLoad {
    [super viewDidLoad];

    // 蓝色控件
    UIView *blueView = [[UIView alloc] init];
    blueView.backgroundColor = [UIColor blueColor];
    [self.view addSubview:blueView];

    // 红色控件
    UIView *redView = [[UIView alloc] init];
    redView.backgroundColor = [UIColor redColor];
    [self.view addSubview:redView];

    // 添加约束
    CGFloat margin = 20;
    CGFloat height = 50;
    [blueView makeConstraints:^(MASConstraintMaker *make) {
        make.left.equalTo(self.view.left).offset(margin);
        make.right.equalTo(redView.left).offset(-margin);
        make.bottom.equalTo(self.view.bottom).offset(-margin);
        make.height.equalTo(height);
        make.top.equalTo(redView.top);
        make.bottom.equalTo(redView.bottom);
        make.width.equalTo(redView.width);
    }];

    [redView makeConstraints:^(MASConstraintMaker *make) {
        make.right.equalTo(self.view.right).offset(-margin);
    }];
}

```
