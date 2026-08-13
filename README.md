#  Clash 配置文件

一套开箱即用的 Clash 分流配置：国内直连、AI 走美国、流媒体走亚洲、Adobe 断网、节点按国家/地区细分并自动选最低延迟。

## 分流策略总览

| 类型 | 策略 | 说明 |
|------|------|------|
| 🇨🇳 国内网站/应用 | **DIRECT 直连** | 百度、淘宝、B站、微信、微博等全部直连 |
| 🤖 AI 服务 | **🇺🇸 美国** | OpenAI/ChatGPT、Claude、Gemini、Grok、Copilot 等走美国节点 |
| 🎬 流媒体 | **🌏 亚洲** | YouTube、Instagram、Netflix、TikTok 等走亚洲节点，避开美国 |
| 🚫 Adobe 全线产品 | **REJECT 断网** | 阻止 Adobe 应用联网，防止授权验证弹窗/闪退 |
| 🌐 其他国外流量 | **🎯 兜底规则** | 可手动切换任意节点/分组 |

## 节点分组（11 个）

| 分组 | 类型 | 说明 |
|------|------|------|
| 🚀 手动选择 | select | 总入口，可手动指定任意分组/节点 |
| ♻️ 自动选择 | url-test | 全部节点，自动选最低延迟 |
| 🌏 亚洲 | url-test | 亚洲汇总（日/港/新/韩/台），自动选最低延迟 |
| 🇯🇵 日本 | url-test | 日本节点（15 个），自动选最低延迟 |
| 🇭🇰 香港 | url-test | 香港节点（9 个），自动选最低延迟 |
| 🇸🇬 新加坡 | url-test | 新加坡节点（12 个），自动选最低延迟 |
| 🇰🇷 韩国 | url-test | 韩国节点（2 个），自动选最低延迟 |
| 🇨🇳 台湾 | url-test | 台湾节点（2 个），自动选最低延迟 |
| 🇺🇸 美国 | url-test | 美国节点（12 个），自动选最低延迟 |
| 🇬🇧 英国 | url-test | 英国节点（3 个），自动选最低延迟 |
| 🎯 兜底规则 | select | 可手动选择任意分组（含自动选择） |

> 大洲分组说明：亚洲含多国节点故单独建「🌏 亚洲」分组；欧洲仅英国、北美仅美国，按需求不设洲级分组，直接使用「🇬🇧 英国」「🇺🇸 美国」。

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
https://raw.githubusercontent.com/fairylandturbo1010-byte/company-profile/main/config.yaml
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

本配置拦截 Adobe 相关域名，防止非正版 Adobe 应用联网触发授权验证：

- Adobe 主域名（adobe.com、adobe.io 等）
- Creative Cloud 服务
- 登录/认证（ims-na1.adobelogin.com 等）
- 许可证管理（lm.licenses.adobe.com 等）
- 正版验证（genuine.adobe.com 等）
- Adobe Fonts / Typekit / Behance
- 所有含 `adobe` 关键词的域名（兜底拦截）

> ⚠️ 提示：此规则只对「经过 Clash 的流量」生效。Photoshop 等桌面软件可能不读系统代理、绕过 Clash 直连。建议开启 **TUN 模式**（Clash Verge Rev → 设置 → TUN 模式），或配合 Windows 防火墙阻止 Photoshop.exe 出站，才能 100% 保证断网。

## 自定义修改

编辑 `config.yaml`：

- 添加断网域名：`- DOMAIN-SUFFIX,xxx.com,REJECT`
- 添加直连域名：`- DOMAIN-SUFFIX,xxx.com,DIRECT`
- 添加 AI 域名走美国：`- DOMAIN-SUFFIX,xxx.com,🇺🇸 美国`
- 添加流媒体走亚洲：`- DOMAIN-SUFFIX,xxx.com,🌏 亚洲`
- 修改某类流量走指定国家：把规则目标改成对应国家分组名（如 `🇯🇵 日本`）

## 常见问题

**Q：导入后没有节点？**
订阅链接已内置并验证有效。若节点仍为空，请检查网络能否访问 `liangxin.xyz`，或在客户端手动「更新」订阅。

**Q：测速出现 timeout？**
各分组均使用 `url-test` 类型，会自动跳过超时节点，选择延迟最低的可用节点，不会因单个节点超时而中断。
