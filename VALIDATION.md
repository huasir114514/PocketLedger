# 验证记录

实际执行日期：2026-10-01。源码 / APK 版本：0.1.1。

| 检查 | 结果 |
|---|---|
| 核心金额、消息识别、合并规则 | 67 项断言通过，执行实际 Java 源码 |
| Java 语法 | 16 个源码文件解析通过 |
| XML | 5 个文件解析通过 |
| 宿主 SQLite 数据库检查 | 实际 LedgerDb.java，19 个场景、66 项断言通过 |
| 安卓 SDK 编译和 APK 生成 | GitHub Actions 构建成功，AGP 8.9.1 / Gradle 8.11.1 / JDK 17 / compileSdk 35 |
| 安卓单元测试、Lint | testDebugUnitTest / lintDebug 通过 |
| APK 权限检查 | aapt 检查通过，安装包没有 INTERNET 权限 |
| 安卓数据库仪器测试 | Android 35 模拟器：19 个测试通过，connectedDebugAndroidTest 构建成功 |
| 真实付款页 OCR、系统通知、截图动画、后台行为 | 仍需在用户手机执行 DEVICE-CHECKLIST.md |

构建记录：https://github.com/huasir114514/PocketLedger/actions/runs/36851535944

APK 文件：PocketLedger-0.1.1.apk，52,548,808 字节。
SHA-256：967f5f9e3a7fecc0e01b3d5acfa1d9d98f170cb2b76dc29c288f747cb3245006。

首次构建遇到 SDK 安装脚本请求已下架 tools 包，已改为 platform-tools；第二次构建与模拟器测试全部成功。

0.1.1 修复：待处理补记保留原来源和事件键；通知已忽略或删除后不再重放入账；旧数据库升级保留账目和余额；实付金额优先于折扣前交易金额；日期和时间使用原生选择器。

APK 为个人测试用调试签名；真实支付模板及后台行为尚未经过实机验收。
