# Ayabot 礼物统计查询插件

[![AstrBot](https://img.shields.io/badge/AstrBot-%3E%3D4.16-blueviolet)](https://github.com/AstrBot/AstrBot)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

> 配合 [Ayabot](https://github.com/yujianke100/Ayabot) B站直播间机器人使用，让 QQ 群成员可以随时查询自己的礼物投喂、盲盒数量与盈亏统计。

---

## ✨ 功能

| 功能 | 说明 |
|------|------|
| 🔗 **QQ-UID 绑定** | `/绑定 <你的B站UID>` 将 QQ 与 B站账号关联，一次绑定全群通用 |
| 🔓 **解绑** | `/解绑` 解除绑定 |
| 🎁 **礼物查询** | `/礼物查询 [today\|week\|month\|all]` 查看指定时间范围的礼物/盲盒统计 |
| 🖼️ **图片渲染** | 支持三种发送模式：纯文字 / 图片卡片 / 文字+图片同时发送 |
| 👑 **群配置管理** | 管理员可通过 `/设置API` 命令或 WebUI 配置 Ayabot 连接 |
| 📊 **详细数据** | 显示总投喂电池数、弹幕数、礼物数、盲盒数量及盈亏，含逐项明细 |

## 🖼️ 效果预览

```
📊 昵称（UID:123456）的本日礼物数据
━━━━━━━━━━━━━━━━━━
总投喂：2.5万 电池
弹幕数：42
礼物数：18
盲盒：1.2万 电池(+3k) 共8个 盈亏：成本 1.2万/产出 1.5万

礼物详情：
  小电视 x 2 5k 电池
  大宝剑 x 5 2.5k 电池
  ...

盲盒详情：
  幻彩宝箱 x 3 +1.2k：
    稀有道具A x 1 +2k 电池(+0.8k)
    普通道具B x 2 +0.4k 电池(+0.2k)
```

> 将 `render_mode` 设为 `image` 或 `both` 后可获得精美卡片样式图片渲染效果。

---

## 📦 安装

### 方式一：AstrBot WebUI 一键安装（推荐）

1. 打开 AstrBot WebUI → **插件管理** → **市场**
2. 搜索 `ayabot_stats` 或 `Ayabot 礼物统计查询`
3. 点击 **安装**
4. 重启 AstrBot

### 方式二：手动安装

```bash
# 进入 AstrBot 的插件目录
cd AstrBot/addons/

# 克隆本仓库
git clone https://github.com/yujianke100/astrbot_plugin_ayabot_stats.git

# 重启 AstrBot
```

---

## 🚀 快速开始

### 前置条件

1. 已部署并运行 [Ayabot](https://github.com/yujianke100/Ayabot)
2. 已开启 Ayabot 的外部 API 访问（WebUI → 数据管理 → **允许外部 API 访问**）

### 第一步：配置 Ayabot 连接

**在 Ayabot 端获取配置信息：**
1. 打开 Ayabot WebUI（默认 `http://<服务器IP>:19810`）
2. 进入 **数据管理** 页面
3. 找到目标直播间，点击 **开启 API**
4. 复制显示的 **API 地址** 和 **密钥**

**在 AstrBot 端完成配置：**

有两种方式可选：

<details>
<summary><b>🔧 方式 A：通过 WebUI 配置（推荐）</b></summary>

1. 打开 AstrBot WebUI → **插件管理** → 找到本插件 → **配置**（⚙️ 图标）
2. 在 **群配置** 中点击 **添加配置**
3. 填入：
   - **QQ 群号**：你的 QQ 群号
   - **API 地址**：从 Ayabot 复制的完整地址
   - **API 密钥**：从 Ayabot 复制的密钥
4. 点击 **保存配置**
5. **可选**：在 **查询结果发送方式** 中选择 `text` / `image` / `both`
</details>

<details>
<summary><b>💬 方式 B：通过群指令配置</b></summary>

群管理员在群中发送：

```
/设置API <API地址> <API密钥>
```

例如：
```
/设置API http://192.168.1.100:19810/api/external/user_stats?room_id=1992696442 a1b2c3d4e5f6
```
</details>

### 第二步：绑定 B站 UID

群成员在群中发送：

```
/绑定 12345678
```
> 将 `12345678` 替换为你的 B站 UID

### 第三步：查询数据

```
/礼物查询         # 查看本日数据
/礼物查询 today   # 同上
/礼物查询 week    # 本周
/礼物查询 month   # 本月
/礼物查询 all     # 全部记录
```

---

## ⚙️ 配置说明

### 插件配置项

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `render_mode` | string | `text` | 查询结果发送方式：`text` 纯文字 \| `image` 图片卡片 \| `both` 同时发送 |
| `groups` | template_list | — | QQ 群 API 配置列表，每个群独立配置 |
| `bindings_file` | string | `ayabot_bindings.json` | QQ-UID 绑定数据文件（全局通用，相对于 AstrBot data 目录） |

### 完整指令列表

| 指令 | 权限 | 说明 |
|------|------|------|
| `/绑定 <B站UID>` | 所有人 | 绑定自己的 QQ 到 B站 UID |
| `/解绑` | 所有人 | 解除绑定 |
| `/礼物查询 [today\|week\|month\|all]` | 所有人 | 查询礼物/盲盒统计 |
| `/设置API <地址> <密钥> [房间号]` | 管理员 | 设置当前群的 Ayabot API 配置 |
| `/查看API` | 所有人 | 查看当前群的 API 配置状态 |

---

## 📁 项目结构

```
astrbot_plugin_ayabot_stats/
├── main.py              # 插件主逻辑（指令处理、API 调用、HTML 渲染）
├── _conf_schema.json    # AstrBot WebUI 配置项定义
├── metadata.yaml        # 插件元数据
├── requirements.txt     # Python 依赖
├── README.md            # 本文件
└── LICENSE              # MIT 许可证
```

---

## 🛠️ 技术栈

- **运行环境**: Python 3.10+ / AstrBot >= 4.16
- **依赖**: `httpx` (HTTP 客户端)
- **服务端**: [Ayabot](https://github.com/yujianke100/Ayabot) (FastAPI)
- **图片渲染**: AstrBot 内置 HTML-to-image 引擎（需安装 Playwright）

---

## 🔐 隐私与安全

- 绑定数据存储在 AstrBot 服务端，仅存储 QQ 号与 B站 UID 的映射关系
- API 通信使用 Token 认证，密钥仅在配置时传输并脱敏显示
- 每个直播间使用独立 API 密钥，可随时在 Ayabot WebUI 上关闭或重新生成

---

## 📄 许可证

[MIT](LICENSE)

---

## 💡 反馈与贡献

发现 Bug 或有功能建议？欢迎提交 [Issue](https://github.com/yujianke100/astrbot_plugin_ayabot_stats/issues) 或 Pull Request。