# xib file use



````objc

    //  加载xib文件
     // Test.xib --编译--> Test.nib

     // 方式1
    NSArray *objs = [[NSBundle mainBundle] loadNibNamed:@"Test" owner:nil options:nil];
    [self.view addSubview:objs[1]];

     方式2
     // 一个UINib对象就代表一个xib文件
    UINib *nib = [UINib nibWithNibName:@"Test" bundle:[NSBundle mainBundle]];
     // 一般情况下，bundle参数传nil，默认就是mainBundle
    UINib *nib = [UINib nibWithNibName:@"Test" bundle:nil];
    NSArray *objs = [nib instantiateWithOwner:nil options:nil];
    [self.view addSubview:[objs lastObject]];

````
