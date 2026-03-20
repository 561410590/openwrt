# OpenWrt 设备支持 PR 后续执行清单（中文）

本清单用于 **已提交 GitHub PR** 后，提升 OpenWrt 设备支持补丁被审阅/合并概率。

## 1. 先补齐 PR 描述（强烈建议）

请在 PR 描述中补齐：

- 设备完整型号 + 硬件版本（如 v1/v2）
- 刷机路径（factory / sysupgrade）
- 已验证功能：
  - 启动
  - 网口（LAN/WAN）
  - Wi-Fi（2.4G/5G）
  - LED
  - 按键（reset/wps）
- 关键日志片段（建议贴）：
  - `dmesg | head -n 80`
  - 分区识别/网卡识别相关日志
- 已知限制（如果有）

### 1.1 可直接粘贴的 PR 描述模板（含你的设备示例）

```markdown
## 设备信息
- 型号：Nradio C8-688
- 硬件版本：<如 v1>
- SoC：MT7981B
- 网口：3 x 1G LAN，1 x 2.5G WAN
- 闪存/内存：<可选>

## 刷机与启动验证
- 首次刷机（factory）：OK / N/A
- 升级（sysupgrade）：OK / N/A
- 启动：OK

## 功能验证
- LAN/WAN：OK / 问题说明
- Wi-Fi 2.4G/5G：OK / N/A / 问题说明
- LED：OK / 问题说明
- 按键（reset/wps）：OK / 问题说明

## 日志（节选）
- dmesg: <贴关键片段>
- 分区识别: <贴关键片段>
- 网卡识别: <贴关键片段>

## 已知限制
- <没有可写 None>
```

## 2. 本地做 patch 自检

在你的分支执行：

```bash
./scripts/checkpatch.pl -g HEAD
```

若是多提交：

```bash
./scripts/checkpatch.pl -g <base_commit>..HEAD
```

说明：该脚本默认会检查 sign-off（`Signed-off-by`）相关规范。

## 3. 把补丁同步到 openwrt-devel 邮件列表

OpenWrt 维护流程中，邮件列表是核心审阅渠道。建议将与 PR 对应的提交通过邮件发送。

### 3.1 生成补丁

```bash
git format-patch -s -1 HEAD
```

或多提交：

```bash
git format-patch -s <base_commit>..HEAD
```

### 3.2 发送（示例）

```bash
git send-email \
  --to openwrt-devel@lists.openwrt.org \
  *.patch
```

> 可在邮件正文中附上 GitHub PR 链接，说明两边内容一致。

## 4. 跟进节奏建议

- 先等待 7~14 天（避免频繁催促）
- 无回复时，发一条简短跟进：
  - 补充测试信息
  - 说明是否已 rebase 到最新 master

## 5. 可直接复用的跟进模板

```text
Hi maintainers,

Gentle ping for review of this device support series:
- GitHub PR: <your_pr_url>

I have verified:
- Boot: OK
- Ethernet (LAN/WAN): OK
- Wi-Fi: OK / N/A
- LEDs/Buttons: OK
- Sysupgrade: OK / N/A

If needed, I can split commits or provide additional logs/tests.

Thanks!
```

## 6. 合并前最终检查

- [ ] 每个 commit 都有 `Signed-off-by`
- [ ] commit message 结构清晰（target/subtarget 前缀等）
- [ ] DTS、映像配置、文档改动尽量按逻辑拆分
- [ ] 无明显 checkpatch 报错
- [ ] PR 描述包含测试结果与已知限制

