# Status-Easy

##注意点1
- 只给图片设置顶部和水平居中的约束, 然后再随便设置内容
- 在实际现实中imageView会根据内容自己调整大小
![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160616_9.png)


##注意点2
- 自己SB, 注意后边10 这个常量的归属
- 计算cell高度之前强制更新, 因为imageView的内容才设置, 尺寸还没有变化
![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160616_12.png)


##注意点3
- 不要用预估高度, 在懒加载的时候随便使用一个cell, 计算出模型里边的高度属性
![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160616_13.png)
