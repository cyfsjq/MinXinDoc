#通讯录集成指引
##1.引入头文件
```objc
#import <MXKit/MXContacts.h>
```
##2.获取通讯录的viewController
```objc
//定义
-(UIViewController *)getContactsViewController;
//示例
UIViewController *chat = [[MXContacts sharedInstance] getContactsViewController];
//你可以push这个viewController
```
##3.通讯录模块的详细api使用
###获取人员信息，并显示敏行内置的人员信息详情页
```objc
//定义
//loginName 是要获取的人员的登录名
//VC 是将要push聊天view的viewController
-(void)personInfo:(NSString *)loginName withIndicator:(BOOL)showIndicator withViewController:(UIViewController *)VC;
//示例
[[MXContacts sharedInstance] personInfo:@"zhangsan" withIndicator:YES withViewController:vc];
```
###获取人员信息，结果放在callback中返回
```objc
//定义
//loginName 是要获取的人员的登录名
-(void)getPersonInfo:(NSString *)loginName withIndicator:(BOOL)showIndicator withCallback:(finishCallback)callback;
//示例
[[MXContacts sharedInstance] getPersonInfo:@"zhangsan" withIndicator:YES withCallback:^(id result, MXError *error){
	//这里的result是一个NSDictionary;
   NSLog(@"zhangsan的个人信息 = %@", result);
}];
```
