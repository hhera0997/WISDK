# SDK 使用指南

## 配置步骤

### 1️⃣  `minSdkVersion` 为 23 ，屏幕方向为竖屏

在 `gradle.properties` 中将 `PROP_MIN_SDK_VERSION` 值设置为 **23** 或更高版本。

### 2️⃣ 添加 SDK.aar 文件

将提供的 **`SDK.aar`** 文件复制到 **`libs`**  目录

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

### 5️⃣ 在 `Activity` 中导入 SDK

在 `Activity` 中添加导入语句：

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

// 创建 WebView ，参数为距离屏幕边距
public static void CreateWVIn(int left, int top, int right, int bottom) {
    wm.createWVIn(left, top, right, bottom);
}

// 销毁 WebView
public static void DestroyWVIn() {
    wm.destroyWVIn();
}

```

✅ **key** 为字符串类型地址，由我方提供，请联系运营获取。

## 使用说明与调用步骤

### ✅ 创建 WebView 前的配置判断

在调用 `CreateWVIn()` 创建 WebView 之前，务必先调用：

```
IsConfigEnabled();
```

以确保配置加载完成。

### 💡 调用逻辑推荐

- ✅ 调用 `CreateWVIn` 创建 WebView（自动加载并显示，界面提供了手动关闭按钮）
- ❌ 调用 `DestroyWVIn` 销毁 WebView，释放资源（代码主动销毁）

📌 **注意：**

- ✅ WebView 会默认显示在游戏的最上层，请根据需要使用传入参数设定显示区域。
- 👁 如果使用前端展示功能，在商店审核阶段不显示调用按钮，自主控制按钮显示。
- 🔁 屏幕方向为竖屏
