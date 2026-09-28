# 森林乐园宝箱自动领取修复模块

## 问题说明

支付宝森林乐园宝箱功能已从旧的 `gamecenteruprod` 接口迁移到新的 `charitygamecenter` 接口，需要执行以下步骤：

1. 查询宝箱列表 (`alipay.charity.mobile.game.center.h5.query`)
2. 进入小游戏 (`alipay.charity.mobile.game.center.h5.enter`)  
3. **上报游戏进度** (`alipay.charity.mobile.game.center.h5.report`) - 必须步骤
4. 开启宝箱 (`alipay.charity.mobile.game.center.h5.open`)

Seedum FluxV11 版本的脚本还在使用老接口，缺少上述逻辑，导致无法自动领取宝箱。

## 解决方案

本模块通过 Xposed Hook 拦截支付宝的 RPC 调用，在检测到 `gamecenteruprod` 请求后，自动补充调用 `charitygamecenter` 接口完成宝箱领取。

## 编译方法

### 方法一：使用 Android Studio（推荐）

1. 将整个 `ForestGameCenterFix` 目录复制到电脑
2. 用 Android Studio 打开项目
3. 等待 Gradle 同步完成
4. 点击 `Build > Build Bundle(s) / APK(s) > Build APK(s)`
5. 编译完成后在 `app/build/outputs/apk/release/` 找到 APK

### 方法二：使用命令行

在项目根目录执行：

```bash
# Windows
gradlew.bat assembleRelease

# macOS/Linux
./gradlew assembleRelease
```

### 方法三：使用在线编译服务

1. 创建 GitHub 仓库，上传整个项目
2. 在仓库设置中启用 GitHub Actions
3. 创建 `.github/workflows/build.yml` 自动编译
4. 推送代码后自动编译并发布 APK

## 安装使用

1. 安装编译好的 APK
2. 在 Zafiro/LSPosed 中启用模块
3. 勾选作用域：**支付宝 (com.eg.android.AlipayGphone)**
4. 重启支付宝
5. 运行 Seedum 森林乐园功能，模块会自动补充宝箱领取逻辑

## 日志查看

使用 logcat 查看运行日志：

```bash
adb logcat | grep ForestGameCenterFix
```

或在 Zafiro/LSPosed 的日志中查看。

## 技术细节

- **Hook 目标**：`com.alipay.mobile.framework.service.common.RpcService.rpcCall`
- **备用方案**：`com.alipay.mobile.common.transport.http.HttpManager.request`
- **触发条件**：检测到包含 `gamecenteruprod` 的 RPC 请求
- **执行逻辑**：异步调用 4 个 charitygamecenter 接口完成宝箱领取

## 兼容性

- **支付宝版本**：12.12.30 及类似版本
- **Xposed 框架**：LSPosed、Zafiro (Vector)、EdXposed 等
- **Android 版本**：8.0+ (API 26+)

## 注意事项

1. 本模块与 Seedum 独立运行，不会冲突
2. 确保 Seedum 的森林乐园功能已启用
3. 模块会在 Seedum 执行签到/领奖后自动处理宝箱
4. 首次使用建议查看日志确认运行正常

## 开源协议

MIT License