# button label iamgeView choose LMJ

````
特点
UIButton
既能显示文字，又能显示图片（能显示2张图片，背景图片、内容图片）
长按高亮的时候可以切换图片\文字
直接通过addTarget...方法监听点击

UIImageView
能显示图片，不能直接通过addTarget...方法监听点击

UILabel
能显示文字，不能直接通过addTarget...方法监听点击
````

````
选择
仅仅是显示数据，不需要点击
建议选择UIImageView、UILabel

不仅显示数据，还需要监听点击
建议选择UIButton
其实UIImageView、UILabel也可以通过手势识别器来监听（后面课程会学）

长按控件后，会改变显示的内容
不用考虑了，选择UIButton（因为UIButton有highlighted这种状态）

同时显示2张图片：背景图片、内容图片
不用考虑了，选择UIButton
````
