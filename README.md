# Docmost Wiki Skill

用于操作 Docmost Wiki 的 AI Agent 技能。

## 触发词

`/dc` / `docmost` / `wiki` / `文档` / `Docmost`

## 功能

- ✅ 读取页面列表
- ✅ 读取页面详情
- ✅ 创建页面
- ✅ 更新页面内容（支持 Markdown）
- ✅ 搜索页面

## 安装

```bash
npx skills add https://github.com/lemoncat7/docmost-skill --skill docmost
```

## 配置

创建配置文件 `~/.openclaw/conf/docmost/config.json`:

```json
{
  "url": "http://docmost:3000",
  "email": "你的账号邮箱",
  "password": "你的密码"
}
```

## 使用前提

Docmost 需要开启 API 访问权限（个人版可用基础 API）。
