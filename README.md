# 轻记账 · PocketLedger

Android 12+ 本机记账：微信零钱 / 支付宝余额、通知栏截图 OCR、支付通知自动记账、重复核对与每日 / 每月统计。

**0.1.1 APK 已生成。** 安卓编译、单元测试、Lint、安装包联网权限检查和 Android 35 模拟器上的 19 个数据库测试已通过。真实微信 / 支付宝截图与通知仍需在你的手机上验证。

完整工程在 [PocketLedger-source.zip](PocketLedger-source.zip)，下载后解压；云端工作流会自动解压并构建。使用方法见 [中文说明](README-zh.md)，测试结果见 [验证记录](VALIDATION.md)。

下载 APK：**Actions → Build Android APK → 成功的构建 → Artifacts → PocketLedger-debug-apk**，解压得到 `app-debug.apk`。源码 ZIP 不能直接安装。

首次安装后，在 App 设置中开启通知、无障碍截图服务和通知使用权，再录入微信 / 支付宝余额。
