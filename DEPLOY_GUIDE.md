# 广电吉视抽奖系统 - 部署指南

## 目录
1. [环境要求](#环境要求)
2. [项目导入](#项目导入)
3. [构建APK](#构建apk)
4. [安装部署](#安装部署)
5. [初始化配置](#初始化配置)
6. [常见问题](#常见问题)

---

## 环境要求

### 开发环境

| 组件 | 版本要求 |
|------|----------|
| Android Studio | 2023.1.1 (Giraffe) 或更高 |
| JDK | 17 或更高 |
| Gradle | 8.1+ |
| Android SDK | API 21-34 |

### 系统要求

- **操作系统**：Windows 10/11, macOS 10.15+, 或 Linux
- **内存**：至少 8GB RAM（推荐 16GB）
- **磁盘空间**：至少 10GB 可用空间

---

## 项目导入

### 方法一：直接打开项目

1. 打开 Android Studio
2. 选择 **File → Open**
3. 选择项目根目录 `GuangDianLottery/`
4. 等待 Gradle 同步完成

### 方法二：导入项目

1. 打开 Android Studio
2. 选择 **File → New → Import Project**
3. 选择项目根目录
4. 选择 **Create project from existing sources**
5. 点击 **Next** 完成导入

### 同步项目

导入后，Android Studio 会自动开始 Gradle 同步：

1. 等待底部状态栏显示 **Sync finished**
2. 如出现错误，点击 **Sync Now** 重新同步
3. 确保所有依赖下载完成

---

## 构建APK

### 调试版本（Debug）

#### 通过 Android Studio

1. 点击菜单栏 **Build → Build Bundle(s) / APK(s) → Build APK(s)**
2. 等待构建完成
3. 点击右下角通知栏的 **locate** 查看 APK 位置
4. APK 路径：`app/build/outputs/apk/debug/app-debug.apk`

#### 通过命令行

```bash
# 进入项目目录
cd GuangDianLottery

# 执行构建
./gradlew assembleDebug

# Windows 使用
gradlew.bat assembleDebug
```

### 发布版本（Release）

#### 配置签名（首次需要）

1. 点击菜单栏 **Build → Generate Signed Bundle / APK...**
2. 选择 **APK**
3. 点击 **Create new...** 创建密钥库
4. 填写密钥库信息：
   - **Key store path**：选择保存位置
   - **Password**：设置密钥库密码
   - **Key alias**：设置密钥别名
   - **Key password**：设置密钥密码
   - **Validity**：建议 25 年
   - **Certificate**：填写组织信息
5. 点击 **OK** 保存

#### 构建发布版APK

1. 点击菜单栏 **Build → Generate Signed Bundle / APK...**
2. 选择已创建的密钥库
3. 输入密码
4. 选择 **release** 构建类型
5. 点击 **Finish**
6. APK 路径：`app/build/outputs/apk/release/app-release.apk`

#### 通过命令行构建

```bash
# 配置签名后执行
./gradlew assembleRelease

# Windows 使用
gradlew.bat assembleRelease
```

---

## 安装部署

### 安装到设备

#### 方法一：通过ADB

```bash
# 连接设备后执行
adb install app/build/outputs/apk/release/app-release.apk

# 如果已安装，使用 -r 参数覆盖
adb install -r app/build/outputs/apk/release/app-release.apk
```

#### 方法二：直接传输

1. 将 APK 文件复制到安卓设备
2. 在设备上找到 APK 文件
3. 点击安装
4. 允许安装未知来源应用（如提示）

#### 方法三：通过 Android Studio

1. 连接设备或启动模拟器
2. 点击工具栏 **Run** 按钮（绿色三角形）
3. 选择目标设备
4. 自动安装并启动

### 授予权限

首次启动时需要授予以下权限：

- **存储权限**：用于导出数据到本地

授予步骤：
1. 安装后首次打开应用
2. 弹出权限请求时点击 **允许**
3. 或在设置中手动开启：
   - 设置 → 应用 → 广电吉视抽奖 → 权限 → 存储 → 允许

---

## 初始化配置

### 首次启动

1. 打开应用
2. 看到主界面，显示中国广电和吉视传媒LOGO
3. 点击 **开始抽奖** 可进入员工界面

### 管理员配置

#### 进入管理员后台

1. 在主界面
2. **连续快速点击顶部LOGO区域 5次**
3. 看到提示"再点击 X 次进入管理员后台"
4. 输入默认密码：`admin123`
5. 进入管理员后台

#### 修改管理员密码

1. 进入管理员后台
2. 选择 **活动设置** 标签
3. 点击 **修改管理员密码**
4. 输入原密码：`admin123`
5. 输入新密码（至少6位）
6. 确认新密码
7. 点击 **修改**

> ⚠️ **重要**：首次使用后务必修改默认密码！

#### 配置奖品

1. 进入管理员后台
2. 选择 **奖品管理** 标签
3. 点击右下角 **+** 按钮
4. 添加奖品：
   - 输入奖品名称
   - 设置中奖概率
   - 点击 **添加**
5. 重复添加所有奖品
6. 确保概率总和等于100%

**示例奖品配置**：

| 奖品名称 | 概率 |
|----------|------|
| 一等奖：iPhone 15 | 1% |
| 二等奖：平板电脑 | 5% |
| 三等奖：蓝牙耳机 | 10% |
| 四等奖：充电宝 | 20% |
| 五等奖：数据线 | 30% |
| 谢谢参与 | 34% |

#### 开启活动

1. 进入管理员后台
2. 选择 **活动设置** 标签
3. 开启 **抽奖开关**
4. 设置 **开始日期** 和 **结束日期**
5. 设置 **每日最大抽奖次数**（建议 100-1000）
6. 点击 **保存**

#### 测试抽奖

1. 返回主界面
2. 点击 **开始抽奖**
3. 输入测试客户信息
4. 点击转盘测试
5. 确认功能正常

---

## 常见问题

### Q1: Gradle 同步失败

**错误现象**：
- Sync 失败提示
- 依赖下载失败

**解决方案**：

1. 检查网络连接
2. 配置国内镜像（如需要）：

在 `build.gradle`（项目级）添加：
```gradle
allprojects {
    repositories {
        maven { url 'https://maven.aliyun.com/repository/public' }
        maven { url 'https://maven.aliyun.com/repository/google' }
        google()
        mavenCentral()
    }
}
```

3. 清除缓存后重试：
   - **File → Invalidate Caches / Restart...**

### Q2: 构建失败

**错误现象**：
- Build 失败
- 找不到类或资源

**解决方案**：

1. 执行 Clean：
   - **Build → Clean Project**
   
2. 重新构建：
   - **Build → Rebuild Project**

3. 检查 SDK 版本：
   - **File → Project Structure → SDK Location**
   - 确保 SDK 路径正确

### Q3: 安装失败

**错误现象**：
- 解析包错误
- 安装失败

**解决方案**：

1. 检查 APK 是否完整
2. 开启未知来源安装：
   - 设置 → 安全 → 未知来源 → 允许
3. 检查设备存储空间
4. 卸载旧版本后重新安装

### Q4: 应用闪退

**错误现象**：
- 打开后闪退
- 操作后崩溃

**解决方案**：

1. 检查设备系统版本（需 Android 5.0+）
2. 清除应用数据后重试
3. 查看日志定位问题：
   ```bash
   adb logcat | grep GuangDian
   ```

### Q5: 数据库初始化失败

**错误现象**：
- 首次启动报错
- 无法保存数据

**解决方案**：

1. 确保存储权限已授予
2. 检查设备存储空间
3. 清除应用数据后重新启动

---

## 项目结构说明

```
GuangDianLottery/
├── app/                          # 应用模块
│   ├── src/main/
│   │   ├── java/com/guangdian/lottery/
│   │   │   ├── activity/         # Activity页面
│   │   │   ├── adapter/          # RecyclerView适配器
│   │   │   ├── database/         # 数据库操作
│   │   │   ├── fragment/         # Fragment页面
│   │   │   ├── model/            # 数据模型
│   │   │   ├── util/             # 工具类
│   │   │   └── view/             # 自定义View
│   │   ├── res/                  # 资源文件
│   │   │   ├── drawable/         # 图标和背景
│   │   │   ├── layout/           # 布局文件
│   │   │   ├── menu/             # 菜单
│   │   │   ├── values/           # 资源值
│   │   │   └── xml/              # 配置文件
│   │   └── AndroidManifest.xml   # 清单文件
│   ├── build.gradle              # 模块构建配置
│   └── proguard-rules.pro        # 混淆规则
├── build.gradle                  # 项目构建配置
├── settings.gradle               # 项目设置
├── gradle.properties             # Gradle属性
├── README.md                     # 项目说明
├── USER_MANUAL.md                # 使用手册
└── DEPLOY_GUIDE.md               # 部署指南
```

---

## 版本历史

| 版本 | 日期 | 说明 |
|------|------|------|
| v1.0.0 | 2024年 | 初始版本发布 |

---

## 技术支持

**中国广电吉视传媒**

---

**文档版本**：v1.0
**更新日期**：2024年
