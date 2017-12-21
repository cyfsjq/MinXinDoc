#工作圈集成指引
##1.引入头文件
```objc
#import <MXKit/MXCircle.h>
```
##2.获取工作圈的viewController
```objc
//定义
-(UIViewController *)getCircleViewController;
//示例
UIViewController *chat = [[MXCircle sharedInstance] getCircleViewController];
//你可以push这个viewController
```
##3.工作圈模块的详细api使用
###如果工作圈首页是在在UITabbarItem上需要设置tabbar的高度
```objc
//定义
-(void)setTabbarHeight:(int)tabbarHeight;
//示例
[[MXCircle sharedInstance] setTabbarHeight:tabbarHeight];
```
###显示工作圈列表
```objc
//定义
//这个api会把工作圈列表显示在你想要显示的地方，从button开始显示，显示在哪个view里
-(void)showGroupTableViewFromButton:(UIButton *)button InView:(UIView *)view;
//示例
//groupChange 当显示群组的button按下的时候触发的事件
-(void) groupChange:(id)sender {
    UIButton *btn = (UIButton*)sender;
    if (!btn.selected) {
        [[MXCircle sharedInstance] showGroupTableViewFromButton:btn InView:self];
    }
}
```
###注册工作圈切换的callback
```objc
//定义
-(void)registGroupSelect:(finishCallback)callback;
//示例
[[MXCircle sharedInstance] registGroupSelect:^(id result, MXError *error){
   //result里面是title的名字
   //设置切换后title的名字
}];
```
###新建工作圈消息
```objc
//定义
//messageType是工作圈的类型
/*
typedef enum {
    MESSAGE_TYPE_TEXT,//文本
    MESSAGE_TYPE_TASK,//待办
    MESSAGE_TYPE_ACTIVITY,//活动
    MESSAGE_TYPE_POLL,//投票
    MESSAGE_TYPE_ANNOUNCE,//公告
} MESSAGE_TYPE;
*/
//remoteUrl现在传入nil，将来做扩展用
//title是新建的工作圈消息的title
-(void) createNewMessageWithType:(int) messageType remote:(NSString*)remoteUrl title:(NSString*)titles;
//示例
//这个方法是说单击各个新建菜单触发的行为
-(void)selectedMenuItemAppVo:(MXAppsVO *)appVo
{
    MESSAGE_TYPE type;
    MXCircle *circle = [MXCircle sharedInstance];
    if([appVo.name isEqualToString: @"update"])
    {
        type = MESSAGE_TYPE_TEXT;
        [circle createNewMessageWithType:type remote:nil title:appVo.text];

        return;
    }
    if([appVo.name isEqualToString:@"mini_task"])
    {
        type = MESSAGE_TYPE_TASK;
        [circle createNewMessageWithType:type remote:nil title:appVo.text];
        return;
    }
    if([appVo.name isEqualToString:@"event"])
    {
        type = MESSAGE_TYPE_ACTIVITY;
        [circle createNewMessageWithType:type remote:nil title:appVo.text];
        return;
    }
    if([appVo.name isEqualToString:@"poll"])
    {
        type = MESSAGE_TYPE_POLL;
        [circle createNewMessageWithType:type remote:nil title:appVo.text];
        return;
    }
}
```
###注册工作圈有新消息的callback通知
```objc
//定义
-(void)registGroupRedIcon:(finishCallback)callback;
//示例
[[MXCircle sharedInstance] registGroupRedIcon:^(id result, MXError *error) {
	//显示工作圈新消息提示，比如出现新消息的小红点儿
   NSLog(@"show circel new message alert === %d", [result boolValue]);
}];
```
###发布消息到工作圈
```objc
//定义
//type 类型如下
/*
typedef enum {
    MESSAGE_TYPE_TEXT,//文本
    MESSAGE_TYPE_TASK,//待办
    MESSAGE_TYPE_ACTIVITY,//活动
    MESSAGE_TYPE_POLL,//投票
    MESSAGE_TYPE_ANNOUNCE,//公告
} MESSAGE_TYPE;
*/
//content是消息的内容
//VC是要push这个新建工作圈消息的view的viewController
-(void)sendToCircle:(int)type withContent:(NSString *)content withViewController:(UIViewController *)VC;
//示例
[[MXCircle sharedInstance] sendToCircle: MESSAGE_TYPE_TEXT withContent:@"测试新建文本消息" withViewController:vc];
```
