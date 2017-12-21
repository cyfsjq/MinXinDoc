#IM集成指引
##1.引入头文件
```objc
#import <MXKit/MXChat.h>
```
##2.获取IM的viewController
```objc
//定义
-(UIViewController *)getChatViewController;
//示例
UIViewController *chat = [[MXChat sharedInstance] getChatViewController];
//你可以push这个viewController
//设置导航栏
UINavigationController *chatNav=[[UINavigationController alloc]initWithRootViewController:chat];
```
##3.IM模块的详细api使用
###如果IM首页是在在UITabbarItem上需要设置tabbar的高度
```objc
//定义
-(void)setTabbarHeight:(int)tabbarHeight;
//示例
[[MXChat sharedInstance] setTabbarHeight:tabbarHeight];
```
###发起聊天，自带通讯录选人界面
```objc
//定义
-(void)startChat;
//示例
[[MXChat sharedInstance] startChat];
```
###发起聊天，不带通讯录选人界面
```objc
//定义
//userArray 是用户的登录名的数组，不包括自己
//VC 是将要push聊天view的viewController
-(void)chat:(NSArray *)userArray withViewController:(UIViewController *)VC withFailCallback:(finishCallback)callback;
//示例
NSArray *userArr = [[NSArray alloc] initWithObjects:@"zhangsan",@"lisi",@"wangwu", nil];
[[MXChat sharedInstance] chat:userArr withViewController:vc withFailCallback:^(id object, MXError *error) {
        NSLog(@"eror == %@", error.description);
    }];

```
###注册消息未读数变化通知的接口
```objc
//定义
-(void)registUnreadMsgCountCallback:(finishCallback)callback;
//示例
//设置tabbaritem上面的消息未读数
[[MXChat sharedInstance] registUnreadMsgCountCallback:^(id result, MXError *error){
   NSLog(@"unread Message count = %@", result);
        if([result isEqualToString:@"0"])
        {
            [tabBarController.tabBar.items[0] setBadgeValue:nil];
        }
        else
        {
            [tabBarController.tabBar.items[0] setBadgeValue:[NSString stringWithFormat:@"%@", result]];
        }
}];
```
