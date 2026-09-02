# Clash YAML Profile

一份面向个人使用的 Clash 分流配置，适用于 **Mihomo / Clash Meta** 内核，可导入 Clash Verge Rev、Mihomo Party、FlClash 等兼容客户端。

配置文件：[config.yaml](./config.yaml)

## 分流策略

规则按从上到下的顺序匹配，优先级如下：

| 流量类型 | 默认策略 | 可手动切换 |
|---|---|---|
| Adobe 相关域名 | 拦截（`REJECT`） | 否 |
| 内网及本地流量 | 直连 | 否 |
| OpenAI / ChatGPT | 美国节点 | 日本、新加坡、韩国、中国台湾、英国、自动选择 |
| Claude | 美国节点 | 英国、日本、新加坡、韩国、中国台湾、自动选择 |
| Gemini | 美国节点 | 英国、日本、香港、新加坡、韩国、中国台湾、自动选择 |
| 其他 AI 服务 | 美国节点 | 自动选择 |
| YouTube、Netflix、Pinterest 等流媒体 | 亚洲节点 | 日本、香港、新加坡、自动选择、直连 |
| 中国大陆网站与 IP | 直连 | 否 |
| 其他未命中流量 | 节点选择 | 任意可用节点或策略组 |

> 美国节点默认仅用于 AI 服务，以减少因频繁切换地区导致的账号风控风险。

## AI 地区支持说明

按各服务官方支持地区整理，官方不支持的地区不在可选列表中：

| 地区 | OpenAI | Claude | Gemini |
|---|:---:|:---:|:---:|
| 🇺🇸 美国 | ✅ | ✅ | ✅ |
| 🇬🇧 英国 | ✅ | ✅ | ✅ |
| 🇯🇵 日本 | ✅ | ✅ | ✅ |
| 🇸🇬 新加坡 | ✅ | ✅ | ✅ |
| 🇰🇷 韩国 | ✅ | ✅ | ✅ |
| 🇨🇳 中国台湾 | ✅ | ✅ | ✅ |
| 🇭🇰 香港 | ❌ | ❌ | ✅ |

## 节点分组

| 分组 | 类型 | 说明 |
|---|---|---|
| 🚀 Node Select | select | 默认出口，可选任意节点 / 策略组 |
| ♻️ Auto Select | url-test | 全部节点，自动选最低延迟 |
| 🌏 Asia | url-test | 日 / 港 / 新 / 韩 / 台，自动选最低延迟 |
| 🇯🇵 Japan | url-test | 日本节点池，自动选最低延迟 |
| 🇭🇰 Hong Kong | url-test | 香港节点池，自动选最低延迟 |
| 🇸🇬 Singapore | url-test | 新加坡节点池，自动选最低延迟 |
| 🇰🇷 South Korea | url-test | 韩国节点池，自动选最低延迟 |
| 🇨🇳 Taiwan(China) | url-test | 中国台湾节点池，自动选最低延迟 |
| 🇺🇸 United States | url-test | 美国节点池，自动选最低延迟 |
| 🇬🇧 United Kingdom | url-test | 英国节点池，自动选最低延迟 |
| 🌍 Streaming | select | 默认亚洲，可切日 / 港 / 新 / 自动 / 直连 |
| 🤖 AI | select | 其余 AI 服务通用出口，默认美国 |
| 🤖 OpenAI | select | 默认美国，**无香港选项**（官方不支持） |
| 🤖 Claude | select | 默认美国，**无香港选项**（官方不支持） |
| 🤖 Gemini | select | 默认美国，含香港选项（官方支持） |
| 🎯 CN Direct | select | 直连 |
| 🛑 Block | select | 拦截（Adobe 断网） |

## 使用方法

### 1. 准备订阅

打开 `config.yaml`，确认 `proxy-providers.Provider.url` 是你自己的有效订阅地址。建议不要把包含 token 的真实订阅地址提交到公开仓库。

```yaml
proxy-providers:
  Provider:
    type: http
    url: "你的订阅地址"
    interval: 172800     # 订阅更新间隔（秒），172800 = 每 2 天更新一次
```

### 2. 导入配置

**本地导入**

