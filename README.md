# 订阅转换脚本 - Subscription Converter

一个功能强大的订阅链接转换工具，支持多种协议和输出格式。

## ✨ 功能特点

- 🔗 **多协议支持**：VMess、VLESS、Shadowsocks、Trojan、Hysteria2
- 📁 **多种输入方式**：订阅链接、本地文件（支持混合协议）
- 🎯 **多种输出格式**：Clash YAML、V2Ray JSON
- 🔧 **兼容模式**：自动转换不支持的协议（Hysteria2 → VMess）
- 📊 **智能过滤**：自动过滤流量、到期时间等信息节点
- 🎨 **多种模板**：minimal、standard、advanced三种配置模板
- 🔢 **节点限制**：支持限制节点数量，避免配置文件过大
- 🛡️ **UUID修复**：自动验证和修复不标准的UUID格式
- 📈 **详细统计**：显示节点数量、协议分布等详细信息

## ✨ 特性

- 🚀 支持多种协议：Hysteria2、VMess、VLESS、Shadowsocks、Trojan
- 📱 支持多种输出格式：Clash、V2Ray
- 🎨 提供多种配置模板：最小化、标准、高级
- 🔄 自动Base64解码
- 📊 详细的转换统计信息
- 🛡️ 错误处理和异常捕获

## 📋 系统要求

- Python 3.6+
- PyYAML 库

## 🚀 安装

```bash
# 安装依赖
pip3 install PyYAML

# 下载脚本
# 脚本已经准备好，可以直接使用
```

## 📖 使用方法

### 基本用法

```bash
# 转换订阅链接为Clash配置（默认启用兼容模式）
python3 subscription_converter.py "你的订阅链接"

# 从本地文件转换节点（支持包含多种协议的文本文件）
python3 subscription_converter.py nodes.txt --file -o config.yaml

# 指定输出文件
python3 subscription_converter.py "你的订阅链接" -o my_config.yaml

# 使用高级模板
python3 subscription_converter.py "你的订阅链接" -t advanced

# 禁用兼容模式，保持原始协议（需要支持Hysteria2的客户端）
python3 subscription_converter.py "你的订阅链接" --no-compatible -o original.yaml

# 限制节点数量（适用于大型订阅）
python3 subscription_converter.py "你的订阅链接" --limit 100 -o limited_config.yaml

# 转换为V2Ray配置
python3 subscription_converter.py "你的订阅链接" -f v2ray -o v2ray_config.json
```

### 命令行参数

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `url` | 订阅链接URL或本地文件路径 | 必需 |
| `-f, --format` | 输出格式 (clash/v2ray) | clash |
| `-t, --template` | 配置模板 (minimal/standard/advanced) | standard |
| `-o, --output` | 输出文件名 | 自动生成 |
| `--test` | 测试模式，使用内置示例 | - |
| `--no-filter` | 不过滤信息节点（流量、到期时间等） | - |
| `--compatible` | 兼容模式，转换不支持的协议为兼容格式 | 默认启用 |
| `--no-compatible` | 禁用兼容模式，保持原始协议 | - |
| `--limit` | 限制节点数量（避免配置文件过大） | 无限制 |
| `--file` | 从本地文件读取节点内容 | - |
| `-c, --compact` | yaml 输出时每个 proxy 单独一行 | - |

### 配置模板说明

#### 1. Minimal (最小化)
- 最简单的配置
- 只包含基本的代理选择
- 适合简单使用场景

#### 2. Standard (标准)
- 平衡的配置
- 包含自动选择和手动选择
- 适合大多数用户

#### 3. Advanced (高级)
- 完整的配置
- 包含自动选择、故障转移、负载均衡
- 包含广告拦截和分流规则
- 适合高级用户

## 🔧 支持的协议

### Hysteria2
```
hysteria2://password@server:port/?params#name
```

### VMess
```
vmess://base64(json_config)
```

### VLESS
```
vless://uuid@server:port?params#name
```

### Shadowsocks
```
ss://base64(method:password)@server:port#name
```

### Trojan
```
trojan://password@server:port?params#name
```

## 📝 使用示例

### 示例1：基本转换
```bash
python3 subscription_converter.py "https://example.com/subscribe"
```

### 示例2：生成高级Clash配置
```bash
python3 subscription_converter.py "https://example.com/subscribe" \
  -t advanced \
  -o advanced_clash.yaml
```

### 示例3：转换为V2Ray配置
```bash
python3 subscription_converter.py "https://example.com/subscribe" \
  -f v2ray \
  -o v2ray_config.json
```

### 示例4：测试模式
```bash
python3 subscription_converter.py --test
```

## 📊 输出示例

脚本会显示详细的转换信息：

```
正在获取订阅: https://example.com/subscribe
✅ 订阅获取成功，内容长度: 1448
✅ Base64解码成功
📋 开始解析 8 行内容
🔍 解析第 1 行: hysteria2://...
✅ 成功解析: 美国 (hysteria2)
...
🎉 总共解析成功 8 个节点
✅ 配置文件已保存: my_config.yaml

==================================================
📊 转换统计信息
==================================================
总节点数: 8

协议分布:
  HYSTERIA2: 8 个

节点列表:
   1. 美国 (HYSTERIA2) - server1.com:26500
   2. 香港 (HYSTERIA2) - server2.com:26700
   ...
==================================================
```

