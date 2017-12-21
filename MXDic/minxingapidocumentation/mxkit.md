# MXKit接口说明文档
## 1.引入头文件
```objc
#import <MXKit/MXKit.h>
```
## 2.初始化
### 2.1获取MXKit对象
```objc
定义：
+(MXKit*)shareMXKit;
示例代码：
MXKit *MXObj=[MXKit shareMXKit];
```
### 2.2获取敏行viewcontroller，调用此api是整体启动敏行
```objc
定义：
-(UIViewController *)getMainVC;
示例代码：
UIViewController *mainVC=[MXObj getMainVC];
```
### 2.3将app的windows对象传递给敏行，在Kit里对windows作相应操作
```objc
定义：
-(void)setWindowToMXKit:(UIWindow*)window;
示例代码：
[MXObj setWindowToMXKit:self.window];
```
### 2.4处理数据的加密升级
```objc
定义：
-(void)handleUpgrade:(finishCallback)callback;
示例代码：
[MXObj handleUpgrade:^(id result, MXError *error) {
  //一些初始化配置的操作...
}];
```
### 2.5将主app的系统delegate传递给kit；将敏行作为一个整体调用的时候才需要调用这些接口
```objc
定义：
-(BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions;//应用程序启动之后，要执行的委托调用

-(BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation;

-(void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken;

-(void)application:(UIApplication *)application didFailToRegisterForRemoteNotificationsWithError:(NSError *)error;

-(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo;

-(void)applicationWillResignActive:(UIApplication *)application;//应用程序将要由活动状态切换到非活动状态时执行的委托调用，如按下home按钮，返回主屏幕，或全屏之间切换应用程序等

-(void)applicationDidEnterBackground:(UIApplication *)application;//在应用程序已进入后台程序时，要执行的委托调用，所以要设置后台继续运行，则在这个函数里面设置即可。
-(void)applicationWillEnterForeground:(UIApplication *)application;//在应用程序将要进入前台时（被激活），要执行的委托调用，与applicationWillResignActive方法相对应
-(void)applicationDidBecomeActive:(UIApplication *)application;////在应用程序已被激活后，要执行的委托调用，刚好与  applicationDidEnterBackground 方法相对应。  
-(void)applicationWillTerminate:(UIApplication *)application;////在应用程序要完全退出的时候，要执行的委托调用。  
-(void)application:(UIApplication *)application performFetchWithCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler;
//后台执行部分代码的方法
示例代码：
//在appdelegate.m里，各个api的使用方法相同，这里以进入后台的事件为例
- (void)applicationDidEnterBackground:(UIApplication *)application {
MXKit *MXObj = [MXKit shareMXKit];
[MXObj applicationDidEnterBackground:application];
}
```
## 3.设置deviceToken
### 注册设备到server
```objc
定义：
-(void)registDeviceTokenToserver:(NSString *)deviceToken;
示例代码：
[MXObj registDeviceTokenToserver:devicetoken];
```
## 4.服务器环境
### 4.1设置服务器环境
```objc
参数说明：
MX_URL：服务器地址
MX_PORT：服务器端口号
MX_MQTT_URL：服务器推送地址
MX_MQTT_PORT：服务器推送端口号
定义：
-(void)init:(NSString *)url withPort:(NSString *)minxingPort withMqttUrl:(NSString *)mqttUrl withMqttPort:(NSString *)mqttPort;
示例代码：
[MXObj init:MX_URL withPort:MX_PORT withMqttUrl:MX_MQTT_URL withMqttPort:MX_MQTT_PORT];
```
### 4.2获取服务器参数
```objc
定义：
-(NSDictionary *)getMinxing;
示例代码：
NSDictionary *dic = [NSDictionary dictionaryWithDictionary:[MXObj getMinxing]];
```
### 4.3通过IP修改服务器参数进行保存
```objc
参数说明：
url: 服务器地址
minxingPort:服务器端口号
mqttUrl:服务器推送地址
mqttPort:服务器推送端口号
定义：
-(void)save:(NSString *)url withPort:(NSString *)minxingPort withMqttUrl:(NSString *)mqttUrl withMqttPort:(NSString *)mqttPort;
示例代码：
[MXObj save:serverField.text withPort:serverPortField.text withMqttUrl:mqttField.text withMqttPort:mqttPortField.text];
```
## 5.登录与退出的相关接口
### 5.1第一次登录(需要用户名和密码)
```objc
参数说明：
loginName：登录名
password：密码
定义：
-(void)login:(NSString *)loginName withPassword:(NSString *)password;
示例代码：
[MXObj login:name withPassword:password];
```
### 5.2已经登录过的用户直接登录
```objc
定义：
-(void)login:(finishCallback)callback;
示例代码：
[MXObj login:^(id result, MXError *error){
if (result &&!error) {
//登录成功之后进行其他的操作
}
}];
```
### 5.3注册登录成功后回调
```objc
定义：
-(void)registLoginCallback:(finishCallback)callback;
示例代码：
[MXObj registLoginCallback:^(id result, MXError *error){
if(result && !error){
[self initapp];
}
}];
```
### 5.4用户被踢时的回调
```objc
定义：
-(void)registLogout:(finishCallback)callback;
示例代码：
[MXObj registLogout:^(id result, MXError *error){
if (result && !error) {
、、、、、、
}
}];
```
### 5.5获取短信验证码
```objc
参数说明：
phoneNumber: 手机号
定义：
-(void)getPhoneTextVerifyCode:(NSString*)phoneNumber withCallback:(finishCallback)callback;
示例代码：
[[MXKit shareMXKit] getPhoneTextVerifyCode:phoneNumber withCallback:^(id result, MXError *error){
if(error) {
[(MXLoginView *)self.view resetGetVerifyCodeButton];
}
}];
```
### 5.6修改密码
```objc
参数说明：
oldPassword:旧密码
newPassword:新密码
callback：回调里执行修改成功后的相关操作
定义：
-(void)changPassword:(NSString *)oldPassword withNewPassword:(NSString *)newPassword WithCallback:(finishCallback)callback;
示例代码：
[MXObj changPassword:currentstr withNewPassword:newPassword WithCallback:^(id result,MXError *error){
if (result) {
//修改成功的操作
}
}];
```
### 5.7退出登录
```objc
定义：
-(void)logout;
示例代码：
[MXObj logout];
```
### 5.8主动注销，例如：切换其他账号登录时可调用
```objc
定义：
-(void)logoutWithoutUI:(BOOL)removeName;
示例代码：
[MXObj logoutWithoutUI:removeName];
```
## 6.个人信息相关接口
### 6.1获取侧拉栏数据(例如：姓名、头像url、已加入的社区等信息)
```objc
定义：
-(void)getUserDataWithCallback:(finishCallback)callback;
示例代码：
[MXObj getUserDataWithCallback:^(id result, MXError *error){
//result是一个字典类型
userName = [result objectForKey:@"name"];
avatar_url = [result objectForKey:@"imagePath"];
networkId = [[result objectForKey:@"joinedNetworks"] intValue];
joinedNetworksArray = [result objectForKey:@"networks"];
}];
```
### 6.2展现个人信息页面
```objc
定义：
-(void)showUserViewWithNav:(UINavigationController *)nav;
示例代码：
int index = [self.tabBarController selectedIndex];
UINavigationController *nav = [self.tabBarController.viewControllers objectAtIndex:index];
MXKit *MXObj = [MXKit shareMXKit];
[MXObj showUserViewWithNav:(UINavigationController *)nav];
```
### 6.3个人信息页面返回的回调
```objc
定义：
-(void)registUserInfoBackCallback:(finishCallback)callback;
示例代码：
[MXObj registUserInfoBackCallback:^(id result, MXError *error){
if (result && !error) {
NSLog(@"user info back!!!!!!!!!!!!!");
}
}];
```
### 6.4获取当前用户信息，返回值是字典类型，可从字典中取得用户名等信息
```objc
定义：
-(NSDictionary *)getCurrentUser;
示例代码：
NSDictionary *me = [[MXKit shareMXKit] getCurrentUser];
```
### 6.5获得账号ID
```objc
定义：
-(int)getAccountID;
示例代码：
int AccountID = [[MXKit shareMXKit] getAccountID];
```
### 6.6获取社区名
```objc
定义：
-(void)getNetWorkName;
示例代码：
[[MXKit shareMXKit] getNetWorkName];
```
### 6.7是否需要显示外部社区工作圈消息的红点儿
```objc
定义：
-(BOOL)needShowRedIcon;
示例代码：
BOOL showRedIcon = [[MXKit shareMXKit] needShowRedIcon];
```
### 6.8获取已加入社区未读数列表，通过字典获取未读数
```objc
定义：
-(NSDictionary *)getUnreadDict;
示例代码：
NSDictionary *unreadDict=[MXObj getUnreadDict];
```
### 6.9获取社区消息的红点
```objc
定义：
-(BOOL)getRedIcon:(NSString *)key;
示例代码：
NSString *key = [NSString stringWithFormat:@"%@_%d_%d", @"notification_red_icon", networkVO.with_user_id, networkVO.ID];
MXKit *MXObj = [MXKit shareMXKit];
BOOL showRedIcon = [MXObj getRedIcon:key];
```
### 6.10更新未读数的回调
```objc
定义：
-(void)updateUnreadCount:(finishCallback)callback;
示例代码：
[MXObj updateUnreadCount:^(id result, MXError *error){
if (result && !error) {
[[NSNotificationCenter defaultCenter] postNotificationName:kNotificationNameChangeNetworkUnreadCount object:result];
}
}];
```
### 6.11获取所有社区里的消息未读数
```objc
定义：
-(int)getCountAllUnreadMessageInAllNetwork;
示例代码：
int unreadCount = [MXObj getCountAllUnreadMessageInAllNetwork];
```
### 6.12切换社区
```objc
定义：
-(void)changeNetwork:(int)networkId;
示例代码：
[MXObj changeNetwork:networkVO.ID];
```
### 6.13用户保留上次登录的社区
```objc
参数说明：
networkId: 传入社区id
定义：
-(void)forceChangeNetwork2Last:(int)networkId;
示例代码：
[[MXKit shareMXKit] forceChangeNetwork2Last:54];
[self setNetWorkName:@"南区销售"];
```
### 6.14获得社区名称时的回调
```objc
定义：
-(void)getNetWorkNameWithCallback:(finishCallback)callback;
示例代码：
[MXObj getNetWorkNameWithCallback:^(id result, MXError *error) {
[self setNetWorkName:result];
}];
```
### 6.15展示外部社区
```objc
定义：
-(void)gotoExternalViewWithNav:(UINavigationController *)nav;
示例代码：
int index = [self.tabBarController selectedIndex];
UINavigationController *nav = [self.tabBarController.viewControllers objectAtIndex:index];
MXKit *MXObj = [MXKit shareMXKit];
[MXObj gotoExternalViewWithNav:(UINavigationController *)nav];
```
### 6.16展示用户头像
```objc
参数说明：
imageView：传入UIImageView控件
image：头像图片
定义：
-(void)showUserViewWithImageView:(UIImageView *)imageView andUserImage:(UIImage *)image;
示例代码：
[MXObj showUserViewWithImageView:herd.imageView andUserImage:herd.imageView.image];
```
### 6.17使用第三方开发的通讯录
```objc
定义：
-(void)registerContactsManagerClass:(Class)className;
示例代码：
[MXObj registerContactsManagerClass:[MXContactsUIManager class]];
```
## 7.可配配置(在MXClient/library/common/MXConfig.h配置文件中添加配置)
```objc
//初始化敏邮
[MXObj initMinyou:CLIENT_SHOW_MAIL];
//初始化umeng
[MXObj initUmeng:UMENG_KEY withChannel:UMENG_CHANNEL];
//设置个人信息的header页签(“个人信息”、“工作圈”、“文件”)
[MXObj initUserTabBar:CLIENT_SHOW_USER_TAB_BAR];
//加拨短号
[MXObj initExtendDailNumber:EXTEND_DAIL_NUM];
//是否隐藏工作圈的创建
[MXObj initHideCircleCreation:YES];
//隐藏公司通讯录
[MXObj initDisableCompanyContact:CLIENT_SHOW_CONTACT_COMPANY];
//设置控制程序内部浏览器右上角弹出菜单的背景颜色、分割线的颜色、选中颜色、字体颜色
[MXObj initBaseColor:navBarColor];
[MXObj initSepeartorColor:ColorWithRGB(88, 166, 255, 1)];
[MXObj initSelectedColor:[UIColor redColor]];
[MXObj initPopUpTextColor:[UIColor yellowColor]];
[MXObj initSureBtnTitleColor:[UIColor greenColor]];
//个人信息页配置toolView BtnColor(“加为联系人”、“发消息”、“特别关注”这些字体的颜色)和个人信息页面配置上部选中状态的底栏的颜色
[MXObj initInfoBtnColor:infoBtnAndBarColor];
[MXObj initInfoBarColor:infoBtnAndBarColor];
//设置电话号码是否需要加密(星号),比如13812345678，隐藏第4位到第7位，那么变为138xxxx5678。
[MXObj initEncryptCellphonePatten:@"4-7"];
//设置通讯录索引颜色
[MXObj initContactIndexColor:[UIColor greenColor]];
//设置自定义clientID
[MXObj initCustomClientID:MX_CUSTOM_CLIENT_ID];
//启用水印
[MXObj initWarterMask:MX_ENABLE_WATERMASK];
//隐藏公众号
[MXObj initHidePublicAccount:CLIENT_HIDDEN_PUBLIC_ACCOUNT];
//隐藏群聊
[MXObj initHideMultiChat:CLIENT_HIDDEN_MULTI_CHAT];
//隐藏特别关注
[MXObj initHideVipcontant:YES];
//启用邮件
[MXObj initEnableAddressEmail:MX_ENABLE_ADDRESS_MAIL];
//禁用应用中心划线
[MXObj initDisableDrawLine:YES];
//禁止下载
[MXObj initDisableDownload:MX_DISABLE_DOWNLOAD];
//短信验证登录
[MXObj initMobileLogin:MX_ENABLE_MOBILE_TEXT_VERIFY];
//禁用邮箱会话
[MXObj initDisableMXMail:MX_DISABLE_MXMAIL];
//禁用shareExtension
[MXObj initDisableShareExtension:MX_DISABLE_SHARE_EXTENSION];
//点击他人的邮箱地址禁止用敏邮启动
[MXObj initDisableMXMail:YES];
//设置状态栏颜色
[[MXKit shareMXKit]setStatusBarStyle:UIStatusBarStyleLightContent];
//设置title的字体和颜色,传入参数attr是一个字典
[MXObj setTitleBarAttribute:attr];
//设置item bar的字体和颜色，传入参数attr是一个字典
[MXObj setItemBarAttribute:attr];
//设置进度条加载的背景颜色
[[MXKit shareMXKit] setWebViewProgressColor:[UIColor mx_colorWithHexString:@"#1895ff"]];
//设置应用中心的划线
[MXObj initEnableDrawLine:MX_DISABLE_APPCENTERDRAWLINE];
//个人信息页面分组展示
[MXObj initEnableUserInfoGroup:YES];
//设置主题颜色，应用程序的主色调
[[MXKit shareMXKit] setThemeColor:themeColor];
//设置添加好友
[[MXKit shareMXKit] initEnableAddFriends:YES];
//启用公众号历史消息的分享
[[MXKit shareMXKit] endableHistoryMessageShare:YES];
```
## 8.其他API
### 注册第三方接收的推送的回调
```objc
定义：
-(void)registPushServiceCallback:(finishCallback)callback;
示例代码：
[MXObj registPushServiceCallback:^(id result, MXError *error) {
//这里的推送收到的时候是一个非主线程的推送,如果用户需要更新UI，请自行切换到主线程
NSLog(@"receiver push from server==%@", result);
}];
```
### 点击tabbar时kit内部处理tabbar红点更新等的一些操作
```objc
定义：
-(void)tabBarControllerDidSelect:(UINavigationController *)navigationController;
示例代码：
- (void)tabBarController:(UITabBarController *)tabBarController didSelectViewController:(UIViewController *)viewController {
UINavigationController *navCtrl = [self.tabBarController.viewControllers objectAtIndex:self.tabBarController.selectedIndex];
NSArray* vcArray = [navCtrl viewControllers];
if (vcArray.count == 1) {
MXKit *MXObj = [MXKit shareMXKit];
[MXObj tabBarControllerDidSelect:navCtrl];
}
}
```
### 切换页签时的回调(app2app)
```objc
定义：
-(void)registTabSelectCallback:(finishCallback)callback;
示例代码：
[MXObj registTabSelectCallback:^(id result, MXError *error){
if(!error) {
NSNumber *index = (NSNumber *)result;
[self.tabBarController setSelectedIndex:index.intValue];
}
}];
```
### 注册处理公众号里面第三方数据的回调
```objc
定义：
-(void)registNativeCallback:(handleNativeCallback)callback;
示例代码：
[[MXKit shareMXKit] registNativeCallback: ^(id result, MXError *error){
NSLog(@"handle native result===%@", result);
return NO;
}];
```
### 注册新版本的回调
```objc
定义：
-(void)registNewVersionCallback:(finishCallback)callback;
示例代码：
[[MXKit shareMXKit] registNewVersionCallback: ^(id result, MXError *error){
isNew = YES;
}];
```
### 注册页面显示的回调, 这里finishCallback是一个nsdictionary， 字段名是@"className"和@"title"
```objc
定义：
-(void)registPageAppearCallback:(finishCallback)callback;
示例代码：
[MXObj registPageAppearCallback:^(id object, MXError *error) {
NSLog(@"view will appear callback === %@", object);
}];
```
### 注册页面消失的回调
```objc
定义：
-(void)registPageDisapperCallback:(finishCallback)callback;
示例代码：
[MXObj registPageDisapperCallback:^(id object, MXError *error) {
NSLog(@"view will disappear callback === %@", object);
}];
```
### 注册导航栏底部隐藏的回调
```objc
定义：
-(void)registNavigationBottomBarHiddenCallback:(finishCallback)hiddenCallback;
示例代码：
[MXObj registNavigationBottomBarHiddenCallback:^(id object, MXError *error) {
、、、、、、
}];
```
### 关闭其他视图
```objc
定义：
-(void)closeFunctionView;
示例代码：
[MXObj closeFunctionView];
```
### 注册第二辑页面显示的回调
```objc
定义：
-(void)registSecondLevelPageAppearCallback:(finishCallback)callback;
示例代码：
[MXObj registSecondLevelPageAppearCallback:^(id object, MXError *error) {
NSLog(@"view will secondleverlPageappear callback === %@", object);
}];
```
### 在屏幕顶端显示error信息
```objc
定义：
-(void)showMXErrorTips:(NSString *)errorString;
示例代码：
[MXObj showMXErrorTips:errorstr];
```
### 提示添加名片成功
```objc
定义：
-(void)showShortMessage:(NSString *)text;
示例代码：
[[MXKit shareMXKit] showShortMessage:@"添加名片成功"];
```
### 扫一扫接口的使用
```objc
定义：
-(UIViewController *)getQRScanViewController;
-(void)registQRResultCallback:(handleNativeCallback)callback;
示例代码：
//获取扫一扫的页面
UIViewController *qrVC = [[MXKit shareMXKit] getQRScanViewController];
qrVC.hidesBottomBarWhenPushed = YES;
UIViewController *rootVC = [[MXChat sharedInstance] getChatViewController];
[rootVC.navigationController pushViewController:qrVC animated:YES];
//注册二维码扫描的结果回调
[[MXKit shareMXKit] registQRResultCallback:^(id result, MXError *error) {
NSLog(@"result===%@", result);
//这里的return表示是否需要mxkit继续后面的处理，如果return NO则表示需要mxkit继续处理，反之return YES
return NO;
}];
```
### 获得webview的url，通过给参数url拼接mxLayout来控制标题栏、工具栏、分享菜单
```objc
定义：
-(UIViewController *)getWebViewController:(NSString *)url;
示例代码：
UIViewController *vc = [[MXKit shareMXKit] getWebViewController:url];
```
### urlpage作为页签
```objc
定义：
-(UIViewController *)getWebUrlViewController:(NSString *)url;
示例代码：
UIViewController *webUrl=[MXObj getWebUrlViewController:arr[i][@"value"]];
```
### 设置urlpage的headerview
```objc
参数说明：
headerView：传入自定义导航栏的UIView视图
定义：
-(void)setCustomHeaderView:(UIView *)headerView;
示例代码：
[MXObj setCustomHeaderView:urlHeader];
```
### 点击urlPage页签内部显示新的urlpage
```objc
参数说明：
url：页面网址
showStyle：页面的展示方式（push、present方式）
viewController：当前控制器
定义：
-(void)getUrlPageWithUrl:(NSString *)url withShowStyler:(NavigationBarShowStyle)showStyle withViewController:(UIViewController *)viewController withCallback:(finishCallback)callBack;
示例代码：
[MXObj getUrlPageWithUrl:url withShowStyler:NavigationBarShowStyle withViewController:vc withCallback:^(id object, MXError *error) {
//页面将要出现时的操作
}];
```
### 新闻页签
```objc
定义：
-(UIViewController *)getNewsViewController;//获取新闻controller
-(void)cleanNewsViewcontroller; //清除新闻
- (void)setNewsViewControllerCustomHeader:(UIView *)headerView; //设置新闻页签自定义header
示例代码：
UIViewController *news = [MXObj getNewsViewController];
[MXObj setNewsViewControllerCustomHeader:urlHeader];
```
### 展示第三方app调用敏行聊天-
```objc
定义：
-(void)showMineApp2AppChat;
示例代码:
[MXObj showMineApp2AppChat];
```
### 第三方app强制登录敏行的回调
```objc
定义：
-(void)registForceLoginCallback:(finishCallback)callback;
示例代码：
[MXObj registForceLoginCallback:^(id result, MXError *error) {
if(result && !error)
{
}
}];
```
### 展示我的收藏、我的消息、我上传的文件、我下载的文件的页面
```objc
定义：
-(void)showFavorteWithNav:(UINavigationController *)nav;
-(void)showMyMessage:(UINavigationController *)vc;
-(void)showMyUpload:(UINavigationController *)vc;
-(void)showMyDownload:(UINavigationController *)vc;
示例代码：
switch (index) {
case 0:
[[MXKit shareMXKit] showFavorteWithNav:[self.tabBarController.viewControllers objectAtIndex:tabIndex]];
break;
case 1:
[[MXKit shareMXKit] showMyMessage:[self.tabBarController.viewControllers objectAtIndex:tabIndex]];
break;
case 2:
[[MXKit shareMXKit] showMyUpload:[self.tabBarController.viewControllers objectAtIndex:tabIndex]];
break;
case 3:
[[MXKit shareMXKit] showMyDownload:[self.tabBarController.viewControllers objectAtIndex:tabIndex]];
break;
default:
break;
}
```
### plist的值的获取、存储、删除以及二进制的获取、存储
```objc
定义：
//获取MX plist的值
-(id)getMXUserDefaultValueForKey:(NSString *)key;
//存储MX plist的值
-(void)setMXUserDefaultValue:(id)value forKey:(NSString *)key;
//删除MX plist的值
-(void)removeMXUserDefaultValueForKey:(NSString *)key;
//获取二进制的值
-(id)getMXUserDefaultBinaryValueForKey:(NSString *)key;
//存储二进制的值
-(void)setMXUserDefaultBinaryValue:(id)value forKey:(NSString *)key;
示例代码：
NSString *userName = [[MXKit shareMXKit] getMXUserDefaultValueForKey:@"user_name"];
[[MXKit shareMXKit] setMXUserDefaultValue:name forKey:@"user_name"];
[[MXKit shareMXKit] removeMXUserDefaultValueForKey:userName];
NSData *data = [[MXKit shareMXKit] getMXUserDefaultBinaryValueForKey:@"gesturePassCode"];
[[MXKit shareMXKit] setMXUserDefaultBinaryValue:passCodeData forKey:@"gesturePassCode"];
```
### 敏邮的登录
```objc
定义：
//带用户名密码登陆
-(void)showMXEmailWithUserName:(NSString *)userName withPassword:(NSString *)password withAppID:(NSString *)appID withViewController:(UIViewController*)parentVC;
//不带用户名密码
-(void)showMXEmailWithViewController:(UIViewController*)parentVC withAppID:(NSString *)appID withParams:(NSString *)params;
示例代码：
[[MXKit shareMXKit] showMXEmailWithUserName: @"test@dehuinet.com" withPassword:@"Test123" withAppID:self.appID withViewController:self.parentViewController];
[[MXKit shareMXKit] showMXEmailWithViewController:self.parentViewController withAppID:self.appID withParams:self.params];
```
### 设置字体大小完成时的回调
```objc
参数说明：
num: 字体大小(0-3依次为小、标准、大、特大)
aBlock： 回调里执行设置字体大小完成时的操作
定义：
-(void)setFontNum:(CGFloat)num andFinishedBlock:(finishCallback)aBlock;
示例代码：
[[MXKit shareMXKit] setFontNum:_flag andFinishedBlock:^(id object, MXError *error) {
[weakHUD hide:YES];
[weakSelf.navigationController popViewControllerAnimated:YES];
}];
```
### 获取字体大小
```objc
定义：
-(CGFloat)getFontNum;
示例代码：
CGFloat _flag = [[MXKit shareMXKit] getFontNum];
```
### 设置分享的Keys
```objc
定义：
-(void)setShareExtensionGroupID:(NSString *)groupID andWorkCircleKey:(NSString *)key1 andChatKey:(NSString *)key2 andheadersKey:(NSString *)key3 andTokenKey:(NSString *)key4 chatRetArrayKey:(NSString *)key5;
示例代码：
[[MXKit shareMXKit] setShareExtensionGroupID:MX_SHARE_GORUPID andWorkCircleKey:MX_SHARE_CIRCLE andChatKey:MX_SHARE_CHAT andheadersKey:MX_SHARE_REQUEST_HEADER andTokenKey:MX_SHART_TOKEN chatRetArrayKey:MX_SHARE_CAHTARRAY];
```
### 获取邮箱未读消息
```objc
定义：
-(int)getUnreadEmailCount;
示例代码：
[[MXKit shareMXKit] getUnreadEmailCount];
```
### 敏邮升级
```objc
定义：
-(void)mxMailUpgrade;
示例代码：
[[MXKit shareMXKit] mxMailUpgrade];
```
### 设置邮件前台收取间隔，单位是秒
```objc
参数说明：
interval：收取间隔(秒数)
定义：
-(void)initEmailInterval:(int)interval;
示例代码：
[MXObj initEmailInterval:MX_GET_EMAIL_INTERVAL];
```
### 发送邮件给指定的人
```objc
参数说明：
name：名字(可为空)
email：邮箱地址
parentViewController：传入当前控制器，用于跳转到写邮箱界面
定义：
-(void)sendEmailTo:(NSString *)name withEmail:(NSString *)email withViewControler:(UIViewController *)parentViewController;
示例代码：
//参数name可以为nil，其他参数不可以为nil
[[MXKit shareMXKit] sendEmailTo:nil withEmail:@"liyang@dehuinet.com" withViewControler:self];
```
### 获取设备UUID
```objc
定义：
-(NSString *)getDeviceUUID;
示例代码：
NSString *str =[[MXKit shareMXKit] getDeviceUUID];
```
### 开启敏邮日志
```objc
定义：
-(void)enableMailLog;
示例代码：
[[MXKit shareMXKit]enableMailLog];
```
### 删除敏邮信息
```objc
定义：
-(void)removeMXMail;
示例代码：
[[MXKit shareMXKit] removeMXMail];
```
### 测试会话
```objc
定义：
-(void)testConversation;
示例代码：
[[MXKit shareMXKit]testConversation];
```
### 同步未读数到server
```objc
参数说明;
unreadCount: 未读数
定义：
-(void)syncUnreadCount2Server:(NSInteger )unreadCount;
示例代码：
[[MXKit shareMXKit]syncUnreadCount2Server:unredcount];
```
### 显示健步走排行榜列表
```objc
参数说明：
parentVC:父控制器，用来push到排行榜页面
AppId:应用id
ocuId:公众号Id
定义：
-(void)showWalkingRankingListWithViewController:(UIViewController *)parentVC AppId:(NSString *)appId OcuId:(int)ocuId;
示例代码：
[[MXKit shareMXKit]showWalkingRankingListWithViewController:self.parentViewController AppId:@"running_man" OCuId:1];
```
### 显示健步走配置页面
```objc
参数说明：
parentVC:父控制器，用来push到排行榜页面
AppId:应用id
ocuId:公众号Id
定义：
-(void)showWalkingInfoWithViewController:(UIViewController *)parentVC withApppID:(NSString *)appId OcuId:(int)ocuId;
示例代码：
[[MXKit shareMXKit]showWalkingInfoWithViewController:self.parentViewController AppId:@"running_man" OCuId:1];
```
### 获取当前环境的服务器地址，例如：test.dehuinet.com
```objc
定义：
-(NSString *)getHostname;
示例代码：
[MXObj getHostname];
```
### 设置非wifi环境下 上传下载文件大小限制 超过限制弹出提示
```objc
定义：
- (void)initNoWifiFileSizeLimit:(NSInteger)size;
示例代码：
[MXObj initNoWifiFileSizeLimit:MX_NOWIFI_FILESIZE_LIMIT];
```
### 公众号入口，传入参数OCuID
```objc
定义：
- (void)startOcuChat:(NSString *)ocuID;
示例代码：
[[MXKit shareMXKit] startOcuChat:@"ccb74893d3c14028524e9751a3770381"];
```
### ios附件下载问题-在删除缓存的时候调用接口把下载文件时存到数据库里的文件相关也删掉
```objc
定义：
- (void)cleanLocalFiles;
示例代码：
[[MXKit shareMXKit]cleanLocalFiles];
```
### 获得用户敏行信息,例如：MinxingMessenger/4.0.0.036(iPhone 5s; iOS/10.1.1)
```objc
定义：
-(NSString *)getMXUserAgent;
示例代码：
NSString *userAgent = [[MXKit shareMXKit] getMXUserAgent];
```
### 启用安全隧道
```objc
参数说明
address：IP地址
定义：
-(void)startMXTunnel:(NSString *)address;
示例代码：
[[MXKit shareMXKit] startMXTunnel:@"dev3.dehuinet.com:2990"];
```
### 设置安全隧道白名单端口号
```objc
定义：
-(void)setMXTunnelWhiteListPort:(NSString *)port;
示例代码：
[[MXKit shareMXKit] setMXTunnelWhiteListPort:@"3021"];
```
### 注册安全隧道连接
```objc
定义：
-(void)registMXTunnelConnected:(finishCallback)callback;
示例代码：
[[MXKit shareMXKit] registMXTunnelConnected:^(id object, MXError *error){
 if(!error) {
 //成功后的操作
  [self handleLoginCallback];
 } else {
   //show error
  [[MXKit shareMXKit] showMXErrorTips:error.description];
   }
}];
```
### 获取安全隧道登录名
```objc
定义：
-(NSString *)getTunnelUser;
示例代码：
NSString *tunnelUser = [[MXKit shareMXKit] getTunnelUser];
```
### 设置移动设备管理
```objc
定义：
- (void)setMDMEnable:(BOOL)isMDM;
示例代码：
[MXObj setMDMEnable:MX_MDM_ENABLE];
```
### 接收自签证书challenge
```objc
定义：
-(void)mxDidReceiveAuthenticationChallenge:(NSURLAuthenticationChallenge *)challenge;
示例代码：
[[MXKit shareMXKit] mxDidReceiveAuthenticationChallenge:challenge];
```
### 注册分析日志，用来分析相关问题
```objc
定义：
-(void)registAnalayseEvent:(functionCallback)callback;
示例代码：
//UM Event Handler
[[MXKit shareMXKit] registAnalayseEvent:^(NSDictionary *eventDic) {
if(eventDic && [eventDic isKindOfClass:[NSDictionary class]]) {
NSString *eventID = [eventDic objectForKey:@"eventID"];
NSDictionary *attriDic = [eventDic objectForKey:@"attributes"];
NSNumber *duration = [eventDic objectForKey:@"durations"];
if([duration integerValue] > 0) {
if(attriDic) {
[MobClick event:eventID attributes:attriDic counter:[duration intValue]];
} else {
[MobClick event:eventID durations:[duration intValue]];
}
} else {
if(attriDic) {
[MobClick event:eventID attributes:attriDic];
} else {
[MobClick event:eventID];
}
}
}
}];
```
### 注册更新GPS的回调
```objc
定义：
-(void)registGPSProvider:(functionCallback)callback;
示例代码：
[MXObj registGPSProvider:^(handleGpsUpdateCallback gpsCallback){
    MXLocation *locationpProvider = [[MXLocation alloc] init];
    [locationpProvider getLocationWithCallback:gpsCallback];
}];
```
### 自动上传GPS
```objc
参数说明：
url: 接收gps,自动上传的url
interval: 自动上传gps的时间间隔（单位是秒)
定义：
-(void)registGPSUploadServerAddress:(NSString *)url withInterval:(CGFloat)interval;
示例代码：
[MXObj registGPSUploadServerAddress:MX_GPS_UPLOADING_URL withInterval:MX_GPS_UPLOADING_INTERVAL];
```
### 检查是否启用手势密码或者指纹验证，如果成功或者不需要验证则执行callback
```objc
定义：
 -(void)checkSecurtyAuthStatusWithCallback:(fuctionNoParamCallback)callback;
示例代码：
[[MXKit shareMXKit] checkSecurtyAuthStatusWithCallback:^{
//有token进入主页面
[weakSelf showMain];                    dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
  [weakSelf startVersionCheck:YES];
});
}];
```
### 注册工作圈自定义action的点击事件
```objc
定义：
//同MXCircle的方法(-(void)setAction:(NSArray *)action)配合使用
- (void)registCustomActionCallback:(finishCallback)callback;
示例代码：
[MXObj registCustomActionCallback:^(id object, MXError *error) { 
if ([object isKindOfClass:[NSDictionary class]]) {
  if ([[object valueForKey:@"key"] isEqualToString:@"mx_baidu"]) {
     CDVViewController * cdv = [[CDVViewController alloc] init];
     UINavigationController * nav = [[UINavigationController alloc] initWithRootViewController:cdv];
     cdv.startPage = @"https://www.baidu.com";
    [self.window.rootViewController presentViewController:nav animated:YES completion:nil];
  }
}
}];
###  发送用户验证码的回调
参数说明：
phoneNum：要发送验证码的手机号
state:  状态(1:注册，2:忘记密码，3:修改手机号)
定义：
-(void)sendAuthCode:(NSString *)phoneNum state:(int)state  callback:(finishCallback)callback;
示例代码：
[MXObj sendAuthCode:self.phoneNum state:self.state callback:^(id object, MXError *error) {
//执行成功或错误的操作
}];
### 验证用户验证码
```objc
参数说明：
phoneNum：手机号
authCode：验证码
state：状态(1:注册，2:忘记密码，3:修改手机号)
定义：
-(void)validateAuthCode:(NSString *)phoneNum authCode:(NSString *)authCode state:(int)state callback:(finishCallback)callback;
示例代码：
[MXObj validateAuthCode:self.phoneNum authCode:code state:self.state callback:^(id object, MXError *error) {
 if (!error.description) {
//执行验证成功时的操作
} else {
//执行验证失败时的操作
}
 }];
