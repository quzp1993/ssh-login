# SSH Login - Claude Code Skill

SSH 服务器连接与免密登录配置助手，简化 Claude Code 中的服务器操作流程。

## 功能介绍

本 skill 提供以下功能：

- 🚀 快速连接 SSH 服务器
- 🔑 自动配置免密登录
- 🔍 常见问题排查
- 📝 完整的配置指南

## 安装步骤

### 1. 复制 skill 文件

将 `skills/ssh-login.md` 文件复制到您的 Claude Code skills 目录：

```
~/.claude/skills/ssh-login.md
```

或者在 Windows 上：

```
C:\Users\YourName\.claude\skills\ssh-login.md
```

### 2. 重启 Claude Code

重启 Claude Code 使 skill 生效。

## 使用方法

### 基本连接

直接在对话中提出需求：

```
帮我连接服务器
```

skill 会引导您输入：
- 服务器 IP 地址
- 用户名
- 端口（如非默认 22）

### 配置免密登录

```
帮我配置免密登录
```

skill 会自动：
1. 检查现有 SSH 密钥
2. 生成新的密钥对（如需要）
3. 指导上传公钥到服务器
4. 测试免密连接

## 快速开始示例

### 连接到服务器

```bash
ssh licun@172.16.100.7
```

### 使用 ssh-copy-id 配置免密登录

```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519 -N ""
ssh-copy-id -i ~/.ssh/id_ed25519.pub licun@172.16.100.7
```

### 手动添加公钥

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
echo "ssh-ed25519 AAAA... your_key" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

## 常见问题

### 连接超时

```bash
ping 服务器IP
```

检查网络连通性、防火墙设置和 SSH 服务状态。

### 权限被拒绝

检查文件权限：
```bash
ls -la ~/.ssh/authorized_keys  # 应为 600
ls -la ~/.ssh/                  # 应为 700
```

### 密钥认证失败

确认公钥已正确添加到服务器的 `~/.ssh/authorized_keys` 文件中。

## 安全建议

- 🔐 不要在命令行中传递密码
- ✅ 首次连接时验证主机指纹
- 🔑 使用 ed25519 算法（更安全且更小）
- 🔄 定期轮换 SSH 密钥

## 文件说明

| 文件 | 说明 |
|------|------|
| `skills/ssh-login.md` | Claude Code skill 文件 |
| `docs/ssh-login-guide.md` | 完整的 SSH 配置指南文档 |

## 许可证

本项目采用 MIT 许可证 - 详见 [LICENSE](LICENSE) 文件

## 贡献

欢迎提交 Issue 和 Pull Request！