## 🔍 故障排除

### 常见问题

1. **PyYAML 未安装**
   ```bash
   pip3 install PyYAML
   ```

2. **订阅链接无法访问**
   - 检查网络连接
   - 确认订阅链接有效
   - 检查是否需要代理访问

3. **解析失败**
   - 确认订阅内容格式正确
   - 检查是否为支持的协议类型

4. **权限错误**
   ```bash
   chmod +x subscription_converter.py
   ```

### 调试模式

使用测试模式验证脚本功能：
```bash
python3 subscription_converter.py --test
```

## 🤝 贡献

欢迎提交Issue和Pull Request来改进这个工具！

## 📄 许可证

MIT License

## 🙏 致谢

感谢所有开源项目的贡献者，特别是：
- Clash 项目
- V2Ray 项目
- PyYAML 库

---

**注意**: 请确保你有权使用提供的订阅链接，并遵守相关服务条款。

## 🔧 兼容性说明

### 默认兼容模式

**从v1.1版本开始，兼容模式默认启用**，这意味着：

✅ **自动转换不支持的协议**：Hysteria2 → VMess
✅ **兼容所有Clash客户端**：包括旧版Clash X
✅ **无需手动指定参数**：直接运行即可使用

### Hysteria2协议兼容性

如果你的Clash客户端不支持Hysteria2协议，会出现"不支持代理类型: hysteria2"的错误。

**现在的解决方案：**

1. **直接使用（推荐）**
   ```bash
   # 默认启用兼容模式，自动转换为VMess
   python3 subscription_converter.py "订阅链接" -o config.yaml

   # 使用便捷脚本
   ./convert.sh "订阅链接" -o config.yaml
   ```

2. **如果需要原始协议（高级用户）**
   ```bash
   # 禁用兼容模式，保持Hysteria2协议
   python3 subscription_converter.py "订阅链接" --no-compatible -o hysteria2_config.yaml
   ```

3. **升级到支持Hysteria2的客户端**
   - [Clash Verge Rev](https://github.com/clash-verge-rev/clash-verge-rev/releases) (推荐)
   - [ClashX Meta](https://github.com/MetaCubeX/ClashX.Meta/releases)

详细的兼容性指南请查看 [COMPATIBILITY_GUIDE.md](COMPATIBILITY_GUIDE.md)

## 📁 本地文件转换

### 支持的文件格式

脚本支持从本地文本文件读取节点信息，文件应该包含每行一个节点的URL格式：

```
vmess://eyJhZGQiOiAiMTA0LjIxLjgyLjE4MyI...
ss://Y2hhY2hhMjAtaWV0Zi1wb2x5MTMwNTpi...
trojan://74260653385993900@87.121.33.202...
vless://1052f24e-7b09-45eb-b0c5-d858eb124192...
hysteria2://letmein@example.com:443...
```

### 使用方法

```bash
# 创建节点文件
echo "vmess://eyJhZGQiOiAiMTA0LjIxLjgyLjE4MyI..." > nodes.txt
echo "ss://Y2hhY2hhMjAtaWV0Zi1wb2x5MTMwNTpi..." >> nodes.txt

# 转换为Clash配置
python3 subscription_converter.py nodes.txt --file -o my_config.yaml

# 使用高级模板
python3 subscription_converter.py nodes.txt --file -t advanced -o advanced_config.yaml

# 限制节点数量
python3 subscription_converter.py nodes.txt --file --limit 20 -o limited_config.yaml
```

### 特点

- ✅ **混合协议支持**：单个文件可包含多种协议的节点
- ✅ **自动解析**：智能识别不同协议格式
- ✅ **错误容忍**：跳过解析失败的行，继续处理其他节点
- ✅ **兼容模式**：默认启用，确保生成的配置可用于所有Clash客户端

## 🔧 兼容性说明

### 默认兼容模式

**从v1.1版本开始，兼容模式默认启用**，这意味着：

✅ **自动转换不支持的协议**：Hysteria2 → VMess
✅ **兼容所有Clash客户端**：包括旧版Clash X
✅ **无需手动指定参数**：直接运行即可使用

### Hysteria2协议兼容性

如果你的Clash客户端不支持Hysteria2协议，会出现"不支持代理类型: hysteria2"的错误。

**现在的解决方案：**

1. **直接使用（推荐）**
   ```bash
   # 默认启用兼容模式，自动转换为VMess
   python3 subscription_converter.py "订阅链接" -o config.yaml

   # 使用便捷脚本
   ./convert.sh "订阅链接" -o config.yaml
   ```

2. **如果需要原始协议（高级用户）**
   ```bash
   # 禁用兼容模式，保持Hysteria2协议
   python3 subscription_converter.py "订阅链接" --no-compatible -o hysteria2_config.yaml
   ```

3. **升级到支持Hysteria2的客户端**
   - [Clash Verge Rev](https://github.com/clash-verge-rev/clash-verge-rev/releases) (推荐)
   - [ClashX Meta](https://github.com/MetaCubeX/ClashX.Meta/releases)

详细的兼容性指南请查看 [COMPATIBILITY_GUIDE.md](COMPATIBILITY_GUIDE.md)