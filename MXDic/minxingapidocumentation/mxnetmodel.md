# MXNetModel接口说明文档
## 1.引入头文件
```objc
#import <MXKit/MXNetModel.h>
```
## 2.敏行API通道
```objc
参数说明：
method:为HTTP得方法有这么几种 "GET", "POST", "DELETE", "PUT"  
url:为http请求的api方法，可以是相对路径，也可以是绝对路径  
params:请求的参数 
headers:请求头 
showIndiator:为是否显示等待提示框  
invoke:为response处理block  
定义：
-(void)startRequset:(NSString *)method withURL:(NSString *)url withParams:(NSDictionary *)params withHeaders:(NSDictionary *)headers withIndicator:(BOOL)showIndiator withCallback:(requsetCallback) invoke;
示例代码：
[[MXNetModel shareNetModel] startRequset:@"GET" withURL:"" withParams:nil withHeaders:nil withIndicator:YES withCallback:^(id object, MXError *error) {
 //handle callback        
 if(object && !error) {
   //处理response
    } else {
    //出错处理
   }
 }];
```



