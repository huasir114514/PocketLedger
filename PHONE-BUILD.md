# 只有手机时使用

版本 0.1.1 已完成云端构建。仓库：huasir114514/PocketLedger。

1. 下载助手给出的 PocketLedger-0.1.1.apk，在 Android 12+ 手机上点击安装。
2. 打开 App，在设置中允许通知，开启无障碍截图服务和通知使用权。
3. 分别填写微信和支付宝当前余额。
4. 停留在付款成功页，拉下通知栏点“截图记账”；自动记账依赖支付成功系统通知。
5. 完成 DEVICE-CHECKLIST.md 的实机检查。

如从 GitHub 下载：进入 Actions → Build Android APK → 成功的构建 → Artifacts → PocketLedger-debug-apk，下载并解压得到 app-debug.apk。

完整工程位于 PocketLedger-source.zip。源码 ZIP 不能安装；工作流会解压它并生成 APK。
