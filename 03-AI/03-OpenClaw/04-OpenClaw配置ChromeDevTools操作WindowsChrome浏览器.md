# OpenClaw Chrome DevTools 配置指南

本文介绍如何在 WSL 上配置 OpenClaw，使用 Chrome DevTools MCP 操作 Windows 上的 Chrome 浏览器。

## 前提条件

- OpenClaw 已安装并运行在 WSL 中
- Windows 上安装 Chrome 浏览器（版本 146+）
- WSL2 环境

## 配置步骤

### 1. Windows 端：启用 Chrome 远程调试

1. 在 Windows Chrome 浏览器地址栏输入：`chrome://inspect/#devices`
2. 勾选 **Allow remote debugging from this device**
3. Chrome 会在 `127.0.0.1:9222` 启动 DevTools MCP 服务器

### 2. Windows 端：配置端口转发

需要将 Windows 端口转发给 WSL：

```powershell
# 以管理员身份运行 PowerShell
netsh interface portproxy add v4tov4 listenport=9222 connectaddress=192.168.189.1 connectport=9222
```

注意：需要将 `192.168.189.1` 替换为你的 Windows 局域网 IP（通过 `ipconfig` 获取）

### 3. 验证 Windows Chrome DevTools 可访问

```bash
# 在 WSL 中测试
curl http://127.0.0.1:9222/json/version
```

应返回类似：
```json
{
  "Browser": "Chrome/146.0.0.0",
  "Protocol-Version": "1.3",
  ...
}
```

### 4. 配置 OpenClaw MCP

编辑 `~/.openclaw/openclaw.json`，添加 MCP 服务器配置：

```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["-y", "chrome-devtools-mcp"],
      "env": {
        "CHROME_DEBUGGING_PORT": "9222"
      }
    }
  }
}
```

### 5. 重启 OpenClaw Gateway

```bash
openclaw gateway restart
```

## 验证配置

在 OpenClaw 中使用 MCP 工具：
```
/mcp list    # 查看已连接的 MCP 服务器
```

## 常见问题

### 端口不可达

```bash
# 检查 Windows 防火墙
netsh advfirewall firewall add rule name="ChromeDevTools" dir=in action=allow localport=9222
```

### WSL 无法连接 Windows Chrome

```bash
# 获取 Windows IP
hostname -I

# 使用 Windows LAN IP 测试
curl http://<WINDOWS_IP>:9222/json/version
```

### MCP 启动失败

确保 Node.js 在 PATH 中：
```json
{
  "command": "/home/zhouyuming/.nvm/versions/node/v22.22.1/bin/npx"
}
```

## 相关资源

- [chrome-devtools-mcp npm](https://www.npmjs.com/package/chrome-devtools-mcp)
- [OpenClaw Chrome MCP Issue](https://github.com/openclaw/openclaw/issues/16567)