```
### 验证两次输入的密码是否一致
```objc
参数说明：
phoneNum：手机号
passcode: 密码
confirm：第二次密码
deptID：部门ID
state：状态(1:注册，2:忘记密码)
定义：
-(void)regist:(NSString *)phoneNum passcode:(NSString *)passcode confirm:(NSString *)confirm deptID:(NSInteger)deptID state:(int)state params:(NSDictionary *)params callback:(finishCallback)callback;
示例代码：
[MXObj regist:self.phoneNum passcode:passCodeTextField.text confirm:confirmTextField.text deptID:-1 state:self.state params:nil callback:^(id object, MXError *error) {
if (!error.description) {
 [SVProgressHUD show];
[MXObj login:self.phoneNum withPassword:passCodeTextField.text];
 } else {
[self showAlertWithTitle:nil message:error.description type:UIAlertControllerStyleAlert actionTitle:GetLocalResStr(@"mx_system_sure")  actionHandler:nil];
 }
}];
```
### 判断输入的手机账号是否已经注册
```objc
参数说明：
phoneNum：输入的手机号
定义：
-(void)mobileAlreadyExist:(NSString *)phoneNum callback:(finishCallback)callback;
示例代码：
[MXObj mobileAlreadyExist:emailTF.text callback:^(id object, MXError *error) {
if (!error.description) {
 NSDictionary *dic = (NSDictionary *)object;
 BOOL exist = [dic objectForKey:@"exists"];
 if (exist) {
//手机号已注册               
 } else { 
//没注册                
 }
}
}];
```
### 修改手机号
```objc
参数说明：
newPhoneNum：新的手机号
定义：
- (void)changePhoneNum:(NSString *)newPhoneNum callback:(finishCallback)callback;
示例代码：
[MXObj changePhoneNum:self.phoneNum callback:^(id object, MXError *error) {
if (!error.description) {
//修改成功
} else {
//修改失败
}
}];
```
### 校验密码强度
```objc
参数说明：
password：输入的密码
定义：
- (void)checkPasswordStrength:(NSString *)password callback:(finishCallback)callback;
示例代码：
[MXObj checkPasswordStrength:newPasswordTF.text callback:^(id object, MXError *error) {
if (!error.description) {
} else {
}
}];
```
### 拿到view的stack里面最上面的viewController
```objc
定义：
-(UIViewController *)getTopViewController;
示例代码：
[[MXKit shareMXKit] getTopViewController];
```
### 获取注册时需要填写的用户属性
```objc
定义：
- (void)getPersonInfoData:(finishCallback)callback;
示例代码：
[[MXKit shareMXKit] getPersonInfoData:^(id object, MXError *error) {
if (!error.description) {
}
}];
```
### 获取社群名或域名(大服务)
```objc
定义：
-(NSString*)getCurrentDomainName;
示例代码：
[[MXKit shareMXKit]getCurrentDomainName];
```
### 展示社群页面
```objc
定义：
-(void)showDomainViewWithNav:(UINavigationController *)nav;
示例代码：
int tabIndex =  [self.tabBarController selectedIndex];
[[MXKit shareMXKit] showDomainViewWithNav:[self.tabBarController.viewControllers objectAtIndex:tabIndex]];
```





