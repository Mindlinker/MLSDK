# 产品简介
迈聆UI-SDK是一款轻量级的视频会议sdk，提供极简接口和易理解的文档减少开发工作量，提高集成效率，只需调用简单的API方法便可实现快速接入，支持一键创会、入会及丰富的会议功能，流畅的会议体验。全自研音视频编解码算法，支持最高1080P/30fps 高清视频，48Hz音频编码，成熟音频算法，带来极致音视频体验 。

## 核心功能模块
![](https://www.mindlinker.com/doc/assets/img.c29ee815.png)

## 核心流程
app依赖我们的sdk后，只需要在应用初始化的同时进行sdk的初始化并在用户登录后传入token进行sdk的校验，只需要提供一个Activity作为载体，便可一键创建房间或者加入房间，享受视频会议功能。

![](https://www.mindlinker.com/doc/assets/img_1.58cd204d.png)

# 版本更新记录
## V1.0.2
#### 升级时间
2022/08/11

#### 升级内容
1. 增加视频设置
2. 参会人管理页面优化
3. 添加 jwt 验证方式
4. bugsfix

## V1.0.0

#### 升级时间
2022/04/01

#### 升级内容
1. 支持多方视频会议
2. 发起和接受共享
3. 参会人管理

# AppKey获取
## 申请 AppKey 和 AppSecret

目前阶段只能通过联系Mindlinker研发申请企业专属`AppKey`和`AppSecret`，在 [AuthCode 获取](https://www.mindlinker.com/doc/client-sdk/android/baseUse/authCode.html) 时需要用到。
通过下方的企业微信二维码联系申请：<br/>
![An image](https://www.mindlinker.com/doc/img/wechat.png)

# 快速跑通 Demo
### 开发环境
- Android Studio 3.0 以上
- Android minSdkVersion: 24
- Gradle: 3.5 及以上



### 操作步骤
#### Step 1：下载 Demo
- 方式 1：git clone
```kotlin
git clone https://github.com/mindlinker/UISDKDemo.git
```

- 方式 2：[下载 Demo 压缩包](https://github.com/mindlinker/UISDKDemo/archive/refs/heads/main.zip)


#### Step 2：打开项目
线下打开 Android Studio，选择 File -> Open，找到刚刚下载的目录，选择 UISDKDemo ，点击 Open，完成项目的打开工作

![](https://www.mindlinker.com/doc/assets/openFile.152e0b03.png)

#### Step 3：修改 AppKey 和 AppSecret
找到类 Constanct，然后修改 AppKey 和 AppSecret，如果还没有 AppKey 和 AppSecret，那么可以先去 [申请 AppKey](#appkey获取)

```kotlin
object Constanct {
    const val APP_ID = ""           //  设置申请到的 AppKey
    const val APP_SECRET = ""       //  设置申请到的 AppSecret
}
```

#### Step 4：编译运行
确保测试设备与 Android Studio 已经正常连接，点击运行即可将测试 Demo 运行到真机进行体验

![](https://www.mindlinker.com/doc/assets/buildTest.7941cea4.png)

#### Step 5：运行效果
![](https://www.mindlinker.com/doc/assets/buildResult.4d4f9346.png)

### 常见错误
如果遇到以下错误，很有可能是 jdk 版本不对
```kotlin
Caused by: java.lang.ExceptionInInitializerError: Exception org.codehaus.groovy.GroovyBugError [in thread "Daemon worker"]
```

![](https://www.mindlinker.com/doc/assets/buildError.cc05e396.png)

打开 File -> Project Structure -> SDK -> Location -> Gradle Settings，选择 Gradle JDK：11 version，最后再 Sync Project with Gradle Files

![](https://www.mindlinker.com/doc/assets/gradleSetting.066c983e.png)
![](https://www.mindlinker.com/doc/assets/jdk_11.9b25f058.png)


# 引入 SDK
### Step 1：指定依赖仓库
项目的根路径下的 build.gradle 文件添加以下依赖

```groovy
allprojects {
    repositories {
        //...
        maven { url 'https://jitpack.io' }
    }
}
```

### Step 2：指定依赖项
将依赖项添加到您的应用程序 build.gradle 文件中
```groovy
dependencies {
    implementation 'com.github.mindlinker:mlsdk:1.0.2-stable'
}
```


### Step 3：代码混淆
如果您的应用使用了代码混淆，请在 proguard-rules.pro 文件中添加如下配置，其中 MLSDK 中会使用到 Glide、RxJava、OkHttp 等第三方库，需要添加对应的混淆配置。

```kotlin
-keep class com.mindlinker.sdk.model.** {*;}
-keep class com.mindlinker.maxme.** { *; }

-dontwarn org.webrtc.**
-keep class org.webrtc.**{*;}
-keepclassmembers,includedescriptorclasses class * { native <methods>; }

-dontwarn com.maxhub.liblogreporter.**
-keep public class com.maxhub.liblogreporter.**{*;}

-keepclassmembers class * {
   public <init> (org.json.JSONObject);
}

-keepclassmembers enum * {
    public static **[] values();
    public static ** valueOf(java.lang.String);
}

################# sdk中用到的第三方库混淆 #################

# Retrofit 2.X
-dontwarn retrofit2.**
-keep class retrofit2.** { *; }
-keepattributes Signature
-keepattributes Exceptions

-keepclasseswithmembers class * {
    @retrofit2.http.* <methods>;
}

# rxjava
-dontwarn io.reactivex.**
-keep class io.reactivex.** { *; }
-keep class * extends io.reactivex.** { *; }
-dontwarn sun.misc.**

# okhttp
-keepattributes Signature
-keepattributes *Annotation*
-keep class okhttp3.** { *; }
-keep interface okhttp3.** { *; }
-dontwarn okhttp3.**

# okio
-keep class sun.misc.Unsafe { *; }
-dontwarn java.nio.file.*
-dontwarn org.codehaus.mojo.animal_sniffer.IgnoreJRERequirement
-dontwarn okio.**

# Glide
-keep public class * implements com.bumptech.glide.module.GlideModule
-keep public class * extends com.bumptech.glide.module.AppGlideModule
-keep public enum com.bumptech.glide.load.ImageHeaderParser$** {
  **[] $VALUES;
  public *;
}

# Gson
-keep class com.google.gson.**{*;}
```

# 初始化 SDK

### 功能介绍
SDK 初始化建议在 Application 的 onCreate 函数中进行，调用 MLApi.init(context, options) 就可以进行 sdk 的初始化。

### 示例代码
```kotlin
class MLApp : Application() {

    override fun onCreate() {
        super.onCreate()
       initMaxME(this)
    }

    private fun initMaxME(context: Application) {
        val options = MLOptions()
        options.logPath = "/sdcard/UI_SDK"  // 日志输出路径
        options.enableLog = true            // 是否开启日志
        options.enableConsoleLog = true     // 是否在控制台输出日志打印
        MLApi.init(context, options)
    }
}
```

### 参数说明
| 参数名称 | 参数类型 | 是否必填 | 参数描述 |
| --- | --- | --- | --- |
| enableConsoleLog | Boolean | 否 | 是否在控制台输出日志打印 |
| enableLog | Boolean | 否 | 是否开启日志 |
| logPath | String | 是 | 本地日志保存路径，只有enableLog为true的情况下，才会进行日志的写入 |

# 获取 AuthCode
### 功能介绍
AuthCode 根据 JWT 协议生成的，后续 [登录授权](#登录授权) 需要传给 MLApi.authenticate，目的是为了校验 APP 的身份，[了解 AuthCode](https://www.mindlinker.com/doc/rest-api/apis/auth/auth-code.html)

### 示例代码
::: warning
以下获取的 Jwt Token 是为了方便客户端在测试阶段方便调试使用，正式使用时建议从后台生成后获取
:::

```groovy
    implementation 'com.auth0:java-jwt:4.0.0' // 快速生成 jwt token 依赖库
```

```kotlin
    // todo: 正式版的话，为了安全起见，appkey 和 appSecret 是保存在后台服务器的，这个 AuthCode 是由后台返回给到客户端的，
//  客户端这边拿到 authCode 之后传给 MLApi，进行账号登录和验证
private fun createAuthCode(userName: String, avatar: String): String {
    val openId = Settings.System.getString(contentResolver, Settings.Secure.ANDROID_ID)
    val algorithm: Algorithm = Algorithm.HMAC256(Constanct.APP_SECRET)
    val hash = HashMap<String, String>()
    hash["nickname"] = userName
    hash["openId"] = openId
    hash["avatar"] = avatar
    return JWT.create()
        .withClaim("appKey", Constanct.APP_ID)
        .withClaim("userInfo", hash)
        .withIssuedAt(Date())
        .sign(algorithm)
}
```

# 登录授权

### 功能介绍
在完成 [SDK 初始化](initSdk.md) 调用和 [获取 AuthCode](authCode.md) 后需要进行 sdk 登录授权，授权成功后就可以创建会议和加入会议了，具体调用如下 Api 进行

```kotlin
MLApi.authenticate(serverUrl: String, token: String, userName: String, avatar: String, listener: AuthCallback）
```

### 示例代码
```kotlin
class LoginActivity : Activity {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        authenticate()
    }
    
    // sdk 初始化后调用
    private fun authenticate() {
        val serverUrl = ""
        val nickName = "mindlinker"
        val avatar = ""
        val authCode = createAuthCode(nickName, avatar)
        MLApi.authenticate(serverUrl, authCode, nickName, avatar, object : AuthCallback {

            override fun onSuccess() {
                // 该方法成功回调之后就可以创建会议和加入会议了
            }

            override fun onError(code: Int, msg: String) {}
        })
    }

    // todo: 正式版的话，为了安全起见，appkey 和 appSecret 是保存在后台服务器的，这个 AuthCode 是由后台返回给到客户端的，
    //  客户端这边拿到 authCode 之后传给 MLApi，进行账号登录和验证
    private fun createAuthCode(nickName: String, avatar: String): String {
        val openId = Settings.System.getString(contentResolver, Settings.Secure.ANDROID_ID)
        val algorithm: Algorithm = Algorithm.HMAC256(Constanct.APP_SECRET)
        val hash = HashMap<String, String>()
        hash["nickname"] = nickName
        hash["openId"] = openId
        hash["avatar"] = avatar
        return JWT.create()
            .withClaim("appKey", Constanct.APP_ID)
            .withClaim("userInfo", hash)
            .withIssuedAt(Date())
            .sign(algorithm)
    }
}
```

### 参数说明
| 参数名称 | 参数类型 | 是否必填 | 参数描述 |
| --- | --- | --- | --- |
| serverUrl | String | 是 | 服务器地址，即在迈聆开放平台申请的房间服务器地址 |
| token | String | 是 | AuthCode，根据 jwt 协议生成 |
| userName | String | 是 | 用户名称 |
| avatar | String | 是 | 用户头像 |
| callback | String | 是 | AuthCallback |

#### AuthCallback
```kotlin
interface AuthCallback {

    /**
     * 授权成功
     */
    fun onSuccess()

    /**
     * 登录授权失败
     * @param code 错误码
     * @param msg 错误信息
     */
    fun onError(code: Int, msg: String)
}
```

# 创建会议
### 功能介绍
创建会议，使用 Api 是：MLApi.createMeeting()，结果回调：MeetingCallback
```kotlin
MLApi.createMeeting(
        isMuteVideo: Boolean = true,    // 是否关闭摄像头进入房间
        isMuteAudio: Boolean = true,    // 是否关闭麦克风进入房间
        nickName: String = "",          // 用户名称
        avatar: String = "",            // 用户头像
        callback: MeetingCallback)      // 结果回调  
```

### 示例代码
```kotlin
    fun createMeeting(edUserName: String, isMuteVideo: Boolean, isMuteAudio: Boolean) {
        mView?.showLoading(true)
        MLApi.createMeeting(
            isMuteVideo = isMuteVideo, 
            isMuteAudio = isMuteAudio, 
            nickName = "", 
            avatar = "",
            callback = object : MeetingCallback {
                override fun onMeetingExist(roomId: String, sessionId: String) {
                    mView?.showTwoButtonDialog("你之前有创建的房间未结束 \n房间号：$roomId", roomId, sessionId)
                    mView?.showLoading(false)
                }

                override fun onSuccess(data: MLRoomInfo) {
                    Log.d(TAG, "on meeting create success data: $data")
                    mView?.createAndJoinMeetingSuccess(data)
                    mView?.showLoading(false)
                }

                override fun onError(code: Int, msg: String) {
                    Log.d(TAG, "on meeting create failure $code")
                    mView?.showToast(msg)
                    mView?.showLoading(false)
                }

                override fun alreadyInMeeting() {
                    mView?.showToast("你已经在房间中了")
                    mView?.showLoading(false)
                }
            }
        )
    }
```


### 参数说明
MLApi.createMeeting

| 参数名称 | 参数类型 | 是否必填 | 参数描述 |
| --- | --- | --- | --- |
| isMuteVideo | Boolean | 是 | 是否关闭摄像头进入房间 |
| isMuteAudio | Boolean | 是 | 是否关闭麦克风进入房间 |
| nickName | String | 是 | 入会用户名称 |
| avatar | String | 是 | 用户头像 |
| callback | MeetingCallback | 是 | 创建会议结果回调 |

#### MLRoomInfo
会议信息
| 参数名称 | 参数类型  | 参数描述 |
| --- | --- | --- |
| id | String | sessionId，房间的唯一标识 |
| roomNo | String | 房间号 |
| password | String | 房间密码 |
| isAudioMute | Boolean | 麦克风关闭状态，默认为true |
| isVideoMute | Boolean | 摄像头关闭状态，默认为true |
| isRejoin | Boolean | 成员是否重新加入房间,默认为false |



#### MeetingCallback
创建会议结果回调

```kotlin
interface MeetingCallback {

    /**
     * 有创建的会议未结束
     * @param roomId    房间号
     * @param sessionId 房间的唯一标识，用于房间外结束房间的id
     */
    fun onMeetingExist(roomId: String, sessionId: String)

    /**
     * 创建会议成功
     * @param data 会议房间信息
     */
    fun onSuccess(data: MLRoomInfo)

    /**
     * 创建会议失败
     * @param code：失败的错误码
     * @param msg：错误的提示语
     */
    fun onError(code: Int, msg: String)

    /**
     * 成员已经在会中
     */
    fun alreadyInMeeting()
}
```

# 加入会议
### 功能介绍
加入会议，使用 Api 是：MLApi.joinMeeting()，结果回调：MeetingCallback
```kotlin
MLApi.joinMeeting(
        meetingNo: String,              // 房间号
        isMuteVideo: Boolean = true,    // 是否关闭摄像头进入房间
        isMuteAudio: Boolean = true,    // 是否关闭麦克风进入房间
        pwd: String = "",               // 入会密码
        nickName: String = "",          // 用户名称
        avatar: String = "",            // 用户头像
        callback: MeetingCallback)      // 结果回调  
```

### 示例代码
```kotlin
    fun joinMeeting(meetingNUmber: String, isMuteVideo: Boolean, isMuteAudio: Boolean, password: String) {
    mView?.showLoading(true)
    MLApi.joinMeeting(
            meetingNo = meetingNUmber,
            isMuteVideo = isMuteVideo,
            isMuteAudio = isMuteAudio,
            pwd = password,
            nickName = "",
            avatar = "",
            callback = object : MeetingCallback {
                override fun onMeetingExist(roomId: String, sessionId: String) {
                    mView?.showTwoButtonDialog("你之前有创建的房间未结束 \n房间号：$roomId", roomId, sessionId)
                    mView?.showLoading(false)
                }
    
                override fun onSuccess(data: MLRoomInfo) {
                    Log.d(TAG, "on joinMeeting success data: $data")
                    mView?.createAndJoinMeetingSuccess(data)
                    mView?.showLoading(false)
                }
    
                override fun onError(code: Int, msg: String) {
                    Log.d(TAG, "on joinMeeting failure $code")
                    if (code == 403111031) {
                        mView?.showOneButtonDialog("你已经被禁止加入该会议")
                    } else {
                        mView?.showToast(msg)
                    }
                    mView?.showLoading(false)
                }
    
                override fun alreadyInMeeting() {
                    mView?.showToast("你已经在房间中了")
                    mView?.showLoading(false)
                }
            }
        )
    }
```

### 参数说明
MLApi.joinMeeting

| 参数名称 | 参数类型 | 是否必填 | 参数描述 |
| --- | --- | --- | --- |
| meetingNo | String | 是 | 房间号 |
| isMuteVideo | Boolean | 是 | 是否关闭摄像头进入房间 |
| isMuteAudio | Boolean | 是 | 是否关闭麦克风进入房间 |
| pwd | String | 是 | 入会密码 |
| nickName | String | 是 | 入会用户名称 |
| avatar | String | 是 | 用户头像 |
| callback | MeetingCallback | 是 | 创建会议结果回调 |

#### MLRoomInfo
会议信息
| 参数名称 | 参数类型  | 参数描述 |
| --- | --- | --- |
| id | String | sessionId，房间的唯一标识 |
| roomNo | String | 房间号 |
| password | String | 房间密码 |
| isAudioMute | Boolean | 麦克风关闭状态，默认为true |
| isVideoMute | Boolean | 摄像头关闭状态，默认为true |
| isRejoin | Boolean | 成员是否重新加入房间,默认为false |



#### MeetingCallback
创建会议结果回调

```kotlin
interface MeetingCallback {

    /**
     * 有创建的会议未结束
     * @param roomId    房间号
     * @param sessionId 房间的唯一标识，用于房间外结束房间的id
     */
    fun onMeetingExist(roomId: String, sessionId: String)

    /**
     * 创建会议成功
     * @param data 会议房间信息
     */
    fun onSuccess(data: MLRoomInfo)

    /**
     * 创建会议失败
     * @param code：失败的错误码
     * @param msg：错误的提示语
     */
    fun onError(code: Int, msg: String)

    /**
     * 成员已经在会中
     */
    fun alreadyInMeeting()
}
```

# 进入房间

### 功能介绍
会中模块，迈聆 SDK 把布局和相关处理逻辑都封装在 MLFragment 了，在创建会议或者加入会议成功后需要跳转进入到会中，这个时候需要加载 MLFragment 到 MeetingActivity 中。

### 示例代码
在 中声明 MeetingActivity，需要在 intent-filter 中声明 Action

```kotlin
<activity android:name=".ui.home.meeting.MeetingActivity"
    android:theme="@style/BlackTheme"  android:configChanges="keyboard|keyboardHidden|orientation|screenSize|fontScale"
    android:windowSoftInputMode="adjustPan"
    android:launchMode="singleTask">
    <intent-filter>
        <action android:name="包名.action" />
        <category android:name="android.intent.category.DEFAULT" />
    </intent-filter>
</activity>
```

```kotlin
// 这个 MeetingActivity 需要在你们的 app 中创建
class MeetingActivity : Activity() {

    private val mActivityDelegate: IActivityDelegate = object : IActivityDelegate {

        override fun finishActivity() {
            finish()
        }

        override fun applyPublishPermission() {
            val packageName = this@MeetingActivity.packageName
            MLApi.getProjectionIntent(
                this@MeetingActivity,
                this@MeetingActivity.packageManager.getApplicationInfo(packageName, 0).uid,
                packageName
            )
        }
    }
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val fragment = MLFragment.newInstance()
        (fragment as MLFragment).setActivityDelegate(mActivityDelegate)
        supportFragmentManager.beginTransaction()
            .add(R.id.fragmentContainer, fragment, "MLFragment").commit()
    }

    // 由于房间提供共享的功能，需要在IActivityDelegate 方法中申请采集桌面信息的权限，并在onActivityResult方法中将请求的结果返回给SDK。
    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        MLApi.onActivityResult(this, requestCode, resultCode, data)
    }

    
    override fun getResources(): Resources {
        // 界面适配
        if (Looper.getMainLooper() == Looper.myLooper()) {
            MLApi.setAutoSize(super.getResources())
        } else {
            AndroidSchedulers.mainThread().scheduleDirect {
                MLApi.setAutoSize(super.getResources())
            }
        }
        return super.getResources()
    }

    override fun onBackPressed() {
        MLApi.onBackPressed()
    }
}
```

#### IActivityDelegate
MLFragment 中的回调接口
```kotlin
interface IActivityDelegate {
    /**
     * 会议结束：关闭当前 Activity
     */
    fun finishActivity()

    /**
     * 申请权限
     */
    fun applyPublishPermission()
}
```


# 离开会议

### 功能介绍
如果你需要在外部主动离开会议，可以调用 MLApi.dismissOtherMeeting(sessionId: String), 其中 sessionId 为房间的唯一标识符，创建房间或者加入房间成功后会返回该参数。

```kotlin
    MLApi.dismissOtherMeeting(sessionId: String, callback: DismissOtherMeetingCallback)
```

### 示例代码
```kotlin
    fun dismissMeeting(sessionId: String) {
        MLApi.dismissOtherMeeting(
            sessionId,
            callback = object : DismissOtherMeetingCallback {
                override fun onSuccess() {
                    mView?.showToast("会议结束成功")
                }

                override fun onError(code: Int, msg: String) {
                    mView?.showToast("会议结束失败")
                }
            }
        )
    }
```

### 参数说明
| 参数名称 | 参数类型 | 是否必填 | 参数描述 |
| --- | --- | --- | --- |
| sessionId | String | 是 | 房间的唯一标识符 |
| callback | DismissOtherMeetingCallback | 是 | 离开会议结果回调 |

#### DismissOtherMeetingCallback
```kotlin
interface DismissOtherMeetingCallback {
    /**
     * 离开会议成功
     */
    fun onSuccess()

    /**
     * 离开会议失败
     * @param code 错误码
     * @param msg 错误信息
     */
    fun onError(code: Int, msg: String)
}
```

# 离会通知

### 功能介绍
在离开会议的时候会接收到通知消息，回调的具体通知需要通过 MLAPi.setLeaveCallBack() 方法进行设置

```kotlin
    MLApi.setLeaveCallBack(callback: Callback1<LeaveType>)
```

### 示例代码
```kotlin
    private fun setLeaveCallBack() {
        MLApi.setLeaveCallBack {
            when (it) {
                LeaveType.TYPE_KICK_OUT -> {
                    showOneButtonDialog("你已被主持人移出会议")
                }
                LeaveType.TYPE_DISCONNECT -> {
                    showTwoButtonDialog("检测到您已掉线，是否立即重新加入会议?")
                }
                LeaveType.TYPE_LEAVE_NORMAL -> {
                    showTwoButtonDialog("离开会议")
                }
            }
        }
    }

```

### 参数说明
| 参数类型 | 参数描述 | 
| --- | --- |
| LeaveType.TYPE_KICK_OUT | 被主持人踢出房间 |
| LeaveType.TYPE_DISCONNECT | 已掉线 |
| LeaveType.TYPE_LEAVE_NORMAL | 离开会议 |

# 界面适配
### 功能介绍
为了进行更好的适配，请在 MeetingActivity 的 getResources 方法中调用 MLApi.setAutoSize(resource: Resources) 方法进行界面的适配。

### 示例代码

```kotlin
class MeetingActivity : Activity() {

    override fun getResources(): Resources {
        if (Looper.getMainLooper() == Looper.myLooper()) {
              MLApi.setAutoSize(super.getResources())
        } else {
              AndroidSchedulers.mainThread().scheduleDirect {
                     MLApi.setAutoSize(super.getResources())
              }
        }
        return super.getResources()
    }

    override fun onBackPressed() {
        MLApi.onBackPressed()
    }
}
```


# 设置主题色
### 功能介绍
1. 房间默认主题色为蓝色（#0B7BFF）
2. 房间背景色为黑色(#1A1A1A)
   如果要进行修改，可在项目中的 colors.xml 文件中进行以下色值的修改

### 示例代码
```kotlin
<color name="ml_color_main_color">#0B7BFF</color><!--主颜色 / 按钮 / 引导 / 高亮等主要突出使用-->
<color name="ml_color_main_bg">#1A1A1A</color><!--底色块颜色-->
```

# 错误码对照表

| 错误码 | 描述 |
| --- | --- | 
| 0 | 成功 | 
| -1 | 未知错误 | 
| 1004 | 会议已经解散 |
| 9992 | 本地录制失败 |
| 9993 | 无效的本地录制路径 |
| 9994 | 无效的方法调用
| 9995 | 加入会议过程中被取消 |
| 9997 | 用户已在房间中 | 
| 9998 | SDK 未初始化 | 
| 9999 | 无效参数 | 
| 11000 | 无效的设备 |
| 11001 | 无法创建渲染器 |
| 11002 | 无法启动照相机 |
| 11003 | 无效的 sdp |
| 11004 | 无效的 sdp answer |
| 11005 | 无法启动麦克风 |
| 11006 | 无法启动扬声器 |
| 11007 | 无法关闭照相机 |
| 12000 | 不能识别的滤镜 GUID |
| 12001 | 滤镜已经存在 |
| 13001 | 未授权 |
| 13002 | 未加入任务房间 |
| 99997 | 当前网络不可用 | 
| 4001002 | 该参数的值进行唯一性校验时，已存在 |
| 4011000 | Token 失效或者账号在其他设备上登录了 |
| 4003100 | 访问不存在的资源 |
| 4003101 | 端点不存在，端点 ID 错误 |
| 4003102 | 会议不存在 |
| 4003105 | 用户不存在 |
| 4003106 | 设备不存在 |
| 4003107 | 企业不存在 |
| 4031000 | 资源被禁止访问。 |
| 4031001 | 资源被禁止删除。 |
| 4031002 | 用户角色验证失败 |
| 4031003 | 用户权限验证失败 |
| 4031004 | API 权限验证失败 |
| 4031005 | 匿名入会时会议不存在 |
| 4003124 | 房间已存在 |
| 4041000 | URL 参数错误 |
| 4041001 | 找不到对应的 namespace |
| 4041002 | API未注册 |
| 4041003 | 对应的API版本不支持当前的 Method |
| 4041004 | 对应版本找不到指定的 host |
| 5000001 | 获取不到 ClientToken |
| 5000002 | 无法向认证服务器认证 Accesstoken |
| 5001002 | 服务器更新数据失败 |
| 5001005 | 没有可用的 DS |
| 5001006 | 节点未级联 |
| 5002002 | 账号系统异常 |
| 5002001 | SFU 服务器异常  |
| 400111001 | 请求参数校验不正确 | 
| 404111003 | 房间号错误或加入房间已结束 | 
| 403111031 | 成员禁止加入房间 | 
| 403111044 | 输入房间密码错误，如果房间是需要密码的，而成员没传密码加入房间也会报这个错误码 | 
| 403111044 | 输入房间密码次数 5 分钟内达到上限 |
| 403111051 | 无法被指定为主持人，当前用户可能是小程序入会 |
| 403111052 | 无法被指定为焦点视频，当前用户可能是小程序入会 |
| 403111066 | 正在应用自定义布局，暂不支持设置焦点视频
| 403111023 | 房间已锁定 |
| 403111046 | 密码输入错误次数过多 |
| 403111006 | 当前用户无权限 |
| 403111030 | 不能将角色转给硬件终端 |
| 400111001 | 某参数值校验不正确,在于值的类型错误 |
| 400111002 | 缺少 Accesstoken |
| 404111003 | 房间不存在 |
| 404111007 | 未加入任何房间，端点有效，但不存在请求的房间中 |
| 403119005 | 免费视频会议并发数量已到上限 |
| 403119009 | 购买的视频会议数量已到上限 |
| 403119010 | 在其他平台发起了会议 |
| 403119011 | 免费方数已到上限 |
| 403119012 | 支付方数已到上限 |
| 403119013 | 专属会议方数已到上限 |
| 403103017 | 企业开启的会议总数已达上限 |
| 403111018 | 单个会议的人数超过限制 |
| 403111023 | 会议已被锁定 |
| 403111031 | 您已被禁止入会 |
| 403111044 | 入会密码错误 |
| 403111045 | 入会凭证已失效 |
| 403111040 | 企业未开通录制功能 |
| 400111041 | 存储空间不足 |
| 500111004 | 当前资源被锁定 |
| 500111011 | 信令服务器不可用 |
| 500111012 | 没有可用的SFU |
| 500111013 | 远程服务异常 |



# 常见问题
#### **1. 调用 MLApi 的方法抛出 “Maxme has not authenticate” 异常？**
问题原因为未调用MLApi的authenticate()方法，请在sdk初始化完成用户登录后调用authenticate()方法。

#### **2. 界面显示与 demo 中的界面显示不符。**
为了更好的实现界面的适配，建议参考demo中的代码实现MeetingActivity实现状态栏沉浸效果。

#### **3. 发起共享失败或者对方看不到画面**
请确保Activity有生成ActivityDelegate对象，并实现applyPublishPermission()方法传给MLFragment，applyPublishPermission()方法中请务必要调用MLApi.getProjectionIntent()方法，并复写Activity的onActivityResult()方法，将结果通过MLApi的onActivityResult()方法。如果还不行，请检查清单文件中的MeetingActivity是否配置正确。

#### **4. 视频房间中发生报错等异常想象怎么排查？**
请在sdk初始化方法的option参数里设置logPath，设置enableLog=true，当异常发生后可在对应设置的目录中查看。如果都设置了还是没有，请检查是否有给予文件读写权限。

#### **5. 创建房间或者加入房间时传入的参数 isMuteVideo = false, isMuteAudio = false, 但是进入房间后摄像头和麦克风还是处于关闭状态？**
这里可能是有两种原因导致上述情况：
1. 没有给予摄像头和麦克风的权限。只有用户赋予了相关权限后，进入房间后才可以真正实现摄像头的打开和麦克风的打开。因此建议在要打开麦克风和摄像头权限时需申请对应权限。
2. 非用户主动离开，而是被动离开（如应用被强杀），此时该用户会有60秒钟的重连，只有超过60秒才会真正离开房间。因此在60秒内重新进入房间，用户的状态不会发生任何改变。

# Api 参考
## API 概览

### MLApi

| 方法 | 描述 |
| --- | --- |
| [init](#init) | 初始化 SDK |
| [authenticate](#authenticate) | 登录授权 |
| [createMeeting](#createmeeting) | 创建会议 |
| [joinMeeting](#joinmeeting) | 加入会议 |
| [dismissOtherMeeting](#dismissothermeeting) | 主动离开会议 |
| [setLeaveCallBack](#setleavecallback) | 设置离开会议监听 |
| [setAutoSize](#setautosize) | 设置屏幕适配 |
| [onBackPressed](#onbackpressed) | 响应 MeetingActivity 中的返回按键 |
| [onActivityResult](#onactivityresult) | 处理 MeetingActivity 中的 onActivityResult 方法|
| [getProjectionIntent](#getprojectionintent) | 桌面共享的权限数据意图相关 |

### 接口回调
| 接口 | 描述 |
| --- | --- |
| [MeetingCallback](#meetingcallback) | 创建会议/进入会议 接口回调 |
| [AuthCallback](#authcallback) | 登录授权 接口回调 |
| [DismissOtherMeetingCallback](#dismissothermeetingcallback) | 主动离开会议 接口回调 |


## 方法详情
### init
用于 SDK 初始化

#### 方法

```
fun init(context: Application, options: MLOptions)
```

#### 参数说明

| 参数名 | 参数类型 | 是否必填 | 参数描述 |
| --- | --- | --- | --- |
| context | Application | 是 | Application 实例对象 |
| options | MLOptions | 是 | 初始化配置参数可选项 |

- Options 参数说明

| 参数名 | 参数类型 | 是否必填 | 参数描述 |
| --- | --- | --- | --- |
| enableConsoleLog | String | 否 | 是否在控制台输出日志打印 |
| enableLog | Boolean | 否 | 是否开启日志 |
| logPath | String | 是 | 本地日志保存路径，只有 enableLog 为 true 的情况下，才会进行日志的写入 |


#### 示例代码

```
    private fun initMaxME(context: Application) {
        val options = MLOptions()
        options.logPath = "/sdcard/UI_SDK"
        options.enableLog = true
        options.enableConsoleLog = true
        MLApi.init(context, options)
    }
```

### authenticate
使用 jwt token 进行登录授权

#### 方法

```
fun authenticate(serverUrl: String, token: String, userName: String, avatar: String, callback: AuthCallback)
```

#### 参数说明

| 参数名称 | 参数类型 | 是否必填 | 参数描述 |
| --- | --- | --- | --- |
| serverUrl | String | 是 | 服务器地址，即在迈聆开放平台申请的房间服务器地址 |
| token | String | 是 | AuthCode，根据 jwt 协议生成 |
| userName | String | 是 | 用户名称 |
| avatar | String | 是 | 用户头像 |
| callback | [AuthCallback](#authcallback) | 是 | 登录授权结果回调 |


#### 示例代码

```
    override fun authenticate(token: String, nickName: String) {
        val url = PreferenceUtils.getString(Constanct.URL, null) ?: URL
        MLApi.authenticate(
            url,
            token, nickName, "",
            object : AuthCallback {
                /**
                 * 校验成功
                 */
                override fun onSuccess() {
                    Log.i(TAG, "MaxME authenticate success.")
                    mView?.authenticateSuccess()
                }

                /**
                 * 校验失败
                 * @param code 错误信息
                 * @param msg 错误码
                 */
                override fun onError(code: Int, msg: String) {
                    Log.w(TAG, "MaxME authenticate failed, error : $code msg: $msg")
                }
            }
        )
    }

```


### createMeeting
调用该方法快速创建一个新的会议

#### 方法

```
fun createMeeting(isMuteVideo: Boolean = true, isMuteAudio: Boolean = true, nickName: String = "", avatar: String = "", callback: MeetingCallback)
```

#### 参数说明

| 参数名 | 参数类型 | 是否必填 | 参数描述 |
| --- | --- | --- | --- |
| isMuteVideo | Boolean | 否 | 是否关闭摄像头 |
| isMuteAudio | Boolean | 否 | 是否关闭麦克风 |
| nickName | String | 否 | 入会名称 |
| avatar | String | 否 | 入会头像 |
| callback | [MeetingCallback](#meetingcallback) | 是 | 入会结果回调 |


#### 示例代码

```
    override fun createMeeting(edUserName: String, isMuteVideo: Boolean, isMuteAudio: Boolean) {
        MLApi.createMeeting(
            isMuteVideo = isMuteVideo, isMuteAudio = isMuteAudio, nickName = "", avatar = "",
            callback = object : MeetingCallback {
                override fun onMeetingExist(roomId: String, sessionId: String) {
                    mView?.showTwoButtonDialog("你之前有创建的房间未结束 \n房间号：$roomId", roomId, sessionId)
                    mView?.showLoading(false)
                }

                override fun onSuccess(data: MLRoomInfo) {
                    Log.d(TAG, "on meeting create success data: $data")
                    mView?.createAndJoinMeetingSuccess(data)
                    mView?.showLoading(false)
                }

                override fun onError(code: Int, msg: String) {
                    Log.d(TAG, "on meeting create failure $code")
                    mView?.showToast(msg)
                    mView?.showLoading(false)
                }

                override fun alreadyInMeeting() {
                    mView?.showToast("你已经在房间中了")
                    mView?.showLoading(false)
                }
            }
        )
    }

```


### joinMeeting
调用该方法进入会议

#### 方法
```kotlin
MLApi.joinMeeting(
        meetingNo: String,              // 房间号
        isMuteVideo: Boolean = true,    // 是否关闭摄像头进入房间
        isMuteAudio: Boolean = true,    // 是否关闭麦克风进入房间
        pwd: String = "",               // 入会密码
        nickName: String = "",          // 用户名称
        avatar: String = "",            // 用户头像
        callback: MeetingCallback)      // 结果回调  
```


#### 参数说明
| 参数名 | 参数类型 | 是否必填 | 参数描述 |
| --- | --- | --- | --- |
| meetingNo | String | 是 | 房间号 |
| isMuteVideo | Boolean | 否 | 是否关闭摄像头 |
| isMuteAudio | Boolean | 否 | 是否关闭麦克风 |
| pwd | String | 否 | 会议密码 |
| nickName | String | 否 | 入会名称 |
| avatar | String | 否 | 入会头像 |
| callback | [MeetingCallback](#meetingcallback) | 是 | 入会结果回调 |


#### 示例代码

```kotlin
fun joinMeeting(meetingNUmber: String, isMuteVideo: Boolean, isMuteAudio: Boolean, password: String) {
    mView?.showLoading(true)
    MLApi.joinMeeting(
            meetingNo = meetingNUmber,
            isMuteVideo = isMuteVideo,
            isMuteAudio = isMuteAudio,
            pwd = password,
            nickName = "",
            avatar = "",
            callback = object : MeetingCallback {
                override fun onMeetingExist(roomId: String, sessionId: String) {
                    mView?.showTwoButtonDialog("你之前有创建的房间未结束 \n房间号：$roomId", roomId, sessionId)
                    mView?.showLoading(false)
                }
    
                override fun onSuccess(data: MLRoomInfo) {
                    Log.d(TAG, "on joinMeeting success data: $data")
                    mView?.createAndJoinMeetingSuccess(data)
                    mView?.showLoading(false)
                }
    
                override fun onError(code: Int, msg: String) {
                    Log.d(TAG, "on joinMeeting failure $code")
                    if (code == 403111031) {
                        mView?.showOneButtonDialog("你已经被禁止加入该会议")
                    } else {
                        mView?.showToast(msg)
                    }
                    mView?.showLoading(false)
                }
    
                override fun alreadyInMeeting() {
                    mView?.showToast("你已经在房间中了")
                    mView?.showLoading(false)
                }
            }
        )
}
```


### dismissOtherMeeting
离开会议

#### 方法
```kotlin
fun dismissOtherMeeting(sessionId: String, callback: DismissOtherMeetingCallback)
```

#### 参数说明
| 参数名 | 参数类型 | 是否必填 | 参数描述 |
| --- | --- | --- | --- |
| sessionId | String | 是 | 房间的唯一标识符 |
| callback | [DismissOtherMeetingCallback](#dismissothermeetingcallback) | 是 | 离开会议结果回调 |

#### 示例代码
```kotlin
    fun dismissMeeting(sessionId: String) {
        MLApi.dismissOtherMeeting(
            sessionId,
            callback = object : DismissOtherMeetingCallback {
                override fun onSuccess() {
                    mView?.showToast("会议结束成功")
                }

                override fun onError(code: Int, msg: String) {
                    mView?.showToast("会议结束失败")
                }
            }
        )
    }
```


### setLeaveCallBack
设置会议结束接口回调

#### 方法
```kotlin
fun setLeaveCallBack(callback: Callback1<LeaveType>): Boolean
```

### 示例代码
```kotlin
    private fun setLeaveCallBack() {
        MLApi.setLeaveCallBack {
            when (it) {
                LeaveType.TYPE_KICK_OUT -> {
                    showOneButtonDialog("你已被主持人移出会议")
                }
                LeaveType.TYPE_DISCONNECT -> {
                    showTwoButtonDialog("检测到您已掉线，是否立即重新加入会议?")
                }
                LeaveType.TYPE_LEAVE_NORMAL -> {
                    showTwoButtonDialog("离开会议")
                }
            }
        }
    }

```
#### 返回值
Boolean 类型；true：已经入会了，调用成功；false：还没有入会，调用失败

### setAutoSize
设置屏幕适配

#### 方法
```kotlin
fun setAutoSize(resource: Resources)
```

#### 示例代码

```kotlin
class MeetingActivity : Activity() {

    override fun getResources(): Resources {
        if (Looper.getMainLooper() == Looper.myLooper()) {
              MLApi.setAutoSize(super.getResources())
        } else {
              AndroidSchedulers.mainThread().scheduleDirect {
                     MLApi.setAutoSize(super.getResources())
              }
        }
        return super.getResources()
    }

    override fun onBackPressed() {
        MLApi.onBackPressed()
    }
}
```

### onBackPressed
响应 MeetingActivity 中的返回按键

#### 方法
```kotlin
fun onBackPressed()
```

#### 示例代码
```kotlin
class MeetingActivity : Activity() {
    override fun onBackPressed() {
        MLApi.onBackPressed()
    }
}
```

### onActivityResult
处理 MeetingActivity 中的 onActivityResult 方法

#### 方法
```kotlin
    fun onActivityResult(context: Context, requestCode: Int, resultCode: Int, data: Intent?)
```


### getProjectionIntent
桌面共享的权限数据意图相关

#### 方法
```kotlin
    fun getProjectionIntent(context: Context, uid: Int, packageName: String) 
```
#### 参数说明
| 参数名 | 参数类型 | 是否必填 | 参数描述 |
| --- | --- | --- | --- |
| context | Context | 是 | 上下文信息 context |
| uid | Int | 是 | App 的唯一 id |
| packageName | String | 是 | 包名 |


#### 示例代码
```kotlin
    private val mActivityDelegate: IActivityDelegate = object : IActivityDelegate {
        override fun finishActivity() {
            finish()
        }

        override fun applyPublishPermission() {
            val packageName = this@MeetingActivity.packageName
            MLApi.getProjectionIntent(
                this@MeetingActivity,
                this@MeetingActivity.packageManager.getApplicationInfo(packageName, 0).uid,
                packageName
            )
        }
    }
```

### MeetingCallback

```
interface MeetingCallback {

    /**
     * 会议室已经存在
     * @param roomId 会议室 id
     */
    fun onMeetingExist(roomId: String, sessionId: String)

    /**
     * 入会成功
     * @param data 会议室信息
     */
    fun onSuccess(data: MLRoomInfo)

    /**
     * 入会失败
     * @param code
     * @param msg
     */
    fun onError(code: Int, msg: String)

    /**
     * 会议已经存在
     */
    fun alreadyInMeeting()
}
```


### AuthCallback

```kotlin
interface AuthCallback {

    /**
     * 授权成功
     */
    fun onSuccess()

    /**
     * 登录授权失败
     * @param code 错误码
     * @param msg 错误信息
     */
    fun onError(code: Int, msg: String)
}
```


### DismissOtherMeetingCallback
```kotlin
interface DismissOtherMeetingCallback {
    /**
     * 离开会议成功
     */
    fun onSuccess()

    /**
     * 离开会议失败
     * @param code 错误码
     * @param msg 错误信息
     */
    fun onError(code: Int, msg: String)
}
```


