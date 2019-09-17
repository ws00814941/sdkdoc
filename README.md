乐书屋小说SDK对接文档
--
### 快速开始

#### 一、获取AppId和AppSecret

暂不支持后台获取，请联系商务获取您的appid和appsecret

#### 二、导入sdk依赖的包
1.将下方代码添加到您项目文件的build.gradle中

```
allprojects {
    repositories {
        maven {
            url "https://dl.bintray.com/meatballcc/maven"
        }
    }
}
```
2.将下方代码添加到您app目录的build.gradle中

```
dependencies {
    implementation 'com.youshuge.ysynovel:ysysdk:1.1.7'
}

//sdk引用了appcompat的依赖版本号为28.0.0 请修改您的appcompat版本，避免重复引用，
```
<font color=#f00>请将项目的compileSdkVersion，targetSdkVersion改成28</font>
避免编译时报错
#### 三、添加权限
使用sdk需要必要的权限
```
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.VIBRATE"/>
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
    <uses-permission android:name="android.permission.CAMERA"/>
    <uses-permission android:name="android.permission.READ_PHONE_STATE"/>
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
    <uses-permission android:name="android.permission.CHANGE_WIFI_STATE"/>
```
Android6.0开始部分权限需要动态申请，因此请在入口函数中添加以下代码，确保申请到权限，不然影响用户数据的统计
```
 YSYSDK.getManager().requestPermissions(context,new PermissionCallback() {
                    @Override
                    public void onPermissionGranted() {
                    	Log.d(TAG,"权限通过");
                    }

                    @Override
                    public void onPermissionDenied() {
                      	Log.d(TAG,"权限拒绝");
                    }
                });
```

#### 四、代码混淆
请按照下图所示配置代码混淆，确保不要混淆sdk的代码
```
#novelsdk
-dontwarn com.youshuge.novelsdk.**
-keep class com.youshuge.novelsdk.** { *; }
-keep public interface com.youshuge.novelsdk.** { *; }
```

#### 五、初始化SDK
请在Application类中的onCreate方法中调用以下代码初始化SDK
```
  YSYSDK.init(context, new YSYConfig.Builder()
                .debugMode(true)//打开调试模式查看日志，正式环境请关闭
                .appId("your appid")
                .appSecret("your appsecret")
                .build());
```
#### 六、启动SDK
请在相应的事件中添加下列代码，启动小说界面
```
  YSYSDK.getManager().enter();
```

### 阅读激励设置指南
请联系商务开通阅读激励。

开启阅读激励需要读取手机的imei同时关联app的用户id
相关代码
```
    YSYSDK.getManager().requestPermissions(context, new PermissionCallback() {
                    @Override
                    public void onPermissionGranted() {
                    }

                    @Override
                    public void onPermissionDenied() {
                    }
                });
    //关联app的用户id
    YSYSDK.getManager().setUserID("user_id");
```
回调接口
**GET** http://xxx.xx?user_code=1&award_amount=100

|  参数 | 描述 |
| :-  | :-  |
| url | 回调奖励金地址（后台填写） |
| user_code | 用户唯一标识 |
| award_amount | 奖励金额（后台填写） |

```
返回成功结果示例： 
{
    "code":200,
    "msg":"成功",
    "err":""
}
返回失败结果示例:
{
    "code":-1,
    "msg":"失败",
    "err":"用户标识不存在"
}

```
**注意返回结果请按给定格式返回，否则用户无法收到奖励**




