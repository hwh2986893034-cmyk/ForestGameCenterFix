# 森林乐园宝箱修复 - 快速开始

## 🚀 最快速的使用方法（推荐）

### 步骤 1：上传到 GitHub

1. 在 GitHub 创建新仓库（public 或 private 都可以）
2. 将 `/sdcard/Download/ForestGameCenterFix` 目录上传到仓库

**手机直接上传方法：**
```bash
# 如果你有安装 git（Termux）
cd /sdcard/Download/ForestGameCenterFix
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/你的用户名/ForestGameCenterFix.git
git push -u origin main
```

**或者使用电脑：**
- 通过 USB 将文件夹复制到电脑
- 或使用文件管理器压缩后通过微信/QQ发送到电脑
- 解压后上传到 GitHub

### 步骤 2：自动编译

1. 代码上传后，GitHub Actions 会自动开始编译
2. 点击仓库的 **Actions** 标签查看编译进度
3. 编译完成后，在 **Releases** 页面下载编译好的 APK
4. 或者在 Actions 的构建任务中下载 **Artifacts**

### 步骤 3：安装使用

1. 安装下载的 APK
2. 打开 **Zafiro** 应用
3. 找到 **森林乐园修复** 模块并启用
4. 勾选作用域：**支付宝**
5. 重启支付宝（或软重启）
6. 运行 Seedum 的森林乐园功能，模块会自动补充宝箱领取

---

## 💻 其他编译方法

### 方法 A：使用 Android Studio（有电脑）

1. 在电脑上安装 Android Studio
2. 打开项目 `/sdcard/Download/ForestGameCenterFix`
3. 等待 Gradle 同步
4. 点击 Build > Build APK
5. 在 `app/build/outputs/apk/release/` 找到 APK

### 方法 B：在线编译服务

如果不想用 GitHub，可以尝试：
- **AppVeyor**
- **CircleCI**  
- **Travis CI**

这些服务都支持 Android 项目自动编译。

---

## 🔍 验证模块是否生效

### 查看日志：

```bash
# 方法 1：通过 adb
adb logcat | grep ForestGameCenterFix

# 方法 2：在 Zafiro 中查看模块日志
```

### 预期日志输出：

```
ForestGameCenterFix: 森林乐园修复模块已加载
ForestGameCenterFix: RPC Hook 安装成功
ForestGameCenterFix: 检测到 gamecenteruprod 请求
ForestGameCenterFix: 开始处理 charitygamecenter 宝箱逻辑
ForestGameCenterFix: 查询宝箱列表: {...}
ForestGameCenterFix: 处理宝箱: box_12345
ForestGameCenterFix: 进入小游戏: {...}
ForestGameCenterFix: 上报游戏进度: {...}
ForestGameCenterFix: ✅ 宝箱 box_12345 开启成功，获得能量: 88g
```

---

## ❓ 常见问题

**Q: 模块安装后没反应？**
- 确保在 Zafiro 中启用了模块
- 确保作用域勾选了支付宝
- 重启支付宝（必须）
- 查看日志确认 Hook 是否成功

**Q: 编译失败？**
- 检查网络连接（Gradle 需要下载依赖）
- 如果是 GitHub Actions，查看详细日志
- Java 版本需要 17+

**Q: 宝箱还是领不到？**
- 查看日志，可能 RPC 调用失败
- 支付宝版本可能不兼容（开发基于 12.12.30）
- 尝试手动开一次宝箱，看接口是否有变化

**Q: 与 Seedum 冲突吗？**
- 不会冲突，模块只是补充调用，不修改 Seedum

---

## 📦 项目文件说明

```
ForestGameCenterFix/
├── app/
│   ├── src/main/
│   │   ├── java/com/forest/gamecenter/fix/
│   │   │   └── MainHook.java          # 核心 Hook 代码
│   │   ├── assets/
│   │   │   └── xposed_init            # Xposed 入口配置
│   │   ├── res/values/
│   │   │   └── arrays.xml             # 作用域配置
│   │   └── AndroidManifest.xml        # 应用清单
│   └── build.gradle                   # 应用构建配置
├── build.gradle                       # 项目构建配置
├── settings.gradle                    # 项目设置
├── .github/workflows/build.yml        # GitHub Actions 自动编译
└── README.md                          # 详细说明文档
```

---

## 📞 需要帮助？

如果遇到问题：
1. 查看日志定位问题
2. 确认支付宝版本和 Xposed 框架版本
3. 尝试在不同时间运行（避免支付宝服务器限流）

---

**祝你使用愉快！🎉**