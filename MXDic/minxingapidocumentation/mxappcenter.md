# MXAppCenter接口说明文档
## 1.引入头文件
```objc
#import <MXKit/MXAppCenter.h>
```
## 2.初始化MXAppCenter
### 2.1获取MXAppCenter对象
```objc
定义：
+(MXAppCenter *)sharedInstance;
示例代码：
[MXAppCenter sharedInstance];
```
### 2.2获取应用中心的ViewController
```objc
定义：
-(UIViewController *)getAppCenterViewController;
示例代码：
UIViewController *appCenter = [[MXAppCenter sharedInstance] getAppCenterViewController];
```
### 2.3自定义应用中心的headerView
```objc
定义：
-(void)setCustomHeaderView:(UIView *)headerView;
示例代码：
[[MXAppCenter sharedInstance] setCustomHeaderView:appCenterHeader];
```
### 2.4释放应用中心的viewController
```objc
定义：
-(void)cleanAppCenterViewController;
示例代码：
[[MXAppCenter sharedInstance] cleanAppCenterViewController];
```
## 3.MXAppCenter模块详细API使用说明
### 删掉自定义应用中心上部的view
```objc
定义：
-(void)removeCustomAppHeaderView;
示例代码：
[[MXAppCenter sharedInstance]removeCustomAppHeaderView];
```
### 右滑appCenter界面的时候回调
```objc
定义：
-(void)startPullDownWithCallback:(finishCallback)callback;
示例代码：
[[MXAppCenter sharedInstance] startPullDownWithCallback:^(id result, MXError *error) {
   if (result) {
    [appCenterHeader startPullDown];
   }
}];
```
### 设置某个应用中心上部某个特定的应用或者轮播图的view，如果设置开启轮播图，这个view会出现在轮播图上方。
```objc
定义：
-(void)setCustomAppHeaderView:(UIView *)appHeaderView;
示例代码：
[[MXAppCenter sharedInstance] setCustomAppHeaderView:testAppHeaderview];
```
### 设置应用中心轮播图下方的自定义View
```objc
定义：
-(void)setCustomAppHeaderViewBelowBanner:(UIView *)appHeaderView;
示例代码：
[[MXAppCenter sharedInstance] setCustomAppHeaderViewBelowBanner:testAppHeaderview];
```
### 设置自定义应用中心上部刷新view的背景颜色
```objc
定义：
-(void)setCustomAppRefreshViewBackgroundColor:(UIColor *)bgColor;
示例代码：
[[MXAppCenter sharedInstance] setCustomAppRefreshViewBackgroundColor:[UIColor mx_colorWithHexString:@"#ff5300"]];
```
### 设置应用中心刷新view的字体颜色
```objc
定义：
-(void)setCustomAppRefreshFontColor:(UIColor *)fontColor;
示例代码：
[[MXAppCenter sharedInstance] setCustomAppRefreshFontColor:[UIColor whiteColor]];
```
### 设置应用中心刷新View的菊花颜色
```objc
定义：
- (void)setCusomAppRefreshActivityColor:(UIColor *)color;
示例代码：
[[MXAppCenter sharedInstance]setCusomAppRefreshActivityColor:[UIColor yellowColor]];
```
### 设置应用中心应用的自动下载的大小的最大值,默认100k以下的应用会自动安装
```objc
定义：
-(void)setAutoDownloadMaxSize:(NSInteger)maxSize;
示例代码：
[[MXAppCenter sharedInstance] setAutoDownloadMaxSize:MX_APP_AUTO_DOWNLOAD_LIMIT_SIZE];
```
### 跳转到添加app页面
```objc
定义：
-(void)addApp;
示例代码：
[[MXAppCenter sharedInstance] addApp];
```
### 自定义的应用中心的viewcontroller跳转到添加app页面
parentvc：当前控制器
```objc
定义：
-(void)addApp:(UIViewController *)parentVC;
示例代码：
[[MXAppCenter sharedInstance] addApp:parentvc];
```
### 结束编辑模式
```objc
定义：
-(void)endEdit;
示例代码：
[[MXAppCenter sharedInstance] endEdit];
```
### 注册应用中心编辑开始的callback，编辑开始的时候会回调callback里面得代码
```objc
定义：
//编辑模式启动的时候，用户可以对应用进行排序
-(void)registEditBeginCallback:(finishCallback)callback;
示例代码：
[[MXAppCenter sharedInstance] registEditBeginCallback:^(id result, MXError *error) {
[navBarView setRightBtnImage:@"mx_finish_EN_phone"];
if ([[[NSLocale preferredLanguages] firstObject] hasPrefix:@"zh-"]){
  [navBarView setRightBtnImage:@"mx_finish_phone"];
 }
  editing = YES;
}];
```
### 注册应用中心编辑结束的callback，编辑结束的时候会回调callback里面得代码
```objc
定义：
-(void)registEditEndCallback:(finishCallback)callback;
示例代码：
[[MXAppCenter sharedInstance] registEditEndCallback:^(id result, MXError *error) {
editing = NO;
[navBarView setRightBtnImage:@"mx_btn_add_phone"];
 }];
```
### 应用中心的app加载完成之后回调callback里面得代码,第一次登录时应用中心加载完后会调用
```objc
定义：
-(void)registGetAppFinishCallback:(finishCallback)callback;
示例代码：
[[MXAppCenter sharedInstance] registGetAppFinishCallback:^(id result, MXError *error) {
  [navBarView setRightBtnUserInteractionEnabled:YES];
}];
```
### 注册应用中心的应用点击的事件，当用户点击应用中心的应用之后，会回调callback里面得方法，用户可以自由处理，处理完成之后可以调用appItemClick:(NSString*)appID来调用敏行的逻辑
```objc
定义：
-(void)registAppClickedCallback:(finishCallback)callback;
示例代码：
[[MXAppCenter sharedInstance] registAppClickedCallback:^(id result, MXError *error) {
 NSString *appID = [result copy];
 NSLog(@"appID===%@", appID);
 [[MXAppCenter sharedInstance] appItemClick:appID];
}];
```
### 点击未安装或未升级的应用时，返回这个应用的url及大小
```objc
定义：
- (void)registAppInstallOrUpgradeCallback:(finishCallback)callback;
示例代码：
[[MXAppCenter sharedInstance] registAppInstallOrUpgradeCallback:^(id result, MXError *error) {
NSLog(@"%@",result);
 }];
```
### 点击应用公众号消息跳转应用时的回调接口信息,触发时传入应用公众号对应的应用id，可以自己在回调中根据appid判断调整跳转逻辑
需要在初始化聊天控制器（demo里在showmain里）时注册该回调
```objc
定义：
-(void)registPublicAccountClickedCallback:(handleNativeCallback)callback;
示例代码：
[[MXAppCenter sharedInstance] registPublicAccountClickedCallback:^(id result, MXError *error) {
 "app_url" = "";
 url = "http://test.dehuinet.com:8030/dehuinet/apps/sso_redirect?ocu_id=ccb74893d3c14028524e9751a3770381&url=/mxpp/articles/29717?source_id=235898";
NSLog(@"result===%@", result);
 return NO;//返回NO表示第三方使用者自己已经处理，跳过敏行逻辑；YES表示继续进入敏行处理逻辑。
}];
```
### 外部处理完成之后再进入app
```objc
定义：
-(void)appItemClick:(NSString *)appID;
//extParam：传入参数形式a=1&b=2
-(void)appItemClick:(NSString *)appID withExtParame:(id)extParam;
示例代码：
[[MXAppCenter sharedInstance] appItemClick:appID];
[[MXAppCenter sharedInstance] appItemClick:appID withExtParame:@"a=1&b=2"];
```
### 设置应用中心各个应用的消息未读数
```objc
参数说明：
countDic参数里面存的是appID和未读数的key value
定义：
-(void)showBadgeCount:(NSDictionary *)countDic;
示例代码：
NSDictionary *dicCount = [[NSDictionary alloc] initWithObjectsAndKeys:[NSNumber numberWithInt:20],@"bde209e19cfebcfa2f719a52a4406441",[NSNumber numberWithInt:15],@"f3710a8b00b4924057446b53609b5038", nil];
[[MXAppCenter sharedInstance] showBadgeCount:dicCount];
```
### 设置应用中心某个应用隐藏
```objc
参数说明：
appsArray里面存得是appID的数组
定义：
-(void)hiddenApps:(NSArray *)appsArray;
示例代码：
[[MXAppCenter sharedInstance] hiddenApps:[NSArray arrayWithObjects:@"bde209e19cfebcfa2f719a52a4406441", nil]];
```
### 用户自定义appview的frame
```objc
定义：
//用户自定义frame，这样appview就可以放到指定的任意位置
-(void)setCustomViewFrame:(CGRect)frame;
//设置appCenter的ParentController，这个api需要跟上面的setCustomViewFrame配套使用。
-(void)setParentController:(UIViewController *)controller;
示例代码：
[[MXAppCenter sharedInstance] setCustomViewFrame:CGRectMake(0, 100, 320, 100)];
[[MXAppCenter sharedInstance] setParentController:self];
```
### 是不是只有一个应用中心， YES表示只有一个应用中心
```objc
定义：
-(void)setOnlyOneAppCenter:(BOOL) onlyOneAppCenter;
示例代码：
[[MXAppCenter sharedInstance]setOnlyOneAppCenter:YES];
```
### 手动调api来获取应用中心的应用信息, callback 里面得object 是一个包含NSDictionary的NSArray，每一个NSDictionary里面是每个应用的信息
```objc
定义：
-(void)getAppsMessageWithFinshCallback:(finishCallback)callback;
示例代码：
[[MXAppCenter sharedInstance] getAppsMessageWithFinshCallback:^(id object, MXError *error) {
//object 里面是NSDictionary的NSArray， NSDictionary的结构是key值为{avatar_url, name, ocuID, id, operationUrl}
 NSLog(@"apps message result == %@", object);
}];   
```
### 点击某个应用的事件
```objc
定义：
-(void)appClick:(NSString *)appID WithExtParam:(NSString *)extParam withViewController:(UIViewController*)VC;
示例代码：
[[MXAppCenter sharedInstance] appClick:appID WithExtParam:nil withViewController:self];
```
### 用接口点击没有安装的应用的时候的回调,在非应用中心页面用接口点击没有安装的应用，弹出alert，点击确定后会回调
```objc
定义：
-(void)registClickAppFailCallback:(finishCallback)callback;
示例代码：
[[MXAppCenter sharedInstance] registClickAppFailCallback:^(id result, MXError *error) {
NSString *appID = (NSString *)result;
NSLog(@"app not intall or need upgrade!!!!!!!, appID == %@", appID);
//跳转到应用中心
[tabBarController setSelectedIndex:[self getAppCenterIndex]];      
}];
```
### 切换内网
```objc
定义：
-(void)setSwitchNetworkInAppCenter:(BOOL)flag;
示例代码：
[[MXAppCenter sharedInstance] setSwitchNetworkInAppCenter:MX_ENABLE_NETWORK_CHANGE];
```
### 更新应用中心应用的角标数字
```objc
定义：
-(void)updateApp:(NSString *)app_id withBadgeCount:(int)badgeCount;
示例代码：
[[MXAppCenter sharedInstance]updateApp:appID withBadgeCount:0];
```
### 更新应用会话的角标数字
```objc
定义：
-(void)updateAppConversationFromAPI:(NSString *)app_id withBadgeCount:(int)badgeCount;
示例代码：
[[MXAppCenter sharedInstance]updateAppConversationFromAPI:appID withBadgeCount:0];
```
### 应用中心的设置
```objc
//应用中心禁止移动
[[MXAppCenter sharedInstance]initDiableAppItemMove:NO];
//设置应用中心增加右下角添加按钮
[[MXAppCenter sharedInstance]initEnableAppCenterAddButton:YES];
//是否显示应用中心的轮播图
[[MXAppCenter sharedInstance]initEnableAppCenterBanner:YES];
//设置一行显示几个app应用
[[MXAppCenter sharedInstance]initShowLineAppCount: 4];
//设置应用中心首页每行的item的数目
[[MXAppCenter sharedInstance] initShowMainAppCount:4];
//设置ipa竖屏时一行显示几个app应用
[[MXAppCenter sharedInstance]initShowLineAppCountIpadVertical:5];
//设置ipa横屏时一行显示几个app应用
[[MXAppCenter sharedInstance]initShowLineAppCountIpadHorizontal: 8];
//应用中心是否是分类展示
[[MXAppCenter sharedInstance] initShowApPItemByCategory:MX_SHOW_APP_CATEGORY];
// 禁用应用中心划线
[[MXAppCenter sharedInstance]initDisableDrawLine:NO];
```
### 注册滚动偏移量的回调，callback的返回的数据是一个NSDictionary， 分别代表滚动的方向和滚动停止的y轴坐标值
```objc
定义：
-(void)registScrollOffsetCallback:(functionCallback)callback;
示例代码：
[[MXAppCenter sharedInstance] registScrollOffsetCallback:^(NSDictionary *dataDic) {
if(dataDic) {
int scrollDiection = [[dataDic objectForKey:@"scrollDirection"] intValue];
CGFloat scrollYOffset = [[dataDic objectForKey:@"scrollYOffset"] floatValue];
UIScrollView *appScrollView = [[MXAppCenter sharedInstance] getAppScrollView];
if(scrollDiection == 1) {
//down
if(scrollYOffset > -appCustomHeaderViewHeight && scrollYOffset < -5) {
    [UIView animateWithDuration:0.5 animations:^{
     testAppHeaderview.alpha = 1.0f;
     appScrollView.contentOffset = CGPointMake(0, -appCustomHeaderViewHeight);
   [[[MXAppCenter sharedInstance] getAppCenterViewController].view layoutIfNeeded];
      }];
   }
} else if(scrollDiection == 2) {
//up
if(scrollYOffset > -appCustomHeaderViewHeight + 5 && scrollYOffset < 0) {
    [UIView animateWithDuration:0.5 animations:^{
    testAppHeaderview.alpha = 1.0f;
    appScrollView.contentOffset = CGPointMake(0, 0);                  
   [[[MXAppCenter sharedInstance] getAppCenterViewController].view layoutIfNeeded];
      }];
    }
  } else {
       testAppHeaderview.alpha = 1.0f;
        }
    }
}];
```
### 应用中心下拉刷新触发后会回调这个callback
```objc
定义：
-(void)registAppCenterRefreshCallback:(functionCallback)callback;
示例代码：
[[MXAppCenter sharedInstance] registAppCenterRefreshCallback:^(id obj) {
 //do some refresh
 [[MXAppCenter sharedInstance] thirdAppRefreshFinished];
}];
```
### 第三方应用刷新完毕需要调用这个方法来更新应用中心下拉的状态
```objc
定义：
-(void)thirdAppRefreshFinished;
示例代码：
[[MXAppCenter sharedInstance] thirdAppRefreshFinished];
```
### 获得应用的ScrollView
```objc
定义：
-(UIScrollView *)getAppScrollView;
示例代码：
[[MXAppCenter sharedInstance]getAppScrollView];
```
### 设置app图标大小
```objc
定义：
-(void)setAppIconSize:(CGFloat)size;
示例代码：
[[MXAppCenter sharedInstance]setAppIconSize:size];
```
### 刷新nativeapp的状态，版本检测,在应用中心viewwillappear中调用,防止安装完第三方app之后安装状态不对
```objc
定义：
-(void)refreshNativeAppStatus;
示例代码：
[[MXAppCenter sharedInstance]refreshNativeAppStatus];
```
### 设置请求超时时间间隔 单位是秒
```objc
定义：
-(void)setRequsetTimeoutInterval:(NSTimeInterval)timeoutInterval;
示例代码：
[[MXAppCenter sharedInstance]setRequsetTimeoutInterval:30s];
```
### 刷新应用中心view
```objc
定义：
-(void)refreshAppsView;
示例代码：
[[MXAppCenter sharedInstance]refreshAppsView];
```
### 获取应用中心应用下载进度
```objc
定义：
-(void)registAppDownloadingStatus:(functionCallback)callback;
示例代码：
[[MXAppCenter sharedInstance] registAppDownloadingStatus:^(id object) {
  NSLog(@"appdownloadStatus======%@", object);
}];
```
### 检测license是否可用
```objc
参数说明：
licenseCode：license编码
定义：
//yes:可用  NO：不可用
-(BOOL)checkLicense:(NSString *)licenseCode;
示例代码：
BOOL enabled = [[MXAppCenter sharedInstance] checkLicense:@"video_chat"];
```
### 检测appItem是否需要密码认证
```objc
appDic：app信息
定义：
- (BOOL)checkAppItemAuthenticationWithDic:(NSMutableDictionary *)appDic;
示例代码：
BOOL enabled = [[MXAppCenter sharedInstance] checkAppItemAuthenticationWithDic:dic];
```
### 清理应用中心缓存
```objc
参数说明：
loginName： 登录名
定义：
-(void)cleanAppItemsCache:(NSString *)loginName;
示例代码：
[[MXAppCenter sharedInstance] cleanAppItemsCache:@"小明"];
```
### 处理插件点击事件
```objc
定义：
-(void)handleAppClickWithSchemeUrl:(NSString *)schemeUrl withExtParm:(NSString *)extParams withParentVC:(UIViewController*)parentVC withActionCallback:(functionCallback)actionCallback;
示例代码：
NSString *schemeURL=@"video://";
[[MXAppCenter sharedInstance] handleAppClickWithSchemeUrl:schemeURL withExtParm:nil withParentVC:self.viewController withActionCallback:^(id object) {

}
```
### 获取应用config配置
```objc
参数说明：
app_id：应用id
定义：
-(NSString *)loadNativeConfig:(NSString *)app_id;
示例代码：
NSString *pluginConfig=[appcenter loadNativeConfig:appid];
NSDictionary *configDic=[pluginConfig JSONValue];
```
### 获取应用名字
```objc
参数说明：
appid: 应用id
定义：
- (NSString *)getAppNameByAppID:(NSString *)appid;
示例代码：
NSString *appName=[appcenter getAppNameByAppID:appid];
```














