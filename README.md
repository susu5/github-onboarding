# 🐙 GitHub Onboarding Skill

> Step-by-step GitHub onboarding: register account, create repo, upload local code.

---

## 中文描述

**GitHub Onboarding Skill** 是一个完整的 GitHub 入门引导技能，通过浏览器自动化和 Git CLI 命令，引导用户完成 GitHub 全流程：

1. **注册** — 通过浏览器自动化创建 GitHub 账号
2. **创建** — 新建仓库（公开或私有）
3. **上传** — 初始化本地项目并推送到 GitHub

### 使用方式

在 WorkBuddy / CodeBuddy 中加载此 Skill 后，用以下任意方式触发：

```
帮我注册GitHub
创建GitHub项目
把我的代码上传到GitHub
push project to GitHub
create a GitHub repo
```

Skill 会自动引导你完成：
- 账号注册（含邮箱验证、CAPTCHA 处理）
- Git 认证配置（GitHub CLI / PAT / SSH 三种方式）
- 仓库创建
- `.gitignore` 自动生成（支持 Node/Python/Java/Rust/Go/.NET）
- 代码提交与推送

### 包含文件

| 文件 | 说明 |
|------|------|
| `SKILL.md` | 完整引导流程定义 |
| `scripts/generate_gitignore.py` | 自动检测项目类型并生成 `.gitignore` |
| `references/github_tips.md` | Git / GitHub CLI 速查手册 |

---

## English Description

**GitHub Onboarding Skill** guides users through the complete GitHub onboarding journey:

1. **Register** — Create a new GitHub account via browser automation
2. **Create** — Set up a new repository (public or private)
3. **Upload** — Initialize a local project with Git and push it to GitHub

### Usage

Load this Skill in WorkBuddy / CodeBuddy, then trigger with:

```
帮我注册GitHub
create a GitHub repo
upload my code to GitHub
push project to GitHub
```

The Skill automatically guides you through:
- Account registration (email verification, CAPTCHA handling)
- Git authentication (GitHub CLI / PAT / SSH — three options)
- Repository creation
- `.gitignore` auto-generation (Node/Python/Java/Rust/Go/.NET)
- Code commit & push

### Included Files

| File | Description |
|------|-------------|
| `SKILL.md` | Full workflow definition |
| `scripts/generate_gitignore.py` | Auto-detect project type & generate `.gitignore` |
| `references/github_tips.md` | Git / GitHub CLI cheatsheet |

---

## License

MIT
