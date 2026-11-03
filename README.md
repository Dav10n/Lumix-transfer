# Luma 快传

面向 Panasonic LUMIX S5M2 的非官方 Android 图传工具。目标是只做两件事：

1. 稳定连接相机 Wi-Fi。
2. 浏览并传输相机中的照片、RAW 和视频。

不包含遥控拍摄、实时取景、修图、LUT、云同步或社交分享。项目基于
Apache-2.0 许可的 [LumaLink](https://github.com/Angelnable/LumaLink)
传输内核整理和修改，与 Panasonic 没有隶属、授权或背书关系。

> 当前版本：`1.0.1-s5m2`  
> Android：10（API 29）及以上  
> 应用包名：`com.lumalink.g100d.transfer`

## 功能

- 扫描或手动输入 LUMIX 相机热点 SSID
- 绑定相机 Wi-Fi 网络，避免 Android 在移动数据与相机网络之间错误切换
- 浏览 JPEG、RW2/RAW、MP4、MOV、MTS 等媒体
- 按全部、照片、视频、JPG、RAW 筛选
- 卡 1 / 卡 2 浏览切换
- 快速读取最近 150 项，避免每次进入都完整扫描存储卡
- “再读 150 项”和“完整扫描”
- 批量选择、下载队列、前台服务、通知进度
- 取消、失败重试、HTTP Range 断点续传
- 文件长度、JPEG/MP4/RW2 结构和本地 SHA-256 校验
- 照片按需预览：先显示相机缩略图，再加载相机大预览
- JPEG/视频保存到系统媒体库，RAW 保存到 Downloads
- 诊断日志，便于反馈连接、目录读取和传输问题

## 下载与安装

可在 GitHub Releases 中下载：

```text
LumaTransfer-S5M2-1.0.1.apk
```

SHA-256：

```text
43b6df96d2027b5a58734596f46cc8601ad71928e51ffe14f2317edc3d2bf348
```

当前 APK 是个人旁加载测试包，使用本地调试证书签名。它可以正常安装和覆盖升级，
但不适合直接作为 Google Play 正式发行包。长期公开发布前应使用独立、妥善备份的
release keystore 重新签名。

## 连接步骤

1. 关闭 LUMIX Lab、LUMIX Sync 或其他可能占用相机连接的 App。
2. 在 S5M2 中打开 Wi-Fi 连接：
   `MENU → Wi-Fi → Wi-Fi 功能 → 新建连接 → 遥控拍摄与查看`
3. 打开 Luma 快传，点击“连接相机”，选择相机显示的 SSID。
4. 如果 Android 提示此 Wi-Fi 无法上网，选择“保持连接”。
5. 相机屏幕出现访问确认时选择允许。
6. 进入“浏览相机文件”，选择文件并加入传输队列。

大文件传输时，建议把 Luma 快传的电池策略设为“无限制”。相机热点通常没有互联网，
传输期间不要让系统自动切换到移动数据，VPN 也可能中断本地相机连接。

## S5M2 双卡

S5M2 的相机文件接口一次只提供当前选中的卡槽。App 会在文件页提供：

```text
卡 1 / 卡 2
```

这表示“现在浏览哪张卡”，不是把两张卡合并成一个目录。对“照片存卡 1、视频存卡 2”
这类分配录制配置：

- “双卡槽功能 / 分配录制”是相机自己的拍摄规则。
- App 不发送修改双卡槽功能或分配录制的命令。
- App 使用 `setsetting current_sd` 切换当前浏览卡槽。
- 切换命令只会在用户点击卡槽时发送，传输、录像、写卡和间隔拍摄期间会被阻止。
- 建议在传输结束后检查相机显示，确认双卡槽功能仍为“分配录制”。

由于这是非官方协议，不同固件可能对 `current_sd` 有细微差异。无法确认相机空闲时，
App 会拒绝切换，而不是猜测继续执行。

## 视频与预览

S5 系列的视频标题可能是 `106-7148`，不一定带扩展名。App 会同时根据以下信息判断类型：

- `upnp:class` 中的 `videoItem`
- `.mp4`、`.mov`、`.mts`、`.m2ts` 扩展名
- `PANASONIC.COM_PN=CAM_AVC_*`

点击“视频”筛选且最近文件里没有视频时，App 会自动继续扫描更早的文件。

预览需要手动打开“预览”开关。缩略图由相机逐张提供，速度取决于相机热点、手机 Wi-Fi
和当前相机状态。点开照片时会先显示已缓存的缩略图，再请求相机大预览，不会先下载整张原图。

## 文件保存位置

| 类型 | 位置 |
| --- | --- |
| JPEG | 系统相册 `Pictures/Luma Transfer` |
| MP4/MOV/MTS | 系统相册 `Movies/Luma Transfer` |
| RAW | `Downloads/Luma Transfer/RAW` |

如果 MediaStore 写入失败，App 会回退到应用专属目录并记录实际路径。

## 安全与隐私

- 不申请通讯录、短信、麦克风、摄像头、全盘存储或安装应用权限。
- 不上传照片或视频到任何远程服务器。
- 相机请求被限制在本地/私有 IP 地址。
- 不发送拍照、录像、实时取景、删除、格式化、固件或寄存器命令。
- 文件访问只读；传输必须由用户选择并触发。
- 唯一会改变相机状态的命令是播放模式切换和用户明确选择的当前卡槽。
- 卡槽切换前会读取相机状态，录像、写卡或间隔拍摄时不执行。

更完整的安全边界见 [SECURITY.md](SECURITY.md)。

## 已知限制

- Panasonic 没有公开 S5M2 的这套 HTTP/UPnP API，协议来自社区互操作研究。
- 相机接口一次只显示当前卡槽，不能把两张卡直接合并浏览。
- S5M2 每页通常最多返回 50 项，每次授权可浏览的数量有限；完整扫描大容量卡可能较慢。
- 缩略图请求需要逐张经过相机文件授权，预览速度受相机服务限制。
- 当前 APK 使用调试证书签名。
- 当前 1.0.1 已完成自动化测试，但不同 S5M2 固件和手机厂商仍需要更多真机验证。

如果相机出现无响应，先停止操作并关闭相机后重新开机；不要连续发送连接或模式切换请求。

## 构建

要求：

- JDK 17
- Android SDK 35
- Gradle 8.9，项目包含 wrapper

在项目根目录创建不提交的 `local.properties`：

```properties
sdk.dir=/path/to/Android/Sdk
```

构建调试包：

```bash
./gradlew :app:assembleDebug
```

运行测试：

```bash
./gradlew :protocol:test :core:testDebugUnitTest
```

输出：

```text
app/build/outputs/apk/debug/app-debug.apk
```

GitHub Actions 会在 push 和 pull request 时运行测试并构建调试 APK。

## 工程结构

```text
app/        应用入口、导航、设置和关于页面
core/       数据模型、Room、设置和诊断
protocol/   LUMIX HTTP、UPnP、访问授权、文件解析
transport/  Wi-Fi 绑定、连接界面、传输队列和前台服务
ui/         Compose 设计组件和主题
docs/       协议、测试和发布文档
```

## 自动化验证

当前版本执行了：

- 36 个协议测试
- 5 个核心模块测试
- Android Release 构建与 Lint Vital 检查
- APK v2 签名校验

测试覆盖访问授权、UPnP Browse、S5 视频解析、双卡文件重名、卡槽切换命令、
最近文件分页、下载路径和结构校验。

## 参与贡献

见 [CONTRIBUTING.md](CONTRIBUTING.md)。真机兼容性报告尤其需要以下信息：

- 相机型号和固件版本
- Android 版本和手机型号
- 连接模式：相机热点 / 路由器 / 手机热点
- 卡槽状态和双卡槽功能
- 失败发生的阶段和界面错误信息

维护者发布 GitHub Release 的步骤见 [docs/RELEASING.md](docs/RELEASING.md)。

## 上游与许可

本项目基于 LumaLink 4.3.11 的 Apache-2.0 传输内核进行了 S5M2 专用修改，
保留原项目归属和 NOTICE。修改内容包括品牌、应用标识、视频类型解析、
当前卡槽切换、最近文件优先加载和预览流程。

许可证：[Apache License 2.0](LICENSE)  
第三方组件：[THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)

## 免责声明

Luma 快传是非官方第三方互操作工具。Panasonic 和 LUMIX 是 Panasonic Holdings
Corporation 的商标，仅用于说明兼容设备。使用本软件造成的相机模式异常、
连接中断或文件传输失败，应先停止操作并检查相机和存储卡状态。请自行备份重要素材。
