# wecom-gui-message

通过 macOS GUI 自动化给企业微信（WeCom）联系人或群发消息。

使用 peekaboo + screencapture + Swift Vision OCR + cliclick 实现全流程自动化。

## 前置条件

- macOS（arm64, Retina 2x）
- 企业微信桌面客户端（`com.tencent.WeWorkMac`）
- 工具：peekaboo, cliclick, swift
- 系统授权：Screen Recording + Accessibility

## 安装

### ClawHub

```bash
clawhub install wecom-gui-message
```

### 手动安装

```bash
git clone https://github.com/jacky-wzj/wecom-gui-message.git ~/.openclaw/skills/wecom-gui-message
```

## 使用

```bash
python3 scripts/send_message.py "联系人或群名" "消息内容"
python3 scripts/send_message.py "杨杨杨" "今日AI日报\nhttps://example.com" --wait-login
```

| 参数 | 说明 |
|------|------|
| `--wait-login` | 需登录时自动轮询等待扫码 |
| `--timeout N` | 登录等待超时秒数（默认 120） |

| Exit Code | 含义 |
|-----------|------|
| 0 | 发送成功 |
| 1 | 发送失败 |
| 2 | 需要登录 |

## 核心流程

```
激活企微 → 调整窗口 → 登录检测 → 等待弹窗消失 → 消息列表OCR查找 → 点击目标 → 关闭侧面板 → 点击输入框 → 粘贴 → 回车发送 → 验证
```

## 关键设计决策

1. **窗口截图** > 全屏截图（全屏可能 OCR 返回空）
2. **消息列表 OCR** > Cmd+F 搜索（搜索对外部联系人不可靠）
3. **坐标换算**：screen = window_pos + ocr_window_coord
4. **输入框坐标定位** > OCR 找 placeholder（企微无 placeholder）

## License

MIT
