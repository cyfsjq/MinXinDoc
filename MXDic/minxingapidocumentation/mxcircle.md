# MXCircle接口说明文档
## 1.引入头文件
```objc
#import <MXKit/MXCircle.h>
```
## 2.初始化MXCircle
### 2.1获取MXCircle对象
```objc
定义：
+(MXCircle *)sharedInstance;
示例代码：
[MXCircle sharedInstance];
```
### 2.2获取工作圈的ViewController
```objc
定义：
-(UIViewController *)getCircleViewController;
示例代码：
UIViewController *workCircle = [[MXCircle sharedInstance] getCircleViewController];
```
### 2.3自定义工作圈的headerView
```objc
定义：
-(void)setCustomHeaderView:(UIView *)headerView;
示例代码：
//如果是用tabbar加载circle那么就需要设置tabbar的高度
[[MXCircle sharedInstance] setTabbarHeight:tabbarHeight];
[[MXCircle sharedInstance] setCustomHeaderView:circleHeader];

```
### 释放工作圈的viewController
```objc
定义：
-(void)cleanCircleViewController;
示例代码：
[[MXCircle sharedInstance] cleanCircleViewController];
```
## 3.MXCircle模块详细API使用说明
### 获取当前工作圈信息
```objc
定义：
-(NSDictionary *)getCurrentGroupDic;
示例代码：
NSDictionary *groupDic = [[MXCircle sharedInstance] getCurrentGroupDic];
```
### 右滑circle界面时的回调
```objc
定义：
-(void)startPullDownWithCallback:(finishCallback)callback;
示例代码：
[[MXCircle sharedInstance] startPullDownWithCallback:^(id result, MXError *error) {
    if (result) {
    [circleHeader startPullDown];
  }
}];
```
### 注册工作圈群组切换的回调
```objc
定义：
-(void)registGroupSelect:(finishCallback)callback;
示例代码：
[[MXCircle sharedInstance] registGroupSelect:^(id result, MXError *error){
  //result里面是title的名字
 [circleHeader updateGroupTitle:[(NSDictionary *)result objectForKey:@"groupName"]];
 [circleHeader popupGroupSwitcherDismissed];
}];
```
### 显示工作圈群组列表，这个api会把工作圈列表显示在你想要显示的地方，从button开始显示，显示在哪个view里
```objc
参数说明：
button：当前点击的按钮
view：当前视图
定义：
-(void)showGroupTableViewFromButton:(UIButton *)button InView:(UIView *)view;
示例代码：
//groupChange 当显示群组的button按下的时候触发的事件
-(void) groupChange:(id)sender {
  UIButton *btn = (UIButton*)sender;
  if (!btn.selected) {
[[MXCircle sharedInstance] showGroupTableViewFromButton:btn InView:self];
  }
}
```
### 工作圈群组列表消失
```objc
定义:
-(void)dissmissGroupTableView;
示例代码：
[[MXCircle sharedInstance]dissmissGroupTableView];
```
### 注册工作圈有新消息的callback通知
```objc
定义：
-(void)registGroupRedIcon:(finishCallback)callback;
示例代码：
[[MXCircle sharedInstance] registGroupRedIcon:^(id result, MXError *error) {
  //显示工作圈新消息提示，比如出现新消息的小红点儿
  NSLog(@"show circel === %d", [result boolValue]);
  unreadMessageView.hidden = ![result boolValue];
```
### 初始化的时候去获取是否应该显示工作圈的红点儿
```objc
定义：
-(BOOL)getCircleRedIcon;
示例代码：
BOOL showRedIcon = [[MXCircle sharedInstance] getCircleRedIcon];
```
### 新建工作圈消息
```objc
参数说明：
messageType是工作圈的类型
typedef enum {
    MESSAGE_TYPE_TEXT,//文本
    MESSAGE_TYPE_TASK,//待办
    MESSAGE_TYPE_ACTIVITY,//活动
    MESSAGE_TYPE_POLL,//投票
    MESSAGE_TYPE_ANNOUNCE,//公告
} MESSAGE_TYPE;
remoteUrl： 现在传入nil，将来做扩展用
title： 新建的工作圈消息的title
定义：
-(void) createNewMessageWithType:(int) messageType remote:(NSString *)remoteUrl title:(NSString *)titles;
示例代码：
//这个方法是说单击各个新建菜单触发的行为
-(void)selectedMenuItemAppVo:(MXAppsVO *)appVo{
    MESSAGE_TYPE type;
    MXCircle *circle = [MXCircle sharedInstance];
    if([appVo.name isEqualToString: @"update"]){
        type = MESSAGE_TYPE_TEXT;
        [circle createNewMessageWithType:type remote:nil title:appVo.text];
        return;
    }
    if([appVo.name isEqualToString:@"mini_task"]){
        type = MESSAGE_TYPE_TASK;
        [circle createNewMessageWithType:type remote:nil title:appVo.text];
        return;
    }
    if([appVo.name isEqualToString:@"event"]){
        type = MESSAGE_TYPE_ACTIVITY;
        [circle createNewMessageWithType:type remote:nil title:appVo.text];
        return;
    }
    if([appVo.name isEqualToString:@"poll"]){
        type = MESSAGE_TYPE_POLL;
        [circle createNewMessageWithType:type remote:nil title:appVo.text];
        return;
    }
}
```
### 发布消息到工作圈
```objc
参数说明：
type 类型如下
typedef enum {
    MESSAGE_TYPE_TEXT,//文本
    MESSAGE_TYPE_TASK,//待办
    MESSAGE_TYPE_ACTIVITY,//活动
    MESSAGE_TYPE_POLL,//投票
    MESSAGE_TYPE_ANNOUNCE,//公告
} MESSAGE_TYPE;
content：是消息的内容
VC：是要push这个新建工作圈消息的view的viewController
定义：
-(void)sendToCircle:(int)type withContent:(NSString *)content withViewController:(UIViewController *)VC;
示例代码：
[[MXCircle sharedInstance] sendToCircle: MESSAGE_TYPE_TEXT withContent:@"测试新建文本消息" withViewController:vc];
```
### 分享到工作圈接口
```objc
参数说明：
title:标题
description：详细描述
url：http链接地址
nativeURL：native链接地址
thumbnailURL：缩列图链接地址
VC：分享成功push到的页面
定义：
-(void)shareTitle:(NSString *)title withDescription:(NSString *)description withURL:(NSString *)url withNativeURL:(NSString *)nativeURL withThumbnailURL:(NSString *)thumbnailURL withViewController:(UIViewController *)VC;
示例代码：
[[MXCircle sharedInstance] shareTitle:@"test" withDescription:@"test" withURL:@"https://www.baidu.com" withNativeURL:nil withThumbnailURL:@"http://img1.gtimg.com/13/1301/130137/13013760_980x1200_0.jpg" withViewController:self];
```
### 分享到工作圈接口，不带UI, 分享的结果在callback里回调，成功返回@"success"， 失败，返回errorstring
```objc
参数说明：
title:标题
description:详细描述
url：http链接地址
nativeURL：native链接地址
thumbnailURL：缩列图链接地址
groupID:工作圈ID
callback:分享后的回调
定义：
-(void)shareTitle:(NSString \*)title withDescription:(NSString \*)description withURL:(NSString \*)url withNativeURL:(NSString \*)nativeURL withThumbnailURL:(NSString \*)thumbnailURL withGroupID:(int)groupID withCallback:(finishCallback)callback;
示例代码：
[[MXCircle sharedInstance] shareTitle:@"test" withDescription:@"test" withURL:@"www.baidu.com" withNativeURL:nil withThumbnailURL:@"http://img1.gtimg.com/13/1301/130137/13013760_980x1200_0.jpg" withGroupID:163 withCallback:^(id object, MXError *error) {
  if (object) { 
  UIAlertView *alter = [[UIAlertView alloc] initWithTitle:@"信息" message:object delegate:nil cancelButtonTitle:@"OK" otherButtonTitles:nil];
    [alter show];
   }
}];
```
### 设置显示指定的工作圈
```objc
定义：
//获取已加入的工作圈列表,遍历数组，获得字典，每一个字典里包含groupID和groupName的信息
-(NSArray *)getJoinedGroupArray;
//通过传入groupID参数切换工作圈
-(void)setCurrentGroup:(int)groupID;
//强制刷新工作圈，按照已经设置的currentgroupid来刷新
-(void)froceRefrushCircle;
示例代码：
NSArray * array = [[MXCircle sharedInstance] getJoinedGroupArray];
NSDictionary * dic=array[0];
int groupID=array[0]@["groupID"];
[[MXCircle sharedInstance]setCurrentGroup:groupID];
[[MXCircle sharedInstance]froceRefrushCircle];
```
### 获得话题列表
```objc
定义：
-(UIViewController *)getTopicViewController;
示例代码：
UIViewController * vc = [[MXCircle sharedInstance] getTopicViewController];
```
### 通过groupID设置当前群组
```objc
定义：
-(void)setCurrentGroup:(int)groupID;
示例代码：
[[MXCircle sharedInstance]setCurrentGroup:45]; 
```
### 强制刷新工作圈，按照已经设置的currentgroupid来刷新
```objc
定义：
-(void)froceRefrushCircle;
示例代码：
[[MXCircle sharedInstance]froceRefrushCircle];
```
### 启动不限制点赞人数显示
```objc
定义：
-(void)initDisableLimitShowPrise:(BOOL)disableLimitShowPrise;
示例代码：
//启动不限制点赞人数显示.  YES为完整显示  NO为不完整显示
[[MXCircle sharedInstance] initDisableLimitShowPrise:MX_DISABLE_LIMITSHOWPRAISE];
```
### 设置分享接口
```objc
定义：
-(void)setShareToXXX:(NSString *)share2XXXUrl;
示例代码：
[[MXCircle sharedInstance] setShareToXXX:url];
```
### 可配置工作圈弹出的横条(赞、回复、分享等)
```objc
定义：
-(void)setAction:(NSArray *)action;
示例代码：
#ifdef MX_Circle_Action
NSData *data = [MX_Circle_Action dataUsingEncoding:NSUTF8StringEncoding];
NSArray *arr = nil;
if (data) {
arr = [NSJSONSerialization JSONObjectWithData:data options:kNilOptions error:nil];
NSLog(@"dic ============   %@",arr);
}
[[MXCircle sharedInstance] setAction:arr];
[MXObj registCustomActionCallback:^(id object, MXError *error) {

}];
#endif
```

















