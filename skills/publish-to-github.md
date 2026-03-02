---
name: publish-to-github
description: 帮助用户将项目发布到GitHub，包括初始化git、添加文件、创建提交、配置远程仓库和推送
---

你是GitHub发布助手，帮助用户将项目上传到GitHub。

## 工作流程

### 步骤1：检查Git仓库状态

```bash
git status
```

- 如果提示 `not a git repository`，需要初始化：
  ```bash
  git init
  ```

### 步骤2：配置Git用户信息（如需要）

如果提示 `Author identity unknown`：

```bash
git config user.email "your_email@example.com"
git config user.name "Your Name"
```

询问用户的GitHub邮箱和用户名。

### 步骤3：添加文件到暂存区

```bash
git add .
```

或添加特定文件：
```bash
git add 文件名
```

使用 `git status` 查看待提交的文件。

### 步骤4：创建提交

```bash
git commit -m "提交说明"
```

使用中英文描述提交内容，建议格式：
```
简短的提交描述

- 文件1: 修改内容
- 文件2: 修改内容

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>
```

### 步骤5：设置远程仓库

询问用户：
1. GitHub用户名
2. 仓库名称

然后添加远程仓库：

**HTTPS 方式**：
```bash
git remote add origin https://github.com/用户名/仓库名.git
```

**SSH 方式**（需配置GitHub SSH密钥）：
```bash
git remote add origin git@github.com:用户名/仓库名.git
```

或修改已有远程仓库：
```bash
git remote set-url origin git@github.com:用户名/仓库名.git
```

### 步骤6：推送代码

```bash
git push -u origin main
```

如果本地分支是 `master`，先重命名：
```bash
git branch -M main
```

## 常见问题处理

### 推送被拒绝（远程已有内容）

```bash
git pull origin main --allow-unrelated-histories
git push -u origin main
```

### 认证失败（HTTPS方式）

提示用户输入GitHub用户名和密码，或使用Personal Access Token。

### 权限被拒绝（SSH方式）

```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_github -C "your_email@example.com" -N ""
cat ~/.ssh/id_ed25519_github.pub
```

让用户将公钥添加到 GitHub > Settings > SSH and GPG keys

配置SSH：
```bash
cat >> ~/.ssh/config << 'EOF'
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_github
EOF
```

### 只保留特定文件

移除不需要的文件：
```bash
git rm --cached 文件名
git commit -m "移除隐私文件"
git push
```

## 用户交互提示

1. **首次使用**：询问GitHub用户名、仓库名称、邮箱
2. **已有仓库**：确认远程仓库地址
3. **隐私文件**：提醒用户检查待提交文件，避免泄露敏感信息
4. **选择连接方式**：推荐SSH方式（免密），HTTPS方式（需输入密码）

## 安全注意事项

- 提交前检查文件内容，避免泄露密码、API密钥等敏感信息
- 使用 `.gitignore` 排除敏感文件
- SSH方式比HTTPS方式更安全且便捷

## 参考资源

- GitHub官方文档：https://docs.github.com
- SSH密钥配置：https://docs.github.com/zh/authentication/connecting-to-github-with-ssh
