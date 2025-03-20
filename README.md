## 一、插件介绍
此插件是 openinstall 为了方便 uni-app 集成使用 openinstall 功能而开发的，实现了携带参数安装，渠道统计(H5渠道等)，一键拉起全部功能。

`openinstall` 可帮助 Android/iOS 开发者精确的获取 App 每一次安装来源；在 App 安装或拉起后，直达指定场景，大大提高用户转化率和留存率。同时，openinstall 在精准的 app 安装来源跟踪的技术上，开发了免打包，跨平台的渠道统计功能，轻松创建与管理成千上万的渠道，实现线上线下全渠道覆盖。openinstall 统计数据完全独立于第三方平台，提供多维度的统计报表，实时客观地反映渠道效果。

## 二、集成准备
前往 [openinstall 官网](https://www.openinstallglobal.com/)，注册账户，登录管理控制台，创建应用后，跳过 "集成指引"，在 "应用集成" 的对应平台的 "应用配置" 中获取 `appkey` 和 `scheme` 以及 iOS 的关联域名。

![获取appkey和scheme](https://web.cdn.openinstallcloud.com/doc/ios-appkey.png)

#### 配置appkey
在 mainfest.json 的 **App原生插件配置** 的 openinstall 插件配置框内配置 `openinstall` 分配给应用的 `AppKey`  
![设置appkey](https://web.cdn.openinstallcloud.com/doc/uniapp-appkey.jpg)
#### 配置scheme
在 mainfest.json 的 **App常用其它配置** 中配置 `openinstall` 分配给应用的 `scheme`    
![设置scheme](https://web.cdn.openinstallcloud.com/doc/uniapp-scheme.jpg)
#### 配置universal links（iOS平台）
1、开启Associated Domains服务  
需要在苹果开发者后台开启 [苹果开发者后台](https://developer.apple.com)  

2、在HBuilderX里面配置关联域（Associated Domains）  
示例如图：  
![配置关联域名](https://web.cdn.openinstallcloud.com/doc/uniapp-applinks.png)  

## 三、使用教程

#### 引用
``` js
const openinstall = uni.requireNativePlugin('openinstall-plugin-global');
```

#### 初始化
`init()`    
示例：在 `App.vue` 的 `onLaunch` 方法中进行初始化
``` js
openinstall.init();
```
>**注意：必须先进行初始化，才能调用其他api

#### 获取安装数据
`getInstall(seconds, callback)`
- `seconds` : 回调超时时间
- `callback` : 数据回调函数

示例：
``` js
openinstall.getInstall(
    8,
    function(result) {
        console.log('getInstall : channel=' + result.channelCode + ', data=' + result.bindData 
            + ', shouldRetry=' + result.shouldRetry);
    }
);
```

#### 获取拉起数据
 
`registerWakeUp(callback)`  
- `callback` : 数据回调函数  

示例：
在 `App.vue` 的 `onLaunch` 方法中注册拉起回调（在初始化之后调用 ） 
``` js
openinstall.registerWakeUp(function(result){
    console.log('getWakeup : channel=' + result.channelCode + ', data=' + result.bindData);
});
```
#### 注册量统计
`reportRegister()`  
示例：
``` js
openinstall.reportRegister();
```

#### 效果点统计
`reportEffectPoint(effectPointId, effectPointValue)`
- `effectPointId` : 效果点ID
- `effectPointValue` : 效果点值，数值类型

示例：
``` js
openinstall.reportEffectPoint("effect_test", 1);
```

#### 效果点明细统计
`reportEffectPoint(pointId, pointValue, extras)`
- `pointId` : 效果点ID
- `pointValue` : 效果点值，数值类型
- `extras` : 效果点自定义参数和值,key和value都必须是string类型

示例：
``` js
var extras = {
    "key1": "value1",
    "key2": "value2",
}
openinstall.reportEffectPoint("effect_detail", 1, extras);
```

## 四、导出apk/ipa包并上传
- 代码集成完毕后，需要导出安装包上传openinstall后台，openinstall会自动完成所有的应用配置工作。  
![上传安装包](https://web.cdn.openinstallcloud.com/doc/upload-ipa-jump.png)
- 上传完成后即可开始在线模拟测试，体验完整的App安装/拉起流程；待测试无误后，再完善下载配置信息。
![在线测试](https://web.cdn.openinstallcloud.com/doc/js-test.png)

