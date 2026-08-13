# Company Profile - Clash 配置文件

一套开箱即用的 Clash 分流配置：国内直连、AI 走美国、流媒体走亚洲、Adobe 断网、节点自动选最低延迟。

## 分流策略总览

| 类型 | 策略 | 说明 |
|------|------|------|
| 🇨🇳 国内网站/应用 | **DIRECT 直连** | 百度、淘宝、B站、微信、微博等全部直连，不经过代理 |
| 🤖 AI 服务 | **🇺🇸 美国节点** | OpenAI/ChatGPT、Claude、Gemini、Grok、Copilot 等走美国节点 |
| 🎬 流媒体 | **🌏 亚洲节点** | YouTube、Instagram、Netflix、TikTok 等走日本/香港/新加坡节点，避开美国 |
| 🚫 Adobe 全线产品 | **REJECT 断网** | 阻止 Adobe 所有应用联网，防止授权验证弹窗/闪退 |
| 🌐 其他国外流量 | **♻️ 自动选择** | 全部节点中自动选最低延迟 |

## 节点分组

配置通过 `filter` 自动按地区筛选订阅节点：

- **♻️ 自动选择**：全部节点，按最低延迟自动切换
- **🇺🇸 美国节点**：节点名含「美国」的节点（自动选最低延迟）
- **🌏 亚洲节点**：节点名含「日本/香港/新加坡/韩国/台湾」的节点（自动选最低延迟）
- **🚀 手动选择**：总入口，可手动指定节点

## 使用方法

### 1. 订阅链接已内置

`config.yaml` 中已填入订阅链接：

```yaml
proxy-providers:
  Company-Profile:
    type: http
    url: "https://liangxin.xyz/api/v1/liangxin?OwO=..."
    interval: 172800   # 每 48 小时更新一次
```

> 如需更换订阅，只需修改 `url` 的值。`interval` 单位为秒，`172800` = 每 2 天更新一次。

### 2. 导入 Clash

**方式一：GitHub Raw 链接订阅（推荐）**

在 Clash 客户端（Clash Verge Rev / ClashX / Clash for Windows 等）中添加订阅，填入：

```
https://raw.githubusercontent.com/<你的用户名>/company-profile/main/config.yaml
```

**方式二：本地导入**

下载 `config.yaml`，在客户端中「导入本地配置文件」。

### 3. 支持的客户端

- Clash Verge / Clash Verge Rev（Mihomo 内核）
- Clash for Windows
- ClashX / ClashX Pro（macOS）
- Clash for Android
- Stash / Shadowrocket 等（支持 Clash 订阅）

## Adobe 断网说明

本配置拦截了 Adobe 相关域名，防止非正版 Adobe 应用联网触发授权验证导致弹窗/闪退：

- Adobe 主域名（adobe.com、adobe.io 等）
- Creative Cloud 服务
- 登录/认证（ims-na1.adobelogin.com 等）
- 许可证管理（lm.licenses.adobe.com 等）
- 正版验证（genuine.adobe.com 等）
- Adobe Fonts / Typekit / Behance
- 所有含 `adobe` 关键词的域名（兜底拦截）

## 自定义修改

编辑 `config.yaml` 的 `rules` 部分：

- 添加断网域名：`- DOMAIN-SUFFIX,xxx.com,REJECT`
- 添加直连域名：`- DOMAIN-SUFFIX,xxx.com,DIRECT`
- 添加 AI 域名走美国：`- DOMAIN-SUFFIX,xxx.com,🇺🇸 美国节点`
- 添加流媒体走亚洲：`- DOMAIN-SUFFIX,xxx.com,🌏 亚洲节点`

## 常见问题

**Q：导入后没有节点？**
订阅链接已内置并验证有效。若节点仍为空，请检查网络能否访问 `liangxin.xyz`，或在客户端手动「更新」订阅。

**Q：测速出现 timeout？**
`自动选择/美国/亚洲` 分组均使用 `url-test` 类型，会自动跳过超时节点，选择延迟最低的可用节点，不会因单个节点超时而中断。
