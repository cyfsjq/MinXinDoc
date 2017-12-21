#应用中心集成指引
##1.引入头文件
```objc
#import <MXKit/MXAppCenter.h>
```
##2.获取应用中心的viewController
```objc
//定义
-(UIViewController *)getChatViewController;
//示例
UIViewController *chat = [[MXAppCenter sharedInstance] getAppCenterViewController];
//你可以push这个viewController
```
##3.应用中心模块的详细api使用
###注册应用中心编辑开始的callback
```objc
//定义
//编辑模式启动的时候，用户可以对应用进行排序
-(void)registEditBeginCallback:(finishCallback)callback;
//示例
[[MXAppCenter sharedInstance] registEditBeginCallback:^(id result, MXError *error) {
	NSLog(@"应用中心编辑模式启动");
}];
```
###注册应用中心编辑结束的callback
```objc
//定义
-(void)registEditEndCallback:(finishCallback)callback;
//示例
[[MXAppCenter sharedInstance] registEditEndCallback:^(id result, MXError *error) {
	NSLog(@"应用中心编辑模式结束");
}];
```
###设置应用中心各个应用的消息未读数
```objc
//定义
//countDic 里面存的是appID和未读数的key value
-(void)showBadgeCount:(NSDictionary *)countDic;
//示例
NSDictionary *dicCount = [[NSDictionary alloc] initWithObjectsAndKeys:[NSNumber numberWithInt:20],@"bde209e19cfebcfa2f719a52a4406441",[NSNumber numberWithInt:15],@"f3710a8b00b4924057446b53609b5038", nil];
[[MXAppCenter sharedInstance] showBadgeCount:dicCount];
```
###设置应用中心某个应用隐藏
```objc
//定义
//appsArray里面存得是appID的数组
-(void)hiddenApps:(NSArray *)appsArray;
//示例
[[MXAppCenter sharedInstance] hiddenApps:[NSArray arrayWithObjects:@"f3710a8b00b4924057446b53609b5038", nil]];
```