1. 下载 `config.yaml`
2. 打开 Clash 客户端的订阅或配置页面
3. 选择「导入本地文件」，选中下载的 YAML
4. 启用该配置并更新一次订阅

**通过 URL 导入**

- GitHub Raw：`https://raw.githubusercontent.com/fairylandturbo1010-byte/clash-yaml-profile/main/config.yaml`
- jsDelivr：`https://cdn.jsdelivr.net/gh/fairylandturbo1010-byte/clash-yaml-profile@main/config.yaml`

使用 URL 导入后，仓库中的配置更新可以由客户端重新拉取。GitHub Raw 或 CDN 可能存在缓存延迟。

### 3. 选择节点

导入成功后，在客户端的「代理」页面进行选择：

- `🚀 Node Select`：默认出口，可选择自动测速或指定节点
- `🌏 Asia` / `🇯🇵 Japan` / `🇭🇰 Hong Kong` …：各地区节点池，自动选该地区最低延迟节点
- `🌍 Streaming`：默认使用亚洲节点，也可切换到日本、香港、新加坡
- `🤖 OpenAI` / `🤖 Claude` / `🤖 Gemini`：默认使用美国节点，可切换到各自官方支持的地区
- `🤖 AI`：其余 AI 服务的通用出口，默认美国节点

节点选择会由客户端保存，重启后通常无需重新设置。

## 自定义配置

### 添加域名规则

在 `rules` 中添加规则，并放到兜底规则 `MATCH` 之前：

```yaml
- DOMAIN-SUFFIX,example.com,🚀 Node Select
- DOMAIN-SUFFIX,example.cn,🎯 CN Direct
- DOMAIN-SUFFIX,example.net,🛑 Block
```

规则从上到下匹配，越具体、优先级越高的规则应放得越靠前。

### 调整地区节点

各地区节点池通过 `proxy-groups` 中的 `filter` 正则筛选节点名称。如果订阅服务使用了不同的地区命名，请相应修改正则表达式：

```yaml
- name: 🇯🇵 Japan
  type: url-test
  use:
    - Provider
  filter: '(?i)\bJP\b|日本|东京|大阪|名古屋|Japan|Tokyo|Osaka|Nagoya'
```

### 恢复 Adobe 联网

当前配置会拦截 Adobe 主域名、关联产品及部分统计域名。需要正常使用 Adobe 在线服务时，请删除 `rules` 中 Adobe 拦截段；仅切换代理节点不会解除拦截。

## 常见问题

**订阅更新失败或没有节点**

- 检查 `proxy-providers.Provider.url` 是否有效或已经过期
- 在客户端中手动更新订阅
- 检查当前网络能否访问订阅服务
- 确认客户端使用 Mihomo / Clash Meta 内核

**某个地区节点池为空**

订阅中的节点名称可能无法匹配现有 `filter`。根据实际节点名称调整对应地区节点池的筛选正则。

**网站分流不符合预期**

- 检查配置是否处于 `rule` 模式
- 查看客户端连接日志，确认实际命中的规则
- 将自定义规则放在 `GEOSITE,cn`、`GEOIP,CN` 和最终 `MATCH` 规则之前

**Adobe 官网或应用无法联网**

这是 Adobe 拦截规则的预期行为。需要恢复访问时，请参照上方「恢复 Adobe 联网」。

> 注意：本配置仅作用于经过 Clash 的流量。桌面应用（如 Photoshop）若不走系统代理，需要开启客户端的 **TUN 模式**，或使用系统防火墙出站规则才能真正断网。

## 安全提示

- 不要在公开仓库中提交包含 token、用户名或密码的订阅地址
- 如果订阅地址曾公开，请立即到服务商后台重置订阅 token，并更新本地配置
- 建议将含真实订阅地址的配置保存在私有仓库，或只保留在本地
- 分享配置前，请用占位地址替换 `proxy-providers.Provider.url`

## 配置检查

修改 YAML 后，建议先使用客户端的配置校验功能确认语法无误，再启用新配置。若客户端提示不支持 `GEOSITE`、`GEOIP` 或 `proxy-providers`，请升级到较新的 Mihomo / Clash Meta 内核。
