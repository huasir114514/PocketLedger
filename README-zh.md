# 轻记账 · 安卓项目

按需求实现的原生安卓记账应用，中文界面，支持 Android 12 及以上。版本 0.1.1。

**交付状态：0.1.1 已通过 GitHub Actions 安卓构建、单元测试、Lint、APK 联网权限检查，以及 Android 35 模拟器上的 19 个数据库仪器测试。APK 已生成。微信 / 支付宝真实付款页面 OCR、系统通知格式、截图动画和手机后台行为仍需在你的手机上验收。**

## 已实现的功能

- 录入、校准微信零钱和支付宝余额。
- 通知栏常驻“截图记账”按钮。点击后收起通知栏，截取当前付款页面，显示约 240 毫秒的闪光动画，在本机识别中文和金额；全过程不打开主界面。
- 开启通知使用权后，监听新到的微信、支付宝支付成功通知并自动记账。
- 截图与通知中同应用、同金额的记录，默认在 10 分钟内匹配，先只记一笔，通知提示“我有个问题”。核对页可以确认合并，也可以恢复为两次消费。
- 时间范围可以改为 1 / 5 / 10 / 30 / 60 分钟。明确不同的订单号不合并；明确相同订单号不受时间范围限制。
- 通知回调去重信息保存在数据库；同一截图在 60 秒内重复点击不会重复记账。两次不同通知不会只因金额相同就被合并。
- 按月切换：每日消费柱状图、每天的金额、每月消费合计、账单明细。
- 手动补记、修改金额和商家、调整付款来源、删除账单。
- 识别不清时进入待处理，填写确认后才入账。
- 待处理补记保留截图 / 通知来源，继续参与重复匹配；删除或忽略记录后，同一通知重放不会再次入账。
- CSV 导出，可用 Excel 打开。

只有手机时，请先看 [手机云端构建说明](PHONE-BUILD.md)。本项目已上传到 `huasir114514/PocketLedger`，并完成云端构建。

## 生成 APK：Windows

