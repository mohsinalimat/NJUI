## Performance optimization `性能优化`
### `Cell`的重复利用<br>
- 什么时候调用：`每当有一个cell进入视野范围内就会调用`

- 普通cell, 非自定义等高或者非等高

| 第一种方式 | 手动创建 | `if(cell==nil)` |
| -- | -- | -- |
| 第二种方式 | 手动注册 | `regiterClass` |
| 第三种方式 | storyboard<br>设置cell-ID<br>系统帮助注册 | 1一个cell-ID<br>2二个cell-ID<br>`在代码中直接根据ID拿到对应的cell` |



---
### 第一种方式`自己创建cell`
- 定义绑定标识
- 从tableView的缓存池子中根据绑定表示取出cell
- 如果为空,我们就自己创建并且绑定标识
- 重新覆盖置数据<br>

```objc

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    // 0.重用标识
    // 被static修饰的局部变量：只会初始化一次，在整个程序运行过程中，只有一份内存
    static NSString *ID = @"cell";

    // 1.先根据cell的标识去缓存池中查找可循环利用的cell
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:ID];

    // 2.如果cell为nil（缓存池找不到对应的cell）
    if (cell == nil) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:ID];
    }

    // 3.覆盖数据
    cell.textLabel.text = [NSString stringWithFormat:@"testdata - %zd", indexPath.row];

    return cell;
}

```

---
### 第二中方式 `手动注册->让系统帮助创建`
- 首先定义一个全局变量ID标识
- 在`- (void)viewDidLoad`中注册ID
- 在`- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath`中
    - 直接从tableView里边根据标识拿到cell
        - 如果没有,系统会根据标识,自动创建cell
            - 注意系统是调用是用这个方法<br>
                ```objc
                cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:ID]
                ```

    - 覆盖数据

- `示例代码`

```objc

// 定义重用标识
NSString *ID = @"hero";

- (void)viewDidLoad {
    [super viewDidLoad];

    // 注册某个标识对应的cell的类型
    [self.tableView registerClass:[UITableViewCell class] forCellReuseIdentifier:ID];
}

| 0:0 | 1:0 |---华丽分割线

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    // 1.去缓存池中查找cell
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:ID];

    // 2.覆盖数据
    XMGHero *hero = self.heroes[indexPath.row];
    cell.textLabel.text = hero.name;
    cell.imageView.image = [UIImage imageNamed:hero.icon];
    cell.detailTextLabel.text = hero.intro;

    return cell;
}

```

### 第三种方式,在`22.3chapter`里边


Label.text = hero.intro;

    return cell;
}

```

### 第三种方式,在`22.3chapter`里边


