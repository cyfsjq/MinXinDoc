# MXContacts接口说明文档
## 1.引入头文件
```objc
#import <MXKit/MXContacts.h>
```
## 2.初始化MXContacts
### 2.1获取MXContacts对象
```objc
定义：
+(MXContacts *)sharedInstance;
示例代码：
[MXContacts sharedInstance];
```
### 2.2获取通讯录的ViewController
```objc
定义：
-(UIViewController *)getContactsViewController;
示例代码;
UIViewController *contacts = [[MXContacts sharedInstance] getContactsViewController];
```
### 2.3自定义通讯录的header
```objc
定义：
-(void)setCustomHeaderView:(UIView *)headerView;
示例代码：
[[MXContacts sharedInstance] setCustomHeaderView:contactsHeader];
```
### 2.4释放通讯录的viewController
```objc
定义：
-(void)cleanContactsViewController;
示例代码：
[[MXContacts sharedInstance] cleanContactsViewController];
```
## 3.MXContacts模块详细API使用说明
### 添加联系人
```objc
定义：
-(void)addContact;
示例代码：
[[MXContacts sharedInstance] addContact];
``` 
### 右滑contact界面的时候回调
```objc
定义：
-(void)startPullDownWithCallback:(finishCallback)callback;
示例代码：
[[MXContacts sharedInstance] startPullDownWithCallback:^(id result, MXError *error) {
if (result) {
   [contactsHeader startPullDown];
   }
}];
```
### 获取人员信息，并显示人员信息详情页
```objc
参数说明：
loginName： 是要获取的人员的登录名
VC： 是将要push聊天view的viewController
定义：
-(void)personInfo:(NSString *)loginName withIndicator:(BOOL)showIndicator withViewController:(UIViewController *)VC;
示例代码：
[[MXContacts sharedInstance] personInfo:@"1065" withIndicator:YES withViewController:self];
```
### 获取人员信息，结果放在callback中返回
```objc
参数说明：
loginName 是要获取的人员的登录名
定义：
-(void)getPersonInfo:(NSString *)loginName withIndicator:(BOOL)showIndicator withCallback:(finishCallback)callback;
示例代码：
[[MXContacts sharedInstance] getPersonInfo:@"1065" withIndicator:YES withCallback:^(id object, MXError *error) {
NSData* jsonData = [NSJSONSerialization dataWithJSONObject:(NSDictionary *)object options:NSJSONWritingPrettyPrinted error:nil];                                                                              
NSString *jsonString = [[NSString alloc] initWithData:jsonData encoding:NSUTF8StringEncoding];
UIAlertView *alter = [[UIAlertView alloc] initWithTitle:@"信息" message:jsonString delegate:nil cancelButtonTitle:@"OK" otherButtonTitles:nil];
[alter show];
}];
```
### 选择人员，isMult = YES 是多选
```objc
参数说明:
isMult： 是否是多选模式
title： 标题
isAllDept：是否是所有部门
VC：传入当前控制器，用于跳转
定义:
-(void)selectUser:(BOOL)isMult withTitle:(NSString *)title withAllDept:(BOOL)isAllDept withViewController:(UIViewController *)VC withCallback:(finishCallback)callback;
示例代码：
[[MXContacts sharedInstance] selectUser:YES withTitle:@"text" withAllDept:YES withViewController:self withCallback:^(id object, MXError *error) {
NSData* jsonData = [NSJSONSerialization dataWithJSONObject:(NSDictionary *)object                                                      options:NSJSONWritingPrettyPrinted error:nil];
NSString *jsonString = [[NSString alloc] initWithData:jsonData encoding:NSUTF8StringEncoding];
UIAlertView *alter = [[UIAlertView alloc] initWithTitle:@"信息" message:jsonString delegate:nil cancelButtonTitle:@"OK" otherButtonTitles:nil];
[alter show];
}];
```
### 选择人员，从应用中心进入视频会议，isMult = YES 是多选（modal模式）
```objc
参数说明：
isMult: 是否是多选
title：标题
isAllDept： 是否是所有部门
VC：传入当前控制器，用于跳转
selectContacts：选择的人员
定义：
-(void)selectAVConferenceContact:(BOOL)isMult withTitle:(NSString *)title withAllDept:(BOOL)isAllDept withViewController:(UIViewController *)VC selectContacts:(NSMutableArray *)selectContacts  withCallback:(finishCallback)callback;
示例代码：
[[MXContacts sharedInstance]selectAVConferenceContact:YES withTitle:@"text" withAllDept:YES withViewController:self selectContacts:selectContactsArr withCallback:^(id object, MXError *error) {
。。。。。。
}];
```
### 视频会议时用到的选人界面
```objc
定义：
-(void)selectAVConferenceContactWithTitle:(NSString *)title topViewController:(UIViewController *)vc usersIDS:(NSArray *)usersIDS selectContacts:(NSMutableArray *)selectContacts callback:(finishCallback)callback;
示例代码：
[[MXContacts sharedInstance]selectAVConferenceContactWithTitle:@"text" topViewController:vc usersIDS:usersIDSArr selectContacts:selectContactsArr callback:^(id object, MXError *error) {
。。。。。。
}];
```
### 获得来电显示设置界面
```objc
定义：
-(UIViewController *)getContactSettingViewController;
示例代码：
UIViewController *contactSetting=[[MXContacts sharedInstance] getContactSettingViewController];
```
### 清除来电显示设置
```objc
定义：
-(void)clearContactSettingViewController;
示例代码：
[[MXContacts sharedInstance] clearContactSettingViewController];
```
### 聊天界面点击名片，显示可选择的通讯录（手机通讯录或企业通讯录）
```objc
定义：
-(void)showSelectContacterView:(dataCallback)callback;
示例代码：
[[MXContacts sharedInstance]showSelectContacterView:^(id data) {
  NSLog(@"名片result===%@", data);          
}];
```
### 配置通讯录发短信，敏信，打电话功能
```objc
定义：
//加在初始化敏行参数的位置
-(void)setPhoneMessageShow:(BOOL)phoneMessageShow withMessageShow:(BOOL)messageShow withPhoneShow:(BOOL)phoneShow;
示例代码：
[[MXContacts sharedInstance] setPhoneMessageShow:YES withMessageShow:YES withPhoneShow:YES];
```
### 设置群聊，公众号，公司通讯录以及特别关注的显示顺序, displayOrder越大越靠前
```objc
定义：
-(void)setMultiChatDisplayOrder:(int)displayOrder;
-(void)setPublicAccountDisplayOrder:(int)displayOrder;
-(void)setVipDisplayOrder:(int)displayOrder;
-(void)setContactDisplayOrder:(int)displayOrder;
示例代码：
[[MXContacts sharedInstance] setMultiChatDisplayOrder:0];
[[MXContacts sharedInstance] setPublicAccountDisplayOrder:1];
[[MXContacts sharedInstance] setVipDisplayOrder:2];
[[MXContacts sharedInstance] setContactDisplayOrder:3];
```
### 通讯录界面添加自定义cell(与群聊、公众号、特别关注、通讯录并齐)
```objc
参数说明：
key: 自定义通讯录的key
name: 显示的名字
avatar: 头像地址
displayOrder: 显示顺序
定义：
-(void)setCustomContactKey:(NSString *)key withName:(NSString *)name withAvatarUrl:(NSString *)avatar withDisplayOrder:(int)displayOrder;
示例代码：
[[MXContacts sharedInstance] setCustomContactKey:@"testCustomKey" withName:@"生活服务" withAvatarUrl:@"" withDisplayOrder:2];
```
### 注册通讯录界面自定义cell点击的回调
```objc
定义：
-(void)registCustomContactClickCallback:(finishCallback)callback;
示例代码：
[[MXContacts sharedInstance] registCustomContactClickCallback:^(id key, MXError *error) {
   NSString *customKey = (NSString *)key;
   NSLog(@"custom contact click key ==== %@", customKey);
}];
```
### 显示公众号列表，VC是parent viewcontroller
```objc
定义：
-(void)showPublicAccount:(UIViewController *)VC;
示例代码：
[[MXContacts sharedInstance] showPublicAccount:[[MXContacts sharedInstance] getContactsViewController]];
```
### 显示保存的群聊列表，VC是parent viewcontroller
```objc
定义：
-(void)showMultiChat:(UIViewController *)VC;
示例代码：
[[MXContacts sharedInstance] showMultiChat:self];
```
### 启动隐藏手机号
```objc
定义：
-(void)initDisableShowPhoneNumber:(BOOL)disableshowPhoneNumber;
示例代码：
[[MXContacts sharedInstance] initDisableShowPhoneNumber:MX_DISABLE_SHOWPHONENUMBER];
```
### 启动隐藏添加联系人功能
```objc
定义：
-(void)initDisableAddContacts:(BOOL)disableAddContacts;
示例代码：
[[MXContacts sharedInstance] initDisableAddContacts:MX_DISABLE_ADDCONTACTS];
```
### 是否隐藏在线状态
```objc
定义：
-(void)initEnableOnlineStatus:(BOOL)enableOnlieStatus;
示例代码：
[[MXContacts sharedInstance] initEnableOnlineStatus:MX_ENABLE_ONLINE_STATUS];
```