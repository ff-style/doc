### UITapGestureRecognizer 添加点击事件 用法

```

	
//单指单击
UITapGestureRecognizer *singleFingerOne = [[UITapGestureRecognizer alloc] initWithTarget:self
	action:@selector(handleSingleFingerEvent:)];
	singleFingerOne.numberOfTouchesRequired = 1; //手指数
	singleFingerOne.numberOfTapsRequired = 1; //tap次数
	singleFingerOne.delegate = self;

//单指双击
UITapGestureRecognizer *singleFingerTwo = [[UITapGestureRecognizer alloc] initWithTarget:self
action:@selector(handleSingleFingerEvent:)];
 singleFingerTwo.numberOfTouchesRequired = 1;
singleFingerTwo.numberOfTapsRequired = 2;
singleFingerTwo.delegate = self;

//双指单击
 UITapGestureRecognizer *doubleFingerOne = [[UITapGestureRecognizer alloc] initWithTarget:self
 action:@selector(handleDoubleFingerEvent:)];
 doubleFingerOne.numberOfTouchesRequired = 2;
 doubleFingerOne.numberOfTapsRequired = 1;
 doubleFingerOne.delegate = self;

 UITapGestureRecognizer *doubleFingerTwo = [[UITapGestureRecognizer alloc] initWithTarget:self
    action:@selector(handleDoubleFingerEvent:)];
 doubleFingerTwo.numberOfTouchesRequired = 2;
 doubleFingerTwo.numberOfTapsRequired = 2;
 doubleFingerTwo.delegate = self;

 //如果不加下面的话，当单指双击时，会先调用单指单击中的处理，再调用单指双击中的处理
 [singleFingerOne requireGestureRecognizerToFail:singleFingerTwo];
 //同理双指亦是如此
 [doubleFingerOne requireGestureRecognizerToFail:doubleFingerTwo];

 [self.view addGestureRecognizer:singleFingerOne];
 [self.view addGestureRecognizer:singleFingerTwo];
 [self.view addGestureRecognizer:doubleFingerOne];
 [self.view addGestureRecognizer:doubleFingerTwo];

 [singleFingerOne release];
 [singleFingerTwo release];
 [doubleFingerOne release];
 [doubleFingerTwo release];
// 处理事件的方法，代码：
//处理单指事件
- (void)handleSingleFingerEvent:(UITapGestureRecognizer *)sender
 {
 if (sender.numberOfTapsRequired == 1) {
 //单指单击
 NSLog(@"单指单击");
 }else if(sender.numberOfTapsRequired == 2){
 //单指双击
 NSLog(@"单指双击");
 }
 }
 //处理双指事件
 - (void)handleDoubleFingerEvent:(UITapGestureRecognizer *)sender
 {
 if (sender.numberOfTapsRequired == 1) {
 //双指单击
 NSLog(@"双指单击");
 }else if(sender.numberOfTapsRequired == 2){
 //双指双击
 NSLog(@"双指双击");
 }
 };
	
	
```