---
name: docmost
description: "Use when user types /dc, docmost, wiki, 文档, Docmost, or asks to interact with Docmost wiki - reading/writing documents, creating pages, managing spaces, or any Docmost-related tasks."
---

# Docmost Wiki Skill

Docmost 是一个自托管的企业 Wiki 平台（类似 Confluence/Notion）。

## 连接信息

- **地址**: http://docmost:3000
- **API Base**: http://docmost:3000/api
- **认证方式**: Cookie (authToken) + Bearer Token

## 登录获取 Token

```python
import urllib.request
import json
import http.cookiejar

cj = http.cookiejar.CookieJar()
opener = urllib.request.build_opener(urllib.request.HTTPCookieProcessor(cj))

# Login
login_data = json.dumps({"email": "账号", "password": "密码"}).encode('utf-8')
req = urllib.request.Request(
    "http://docmost:3000/api/auth/login",
    data=login_data,
    headers={"Content-Type": "application/json"}
)
resp = opener.open(req, timeout=10)

token = None
for c in cj:
    if c.name == 'authToken':
        token = c.value
        break
```

## API 端点

### 获取页面列表
```python
# POST /api/pages/recent - 获取最近页面
req2 = urllib.request.Request(
    "http://docmost:3000/api/pages/recent",
    data=b"{}",
    headers={
        "Content-Type": "application/json",
        "Authorization": f"Bearer {token}"
    },
    method="POST"
)
resp2 = opener.open(req2, timeout=10)
pages = json.loads(resp2.read().decode())
```

### 获取页面详情
```python
# POST /api/pages/info - 获取页面内容
PAGE_ID = "页面ID"
SPACE_ID = "空间ID"

req2 = urllib.request.Request(
    f"http://docmost:3000/api/pages/info",
    data=json.dumps({"pageId": PAGE_ID, "spaceId": SPACE_ID}).encode('utf-8'),
    headers={
        "Content-Type": "application/json",
        "Authorization": f"Bearer {token}"
    },
    method="POST"
)
resp2 = opener.open(req2, timeout=10)
page_data = json.loads(resp2.read().decode())
```

### 创建页面
```python
# POST /api/pages/create
SPACE_ID = "空间ID"

create_data = {
    "spaceId": SPACE_ID,
    "title": "页面标题",
    "parentPageId": None  # 或父页面ID
}

req2 = urllib.request.Request(
    "http://docmost:3000/api/pages/create",
    data=json.dumps(create_data).encode('utf-8'),
    headers={
        "Content-Type": "application/json",
        "Authorization": f"Bearer {token}"
    },
    method="POST"
)
resp2 = opener.open(req2, timeout=10)
new_page = json.loads(resp2.read().decode())
```

### 更新页面内容
```python
# POST /api/pages/update
PAGE_ID = "页面ID"
SPACE_ID = "空间ID"

content = """# 标题

这是页面内容，支持 Markdown 格式。
"""

update_data = {
    "pageId": PAGE_ID,
    "spaceId": SPACE_ID,
    "content": content,
    "operation": "replace",  # append, prepend, replace
    "format": "markdown"  # json, markdown, html
}

req2 = urllib.request.Request(
    "http://docmost:3000/api/pages/update",
    data=json.dumps(update_data).encode('utf-8'),
    headers={
        "Content-Type": "application/json",
        "Authorization": f"Bearer {token}"
    },
    method="POST"
)
resp2 = opener.open(req2, timeout=10)
```

## 已知 Spaces 和 Pages

- **M-General** (spaceId: `0198954f-5807-72e1-81a5-2b98d6532f4c`, slug: `mgeneral`)
  - TODO (pageId: `01989822-098f-7e2d-8b97-3cdf941562a4`)
  
- **N-General** (spaceId: `019c5093-9507-7eb4-8f96-1574b7086156`, slug: `ngeneral`)
  - 莫殇测试页面 - 2026-03-20 (pageId: `019d0baa-cc7c-78b8-b651-85623f8bc9e0`)

## 权限说明

- 普通用户可以在部分 Space（如 N-General）创建/编辑页面
- M-General 可能只有只读权限

## 当前账号

- 账号: 1625669496@qq.com
- 配置文件: `~/.openclaw/conf/docmost/config.json`
