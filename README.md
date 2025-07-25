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

```

✅ **key** 为字符串类型地址，由我方提供，请联系运营获取。