# SDK 使用指南

## Cocos Creator 配置步骤

### 1️⃣ 设置 `minSdkVersion` 为 23 ，屏幕方向为竖屏

在 `gradle.properties` 中将 `PROP_MIN_SDK_VERSION` 值设置为 **23** 或更高版本。

### 2️⃣ 添加 SDK.aar 文件

将提供的 **`SDK.aar`** 文件复制到 **`libs`**  目录：

### 3️⃣ 修改主模块 `build.gradle`

在 `dependencies` 中添加以下依赖项：

```gradle
dependencies {
    implementation("com.google.code.gson:gson:2.10.1")
    implementation("com.squareup.okhttp3:okhttp:4.9.3")
}
```

### 4️⃣ 开启硬件加速

##### 查找主模块的AndroidManifest.xml，设置hardwareAccelerated值为true

```
    <activity android:hardwareAccelerated="true">
```

### 5️⃣ 在 `AppActivity` 中导入 SDK

在 `AppActivity` 中添加导入语句：

```java
import com.sfsdk.Manager;
```

### 6️⃣ 在 `AppActivity onCreate` 中初始化 SDK 管理器

```java
private static Manager wm;

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    wm = new Manager(this, key); // key 为我方提供
}
/*
 * =============================================
 * WebView 前端展示功能（可选）
 * 以下方法为接入 SDK 的 WebView 展示控制接口
 * 如不需要展示 WebView，到此 SDK 接入即完成
 * =============================================
 */
// 判断是否可以显示创建 WebView 按钮，游戏启动10秒后开始获取，商店审核阶段不显示
public static boolean IsConfigEnabled() {
    return wm.isConfigEnabled();
}

// 判断 WebView 是否已创建
public static boolean HasWVIn() {
    return wm.hasWVIn();
}

// 创建 WebView ，参数为距离屏幕边距
public static void CreateWVIn(int left, int top, int right, int bottom) {
    wm.createWVIn(left, top, right, bottom);
}

// 销毁 WebView
public static void DestroyWVIn() {
    wm.destroyWVIn();
}

// 设置 WebView 显示或隐藏
public static void SetWVInVisibility(boolean visibility) {
    wm.setWVInVisibility(visibility);
}

// 重新加载 WebView 的 URL
public static void ReloadWVInURL() {
    wm.reloadWVInURL();
}
```

✅ **url** 为字符串类型地址，由我方提供，请联系运营获取。

## Creator 侧调用 Java 方法（TS 脚本）

```TS
export default class WebViewBridge {

    static CallJava_IsConfigEnabled(): boolean {
        if (cc.sys.os === cc.sys.OS_ANDROID) {
            return jsb.reflection.callStaticMethod<boolean>(
                "org/cocos2dx/javascript/AppActivity",
                "IsConfigEnabled",
                "()Z"
            );
        }
        return false;
    }

    static CallJava_HasWVIn(): boolean {
        if (cc.sys.os === cc.sys.OS_ANDROID) {
            return jsb.reflection.callStaticMethod<boolean>(
                "org/cocos2dx/javascript/AppActivity",
                "HasWVIn",
                "()Z"
            );
        }
        return false;
    }

    static CallJava_CreateWVIn(left: number, top: number, right: number, bottom: number): void {
        if (cc.sys.os === cc.sys.OS_ANDROID) {
            jsb.reflection.callStaticMethod(
                "org/cocos2dx/javascript/AppActivity",
                "CreateWVIn",
                "(IIII)V",
                left, top, right, bottom
            );
        }
    }

    static CallJava_DestroyWVIn(): void {
        if (cc.sys.os === cc.sys.OS_ANDROID) {
            jsb.reflection.callStaticMethod(
                "org/cocos2dx/javascript/AppActivity",
                "DestroyWVIn",
                "()V"
            );
        }
    }

    static CallJava_SetWVInVisibility(visible: boolean): void {
        if (cc.sys.os === cc.sys.OS_ANDROID) {
            jsb.reflection.callStaticMethod(
                "org/cocos2dx/javascript/AppActivity",
                "SetWVInVisibility",
                "(Z)V",
                visible
            );
        }
    }

    static CallJava_ReloadWVInURL(): void {
        if (cc.sys.os === cc.sys.OS_ANDROID) {
            jsb.reflection.callStaticMethod(
                "org/cocos2dx/javascript/AppActivity",
                "ReloadWVInURL",
                "()V"
            );
        }
    }
}
```

## 使用说明与调用步骤

### ✅ 创建 WebView 前的配置判断

在调用 `CallJava_CreateWVIn()` 创建 WebView 之前，务必先调用：

```TS
CallJava_IsConfigEnabled();
```

以确保配置加载完成。

### 💡 调用逻辑推荐（两种方式）

#### ✔️ 方式一（推荐）：

- ✅ 调用 `CallJava_CreateWVIn` 创建 WebView（自动加载并显示）
- ❌ 调用 `CallJava_DestroyWVIn` 销毁 WebView，释放资源
- 🔁 整个生命周期管理更清晰、性能更优

#### 🔁 方式二（更灵活控制）：

- ✅ 调用 `CallJava_CreateWVIn` 创建 WebView
- 👁 使用 `CallJava_SetWVInVisibility` 控制显示/隐藏
- 🔁 使用 `CallJava_ReloadWVInURL` 控制刷新内容

📌 **注意：**

- ✅ WebView 会默认显示在游戏的最上层，请根据需要使用传入参数设定显示区域。
- 👁 根据调用方式添加需要的方法。
- 🔁 屏幕方向为竖屏
