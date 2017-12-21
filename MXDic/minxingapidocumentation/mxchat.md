# MXChat接口说明文档
## 1.引入头文件
```objc
#import <MXKit/MXChat.h>
```
## 2.初始化MXChat
### 2.1获取MXChat对象
```objc
定义：
+(MXChat*)sharedInstance;
示例代码：
[MXChat sharedInstance];
```
### 2.2获取聊天的ViewController
```objc
定义：
-(UIViewController *)getChatViewController;
示例代码：
UIViewController *chat = [[MXChat sharedInstance] getChatViewController];
```
### 2.3自定义聊天导航栏视图
```objc
定义：
-(void)setCustomHeaderView:(UIView *)headerView;
示例代码：
MXChatViewHeader *chatHeader = [[MXChatViewHeader alloc] initWithFrame:headerFrame];
//如果聊天view带tabbar， 需要设置tabbarheight
[[MXChat sharedInstance] setTabbarHeight:tabbarHeight];
[[MXChat sharedInstance] setCustomHeaderView:chatHeader];
```
### 2.4释放聊天的viewcontroller
```objc
定义：
-(void)cleanChatViewController;
示例代码：
[[MXChat sharedInstance] cleanChatViewController];
```
## 3.MXChat模块详细API使用说明
### 右滑chat界面时的回调
```objc
定义：
-(void)startPullDownWithCallback:(finishCallback)callback;
示例代码：
[[MXChat sharedInstance] startPullDownWithCallback:^(id result, MXError *error) {
  if (result) {
  [chatHeader startPullDown];
    }
}];
```
### 注册通讯录名片插件
```objc
定义：
-(void)registbussnissCard;
示例代码：
[[MXChat sharedInstance] registbussnissCard];
```
### 注册红包插件
```objc
定义：
-(void)registRedPacket;
示例代码：
[[MXChat sharedInstance] registRedPacket];
```
### 注册文件共享
```objc
定义：
- (void)registDocView;
示例代码：
[[MXChat sharedInstance] registDocView];
```
### 注册健步走插件
```objc
定义：
-(void)registWalking;
示例代码：
[[MXChat sharedInstance] registWalking];
```
### 发起聊天，自带通讯录选人界面
```objc
定义：
-(void)startChat;
示例代码：
[[MXChat sharedInstance] startChat];
```
### 发起聊天，不带通讯录选人界面
```objc
参数说明：
userArray: 是用户的登录名的数组，不包括自己
VC: 是将要push聊天view的viewController
定义：
-(void)chat:(NSArray *)userArray withViewController:(UIViewController *)VC withFailCallback:(finishCallback)callback;
示例代码：
NSArray *userArr = [[NSArray alloc] initWithObjects:@"zhangsan",@"lisi",@"wangwu", nil];
[[MXChat sharedInstance] chat:userArr withViewController:vc withFailCallback:^(id object, MXError *error) {
  NSLog(@"eror == %@", error.description);
}];
```
### 分享到聊天接口
```objc
参数说明：
title：标题
description：详细描述
url：链接地址
nativeURL：appurl，可为空
thumbnailURL：缩略图地址
VC：传入控制器参数，用于present到本地群聊页面，可为空
定义：
-(void)shareTitle:(NSString *)title withDescription:(NSString *)description withURL:(NSString *)url withNativeURL:(NSString *)nativeURL withThumbnailURL:(NSString *)thumbnailURL withViewController:(UIViewController *)VC;
示例代码：
[[MXChat sharedInstance] shareTitle:@"112233" withDescription:@"ee33" withURL:@"https://www.baidu.com" withNativeURL:nil withThumbnailURL:@"https://ss3.baidu.com/-fo3dSag_xI4khGko9WTAnF6hhy/super/whfpf%3D425%2C260%2C50/sign=08ba595e4fed2e73fcbcd56ce13c95b9/d000baa1cd11728b79d03af8cdfcc3cec2fd2c6e.jpg" withViewController:self];
```
### 收藏message
```objc
参数说明：
messageID：消息Id(message_id)
title: 标题
url： 网址
appUrl：app地址
定义：
-(void)addFavoriteToServer:(int)messageID withTitle:(NSString *)title withUrl:(NSString *)url withAppUrl:(NSString *)appUrl;
示例代码：
[[MXChat sharedInstance] addFavoriteToServer:messageId withTitle:nil withUrl:nil withAppUrl:nil];
```
### 插件消息分享到聊天的接口
```objc
参数说明：
dataString： 消息内容
VC： 传入控制器参数，用于present到选人页面
定义：
-(void)sharePluginMessage:(NSString *)dataString withViewController:(UIViewController *)VC;
示例代码：
[[MXChat sharedInstance] sharePluginMessage:_bodyString withViewController:self];
```
### 进入详情资料
```objc
定义：
-(void)goToDetail;
示例代码：
[[MXChat sharedInstance] goToDetail];
```
### 进入赞我的朋友界面
```objc
定义：
-(void)goToLike;
示例代码：
[[MXChat sharedInstance] goToLike];
```
### 添加联系人
```objc
定义：
-(void)addContact;
示例代码：
[[MXChat sharedInstance] addContact];
```
### 发布工作圈的消息
```objc
定义：
-(void)shareJob;
示例代码：
[[MXChat sharedInstance] shareJob];
```
### 注册未读消息数变化的回调，用来更新聊天消息数
```objc
定义：
-(void)registUnreadMsgCountCallback:(finishCallback)callback;
示例代码：
[[MXChat sharedInstance] registUnreadMsgCountCallback:^(id result, MXError *error){
 NSLog(@"unread Message == %@", result);
 int messageIndex = [self getMessageIndex];
 if([result isEqualToString:@"0"])
 {
   [tabBarController.tabBar.items[messageIndex] setBadgeValue:nil];
 }else{
    [tabBarController.tabBar.items[messageIndex] setBadgeValue:[NSString stringWithFormat:@"%@", result]];
    }
}];
```
### 主动获取未读消息数
```objc
定义：
-(int)getUnreadMessageCount;
示例代码：
int uncount=[MXChat sharedInstance] getUnreadMessageCount];
```
### 注册收到邀请通知后点击跳转到指定群组的回调-
```objc
定义：
-(void)registSwitchGroupCallback:(finishCallback)callback;
示例代码：
[[MXChat sharedInstance] registSwitchGroupCallback: ^(id result, MXError *error){
  NSLog(@"switch group");
 [tabBarController setSelectedIndex:[self getCircleIndex]];
 }];
```
### 注册点击聊天cell的回调
```objc
定义：
-(void)registChatDidSelectRowCallback:(finishCallback)callback;
示例代码：
[[MXChat sharedInstance] registChatDidSelectRowCallback:^(id result, MXError *error) {
  if (result) {
   //点击聊天的cellback 写隐藏tabbar的方法
  }
}];
```
### 注册聊天列表上方显示收取中的回调
```objc
定义：
-(void)registChatReceivingIndicatorCallback:(finishCallback)callback;
示例代码：
[[MXChat sharedInstance] registChatReceivingIndicatorCallback:^(id result, MXError *error) {
    BOOL hidden = [result boolValue];
    [chatHeader setNavBarTitleHidden:!hidden];
    [chatHeader setNavBarTitleIndicatorHidden:hidden];
}];
```
### 注册chatViewWillAppear的回调
```objc
定义：
-(void)registViewWillAppearCallback:(finishCallback)callback;
示例代码：
[[MXChat sharedInstance] registViewWillAppearCallback:^(id result, MXError *error) {
  NSLog(@"list view will appear");
}];
```
### 注册聊天会话点back之后的行为，如果不注册这个callback，那么就是默认行为，直接返回到上一个view
```objc
定义：
-(void)registChatBackAction:(finishCallback)callback;
示例代码：
[[MXChat sharedInstance] registChatBackAction:^(id result, MXError *error) {
 NSLog(@"chat back action callback called");
[self.tabBarController setSelectedIndex:[self getMessageIndex]];
//如果是通讯里里面直接点人发起的聊天，通讯录应该回到最顶层，用户可以自定义这个行为
 [navAddressBook popToRootViewControllerAnimated:YES];
 [navWorkCircle popToRootViewControllerAnimated:YES];
 [navAppCenter popToRootViewControllerAnimated:YES];
 [navChat popToRootViewControllerAnimated:YES];
}];
```
### 注册用Api调用聊天页面会话页面点back的行为，不注册默认回到上一个view
```objc
定义：
-(void)registApiChatBackAction:(finishCallback)callback;
示例代码：
[[MXChat sharedInstance] registApiChatBackAction:^(id result, MXError *error) {
        
}];
```
### 删除指定ID的message
```objc
定义：
-(BOOL)deleteMsg:(int)messageID;
示例代码：
[[MXChat sharedInstance]deleteMsg:messageID];
```
### 自定义会话conversation
```objc
参数说明：
customKey：自定义会话的key
title：会话标题
content：会话内容
avatarUrl：头像地址
updateTime：server发出消息的时间，"2015-11-19T12:08:50+08:00"
count：标记数
displayOrder：显示顺序
alert：显示标记数，YES是显示标记数，NO是显示红点
定义：
-(void)updateCustomConversation:(NSString *)customKey
                      withTitle:(NSString *)title
                    withContent:(NSString *)content
                     withAvatar:(NSString *)avatarUrl
                 withUpdateTime:(NSString *)updateTime  
                withBadegeCount:(int)count
               withDisplayOrder:(int)displayOrder
                      withAlert:(BOOL)alert;
示例代码：
[[MXChat sharedInstance] updateCustomConversation:@"test_custom_key" withTitle:@"testTitle" withContent:@"testContent" withAvatar:@"/photos/56dcd11e0de1756f0eb4ef3d4644307a" withUpdateTime:@"2015-11-19T12:08:50+08:00" withBadegeCount:10 withDisplayOrder:0 withAlert:YES];
```
### 注册自定义会话点击事件的回调
```objc
定义：
-(void)registCustomConversationClickCallback:(finishCallback)callback;
示例代码：
[[MXChat sharedInstance] registCustomConversationClickCallback:^(id result, MXError *error) {
        if(!error) {
           NSDictionary *resultDic = (NSDictionary *)result;
           NSString *custom_key = [resultDic objectForKey:@"custom_key"];
         if(custom_key) {
           NSLog(@"custom_key == %@", custom_key);
           //test push view
          MXSettingViewController *vc = [[MXSettingViewController alloc] init];
           vc.hidesBottomBarWhenPushed = YES;
           [[[MXChat sharedInstance] getChatViewController].navigationController pushViewController:vc animated:YES];
            }
        }
}];
```
### 通过appID打开会话，例如通过意见反馈
```objc
定义：
-(void)openConversationByOpenID:(NSString *)openID withNavigationVC:(UINavigationController *)navVC;
示例代码：
int index =  [self.tabBarController selectedIndex];
UINavigationController *nav = [self.tabBarController.viewControllers objectAtIndex:index];
[[MXChat sharedInstance] openConversationByOpenID:MX_FEEDBACK_APP_ID withNavigationVC:nav];
```
### 通过会话ID删除所选会话
```objc
参数说明：
conversationID：会话ID
定义：
-(void)deleteSelectConversation:(long int)conversationID;
示例代码：
[[MXChat sharedInstance]deleteSelectConversation:1];
```
### 通过会话ID获取会话信息
```objc
定义：
-(NSDictionary *)getConversationByID:(long int)conversationID;
示例代码：
NSDictionary *dic=[[MXChat sharedInstance]getConversationByID:1];
```
### 通过消息ID获取消息信息
```objc
定义：
-(NSDictionary *)getMssagesByID:(int)messageID;
示例代码：
NSDictionary *dic=[[MXChat sharedInstance]getMssagesByID:1];
```
### 配置聊天详情的设置按钮
```objc
定义：
-(void)disableChatSetting:(BOOL)disable;
示例代码：
[[MXChat sharedInstance] disableChatSetting:YES];
```
### 发送自定义视频消息
```objc
参数说明：
conversationVO：会话信息字典
key：自定义视频插件的key
定义：
- (void)sendCustomVideoChatMessageWithConversation:(NSDictionary *)conversationVO key:(NSString *)key;
示例代码：
NSDictionary *conversationDic = [NSMutableDictionary new];[conversationDic setValue:@(self.conversationID) forKey:@"conversation_id"];
[conversationDic setValue:@(self.theConferenceMode) forKey:@"multi_user"];
[conversationDic setValue:@[@([self.online_persons firstObject].ID)] forKey:@"user_ids"]; 
。。。。。。       
[[MXChat sharedInstance] sendCustomVideoChatMessageWithConversation:conversationDic key:@"chat_video_status"];
```
### 聊天列表页自动获取下一个未读数消息(双击tabbar)
```objc
定义：
-(void) autoScroll2UnreadMessage;
示例代码：
[[MXChat sharedInstance] autoScroll2UnreadMessage];
```
### 敏信列表下拉刷新触发后会回调这个callback
```objc
定义：
-(void)registChatRefreshCallback:(functionCallback)callback;
示例代码：
[[MXChat sharedInstance]registChatRefreshCallback:^(id object) {
// 执行敏信列表下拉刷新后的操作       
}];
```
### 敏信列表刷新完毕需要调用这个方法来更新下拉的状态
```objc
定义：
-(void)chatRefreshFinished;
示例代码：
[[MXChat sharedInstance]chatRefreshFinished];
```
### 注册聊天顶部banner的对象
```objc
定义：
//聊天窗口顶部预留扩展位，方便添加宣传内容或接入广告
-(void)registBannerObj:(id)bannerObj;
示例代码：
testConversationBannerObject *bannerObj = [[testConversationBannerObject alloc] init];
[[MXChat sharedInstance] registBannerObj:bannerObj];
```
### 移除聊天顶部banner的对象
```objc
定义：
-(void)removeBannerView;
示例代码：
[[MXChat sharedInstance] removeBannerView];
```
### 注册自定义聊天插件(聊天附件栏显示的插件应用)
```objc
定义：
-(void)registCutsomPlugIn:(id)plugIn;
示例代码：
//添加视频会议
MXVideoPluginObject *mvc=[[MXVideoPluginObject alloc] init];
[[MXChat sharedInstance] registCutsomPlugIn:mvc];
```
### 自定义消息，可以在群聊附件栏启动应用，发送链接，发送本地通讯录的名片等。需实现协议方法如下：
#### 必须实现的协议方法
```objc
定义：
//获取聊天应用图标
-(UIImage *)getIconImage;
示例代码：
-(UIImage *)getIconImage {
   return [UIImage imageNamed:@"Icon-72"];
}
定义：
//获取聊天应用名字
-(NSString *)getName;
示例代码：
-(NSString *)getName {
    return @"test";
}
定义：
//获取聊天应用的key
-(NSString *)getPlugInKey;
示例代码：
-(NSString *)getPlugInKey {
    return @"test_plugin";
}
定义：
//聊天应用发起聊天
-(void)startAction:(NSString *)conversationJsonData withReCallback:(plugInReCallback)recallback;
示例代码：
-(void)startAction:(NSString *)conversationJsonData withReCallback:(plugInReCallback)recallback
{
    NSString *testData = @"testData";
    recallback(testData);
}
定义：
//获取插件视图
-(UIView *)getPlugInViewByData:(NSString *)data;
示例代码：
-(UIView *)getPlugInViewByData:(NSString *)data
{
UIView *bgView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 200, 80)];
......
return bgView;
}
定义：
//聊天页里点击插件消息
-(void)clickAction:(NSString *)data withView:(UIView *)view andPopViewController:(UIViewController *)viewC;
示例代码：
-(void)clickAction:(NSString *)data withView:(UIView *)view andPopViewController:(UIViewController *)viewC{
    
}
定义：
//接受消息的背景图片
-(UIImage *)getIncomingImage;
示例代码：
-(UIImage *)getIncomingImage{
    UIImage *image = [UIImage imageNamed:@"mx_bg_share_others_chat_phone"];
    return image;
}
定义：
//发消息的背景图片
-(UIImage *)getOutGoingImage;
示例代码：
-(UIImage *)getOutGoingImage{
    UIImage *image = [UIImage imageNamed:@"mx_bg_share_my_chat_phone"];
    return image;
}
定义：
//是否在附件栏显示
- (BOOL)showInAttachUtilView;
示例代码：
- (BOOL)showInAttachUtilView{
return YES;
}
```
#### 可选择实现的协议方法
```objc
定义：
- (NSString *)getPluginMessgeType;
示例代码:
-(NSString *)getPluginMessgeType {
    return @"test";
}
定义：
- (BOOL)showInMiddle;
示例代码：
- (BOOL)showInMiddle{
return YES;
}
定义：
- (CGSize)getPluginViewSizeByData:(NSString *)data;
示例代码：
- (CGSize)getPluginViewSizeByData:(NSString *)data{
CGSize size=......;
return size;
}
定义：
- (BOOL)hiddenFromLastSeenData;
示例代码：
- (BOOL)hiddenFromLastSeenData{
return YES;
}
定义：
- (NSDictionary *)getSenderIDByBody:(NSString *)body;
示例代码：
- (NSDictionary *)getSenderIDByBody:(NSString *)body{
return dic;
}
定义：
//红包插件专用的方法，现在用于扩展选中、取消背景色的方法，字典里含有type（incomingcell、outgoingcell）、bubble
- (void)HandlePluginByDic:(NSDictionary *)dic;
示例代码：
- (void)HandlePluginByDic:(NSDictionary *)dic{
}
定义：
- (BOOL)isScreenShotMessage;
示例代码：
- (BOOL)isScreenShotMessage{
return NO;
}
定义：
//用于长按操作菜单显示的指定，返回对应菜单类型的int数组
- (NSArray *)itemsOfLongPressPopupMenuWithParameters:(NSDictionary *)dic;
示例代码：
- (NSArray *)itemsOfLongPressPopupMenuWithParameters:(NSDictionary *)dic{
}
```
### 我的收藏相关协议方法
```objc
定义：
//收藏中 展示的高度，建议在getPlugInViewInFavByData中复用该方法
- (CGSize)getPluginViewSizeInFavByData:(NSString *)data;
示例代码：
- (CGSize)getPluginViewSizeInFavByData:(NSString *)data{
    return CGSizeMake(appWidth-40, 60);
}
定义：
//插件消息在收藏页显示的视图
- (UIView *)getPlugInViewInFavByData:(NSString *)data;
示例代码：
- (UIView *)getPlugInViewInFavByData:(NSString *)data{
 //自定义视图
 return view;
}
定义：
//在收藏中点击
-(void)clickActionInFav:(NSString *)data withView:(UIView *)view andPopViewController:(UIViewController *)viewC;
示例代码：
-(void)clickActionInFav:(NSString *)dataStr withView:(UIView *)view andPopViewController:(UIViewController *)viewC{
 NSData *binData = [dataStr dataUsingEncoding:NSUTF8StringEncoding];
 NSDictionary *dicData = [NSJSONSerialization JSONObjectWithData:binData options:kNilOptions error:nil];
 NSDictionary * data = [dicData objectForKey:@"data"];   
 MXLocationBrowseController * vc = [[MXLocationBrowseController alloc] init];
 vc.bodyString = dataStr;
MXLocationPOIVO * location = [[MXLocationPOIVO alloc] initWithDictionary:data];
vc.location = location;
viewC.searchDisplayController.active = NO;
[viewC.navigationController pushViewController:vc animated:YES];
}
```








































































