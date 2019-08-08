奇优免费小说SDK对接文档
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
    implementation 'com.youshuge.ysynovel:ysysdk:1.0.5'
}
```

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
-keep public inteface com.youshuge.novelsdk.** { *; }
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
Intent intent = new Intent(context,YSYMainActivity.class)
startActivity(intent);
```