1. 安装 [Android Studio](https://developer.android.com/studio)。在 SDK Manager 安装 **Android SDK Platform 35** 和 **Android SDK Build-Tools 35.0.0**。
2. 准备 **JDK 17 或 21**。如果 Android Studio 的内置运行时是 21，脚本会优先使用它；不能直接使用 Java 25 运行本项目的 Gradle。
3. 解压到普通文件夹，例如 `C:\PocketLedger`。双击 `build-apk.cmd`。首次构建需要联网下载 Gradle、Android Gradle Plugin 和 OCR 依赖。
4. 构建成功后，APK 在 `app\build\outputs\apk\debug\app-debug.apk`。将它发到手机，点击安装。

如果需要指定 SDK / Java 路径，在项目文件夹的 PowerShell 中执行：

```powershell
powershell -ExecutionPolicy Bypass -File .\build-apk.ps1 -AndroidSdk "C:\Users\你的用户名\AppData\Local\Android\Sdk" -JavaHomePath "C:\Program Files\Java\jdk-17"
```

脚本会从 Gradle 官方服务器下载 8.11.1 并验证 SHA-256，然后生成**标准 Gradle Wrapper**，运行单元测试、Lint 和 APK 构建。脚本从不自动安装软件、改变 SDK 配置或修改电脑上的全局 Java 设置；它的环境变量仅作用于本次脚本进程。

本项目没有预置 `gradle-wrapper.jar`，原因是交付环境不能下载官方构建依赖。先执行以上脚本生成 Wrapper；之后可直接用 Android Studio 打开项目，或执行 `gradlew.bat assembleDebug`。

若下载失败，请先确认电脑能访问 `services.gradle.org`、Google Maven 和 Maven Central，不要把网页文件重命名成 APK。

## 生成 APK：GitHub Actions

不在电脑上装 SDK 的另一种方式：

1. 本仓库的完整工程保存在 `PocketLedger-source.zip`，工作流会自动解压后构建；下载源码时请解压该文件。也可以把解压后的完整文件放到其他仓库根目录，保留 `.github/workflows/build-apk.yml`。
2. 打开仓库的 **Actions → Build Android APK → Run workflow**。
3. 构建成功后，下载 **PocketLedger-debug-apk** 产物，解压后得到 `app-debug.apk`。

配置包含 APK 构建 / Lint / 单元测试，以及独立的 Android 35 模拟器数据库测试任务。2026-10-01 已完成云端构建和模拟器数据库测试。APK 为调试签名，适合个人测试；发布或长期升级应改为你自己保存的正式签名。云端调试密钥可能每次不同，换签名安装可能要求先卸载；请在卸载前导出账单。

## 首次使用

1. 打开“总览”，分别录入当前微信零钱、支付宝余额。
2. 打开“设置”，允许显示通知。
3. 开启“显示截图按钮”，阅读说明后在系统无障碍设置开启 **轻记账 · 截图按钮**。
4. 开启“自动记录支付成功通知”，在通知使用权设置允许 **轻记账 · 支付通知**。
5. 微信 / 支付宝付款后停留在成功页面，拉下通知栏，展开轻记账通知，点 **截图记账**。
6. 自动模式无需截图按钮：支付成功**系统通知**到来后，会自动处理。

Android 13 及以上侧载安装可能限制无障碍或通知使用权。若设置里不能开启，进入系统的应用信息页面，找到“允许受限设置”，再返回授权；不同系统的入口名称可能不同。没有该限制时无需额外操作。

OPPO 等手机若会清理后台，请在系统应用设置允许轻记账后台运行，必要时锁定最近任务。Android 14 起，“常驻”通知仍可能被手动划走；在设置页点“恢复通知栏按钮”即可。强行停止应用后，需要重新打开，必要时重新开启服务。

## 余额怎么算

余额是你输入的真实余额，减去**校准之后、明确使用账户余额支付**的消费。不是读取微信 / 支付宝账户的实时余额。

- 明确出现“支付方式：零钱 / 余额”等字段：扣减相应账户余额。
- 银行卡、花呗、零钱通、余额宝等付款：计入支出，不扣减微信零钱或支付宝余额。
- 未明确付款来源：计入支出，余额不扣减，明细提示待确认。可以在账单里改为余额付款。
- 校准之前的消费不会再次扣减。合并的记录只扣一次；确认是两笔后会重新计算。
- 收款、转账、充值、提现和退款不作为消费自动入账。这些导致实际余额变化时，请重新校准。

## 识别边界

付款成功页面会被 OCR 成文字，再使用保守规则提取金额：优先实付 / 付款金额，无法确定唯一金额时请你核对，不任意选最大数字。

微信应用内部的“微信支付”消息，不一定会产生安卓系统通知。没有系统通知、消息内容被隐藏、支付应用关闭了相关通知，均无法自动记账。截图按钮可以补充这些情况。

通知识别只接受微信 / 支付宝包名及支付类标题，过滤普通 MessagingStyle 聊天通知。安卓通知监听并不能证明微信内部的发件人身份，第三方应用可以自行决定通知格式。因此自动结果仍需根据实际手机格式验证；本版不承诺所有版本都准确覆盖。

禁止截屏的安全页面不能绕过。截屏需要你主动授予无障碍服务权限，本应用不使用持续录屏，也不会自动点击付款。

同应用同金额的两笔真实消费，也可能在匹配时间内被认为疑似重复，所以设置了核对通知与“恢复两笔”；时间范围可缩短。订单号能识别出来时会优先使用订单号。

## 数据与权限

- 中文 OCR 模型直接随 APK 打包，识别不需要 Google Play 服务下载模型。
- 截图只存在内存，OCR 完成后释放，不保存到相册，不上传。
- 数据库保存金额、应用、时间、商家、付款来源、订单号、来源标记，以及支付识别文字，供你核对。
- 自动模式会收到系统提供的通知回调，但代码在读取内容前先过滤应用，仅保存相关支付通知。
- 无障碍服务只接收窗口状态事件以判断前台应用，不读取控件树、不自动操控支付。
- Manifest 显式移除依赖可能合并进来的联网权限，不申请相册、摄像头或悬浮窗权限；截图闪光使用无障碍覆盖层。
- 数据位于应用私有目录，关闭系统备份，卸载会删除本机数据。CSV 是账单导出，不是可恢复所有设置的完整备份。

## 验证与开发

本机实际执行：

```text
PASS: 67 core assertions
PASS: Java syntax parsed for 16 source files (Android API compilation not performed)
PASS: 19 database scenarios, 66 assertions (host SQLite adapter; Android runtime not tested)
```

覆盖金额精度、金额歧义、失败 / 收款 / 退款过滤、付款来源判断、订单号和重复匹配。此外，GitHub Actions 已执行安卓 SDK 编译、单元测试、Lint 和 APK 权限检查，均通过。

```bash
bash scripts/test-core.sh
bash scripts/test-database-host.sh
java scripts/CheckJavaSyntax.java
# 配置好 Android SDK 和 Gradle 后：
gradle testDebugUnitTest lintDebug assembleDebug
# 连接 Android 12+ 设备或启动模拟器后：
gradle connectedDebugAndroidTest
```

Windows 核心测试：`powershell -ExecutionPolicy Bypass -File scripts\test-core.ps1`。

`LedgerDbTest` 提供 19 个数据库测试，覆盖合并顺序、只扣一次、拆分、余额校准、事件重放、待处理补记、删除 / 忽略回放、写入失败回滚、版本升级及月份边界；使用单独的测试数据库，不接触实际账本。

本机已通过 `scripts/test-database-host.sh` 执行同一份 `LedgerDb.java` 和数据库测试：少量 Android 数据库接口由宿主适配器连接 Python SQLite，**不是复制一份记账算法进行测试**。这证明本机 SQLite 上的逻辑、SQL、持久化和回滚场景通过，不代表 Android API 编译或设备行为通过。本地没有 Android SDK、模拟器或手机；**GitHub Actions 的 Android 35 模拟器已执行并通过同一份 19 个数据库仪器测试**。

源码入口：

- `MainActivity.java`：中文界面、统计、核对与权限说明。
- `CaptureAccessibilityService.java`：截图、闪光、OCR。
- `PaymentNotificationService.java`：新支付通知。
- `LedgerDb.java`：SQLite、原子合并、校准、导出。
- `core/PaymentParser.java`：可独立测试的识别规则。
- `core/DedupPolicy.java`：可独立测试的合并条件。

下一步实机验收：完成 `DEVICE-CHECKLIST.md`。如果你的支付通知或截图文字与规则不同，可提供已遮住姓名、订单号和其他敏感信息的样例，以便调整模板。

## 官方参考

- [无障碍截图与收起通知栏 API](https://developer.android.com/reference/android/accessibilityservice/AccessibilityService)
- [安卓通知监听](https://developer.android.com/reference/android/service/notification/NotificationListenerService)
- [打包中文 OCR 模型](https://developers.google.com/ml-kit/vision/text-recognition/v2/android)
- [Android 14 常驻通知行为](https://developer.android.com/about/versions/14/behavior-changes-all)
- [AGP 8.9 的 Gradle 版本要求](https://developer.android.com/build/releases/agp-8-9-0-release-notes)
- [Gradle 下载校验](https://gradle.org/release-checksums/)
