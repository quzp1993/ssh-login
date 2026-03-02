# SSH 服务器连接与免密登录指南

## 1. 基本连接

### 语法
```bash
ssh 用户名@服务器IP
```

### 示例
```bash
ssh licun@172.16.100.7
```

### 指定端口（非默认22端口）
```bash
ssh -p 端口号 用户名@服务器IP
```

## 2. 免密登录配置

### 步骤1：检查现有SSH密钥
```bash
ls ~/.ssh/id_rsa ~/.ssh/id_ed25519 2>/dev/null || echo "No SSH keys found"
```

### 步骤2：生成SSH密钥对（如果没有）
```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519 -N ""
```

### 步骤3：上传公钥到服务器

#### 方法A：使用 ssh-copy-id（推荐）
```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub 用户名@服务器IP
```

#### 方法B：手动添加
1. 登录服务器：
   ```bash
   ssh 用户名@服务器IP
   ```

2. 在服务器上执行：
   ```bash
   mkdir -p ~/.ssh
   chmod 700 ~/.ssh
   echo "你的公钥内容" >> ~/.ssh/authorized_keys
   chmod 600 ~/.ssh/authorized_keys
   ```

### 步骤4：测试免密登录
```bash
ssh 用户名@服务器IP
```

## 3. 常见问题排查

### 连接超时
```bash
ping 服务器IP
```

检查防火墙、网络连通性、SSH服务状态。

### 权限被拒绝
检查服务器端文件权限：
```bash
# 检查 authorized_keys 权限（应为 600）
ls -la ~/.ssh/authorized_keys

# 检查 .ssh 目录权限（应为 700）
ls -la ~/.ssh/
```

### 查看公钥内容
```bash
cat ~/.ssh/id_ed25519.pub
```

### 查看服务器端授权密钥
```bash
cat ~/.ssh/authorized_keys
```

## 4. 安全注意事项

- 不要在命令行或脚本中直接包含密码
- 首次连接时验证主机指纹
- 使用 ed25519 算法（更安全且更小）
- 定期轮换SSH密钥

## 5. 常用SSH命令

```bash
# 指定密钥文件
ssh -i ~/.ssh/your_key 用户名@服务器IP

# 禁用主机密钥检查（仅用于测试）
ssh -o StrictHostKeyChecking=no 用户名@服务器IP

# 执行远程命令
ssh 用户名@服务器IP "命令"

# 端口转发
ssh -L 本地端口:目标IP:目标端口 用户名@服务器IP
```
