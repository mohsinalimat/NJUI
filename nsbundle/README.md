# `NSBundle`

````objc

[NSBundle mainBundle] pathForResouse
- (nullable NSString *)pathForResource:(nullable NSString *)name ofType:(nullable NSString *)ext;

// 加载xib文件的
[NSBundle mainBundle] loadNib

- (NSArray *)loadNibNamed:(NSString *)name owner:(id)owner options:(NSDictionary *)options;

````
