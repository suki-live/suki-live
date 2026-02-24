# 🎤 suki.live

> 为虚拟主播打造的轻量歌单主页 · 自由注册 · 永久托管 · 用记忆与歌声，连接每一个你

[✨ 访问首页](https://suki.live) · [📋 主播名录](https://suki.live/registry.html) · [📬 匿名信指引](https://dreamail.cn/)

---

## 🌸 项目简介

**suki.live** 是一个基于 GitHub Pages + Cloudflare Workers 的静态托管平台，专为虚拟主播/个人创作者提供：

- 🎵 **歌单展示**：支持网易云 / B 站 / YouTube 等多平台链接
- 📝 **Markdown 自我介绍**：简洁排版，专注内容
- 🖼️ **头像自动加载**：只需放置 `avatar.png`，无需配置
- ✉️ **匿名信支持**：集成 dreamail.cn，保护双方隐私
- 📋 **统一名录页**：一键浏览所有驻留主播
- 🌐 **子域名访问**：`your-id.suki.live` 专属主页
- 🚀 **零成本托管**：GitHub Pages + Cloudflare 免费额度

> 💡 适合：虚拟主播、独立音乐人、内容创作者、想拥有个人主页的你

---

## ✨ 功能特性

| 特性 | 说明 |
|------|------|
| 🔗 子域名路由 | `example.suki.live` → `/u/example/`，自动重写 |
| 🖼️ 头像自动加载 | 固定使用 `/u/{id}/avatar.png`，无需 JSON 配置 |
| 📦 配置解耦 | `info.json`（个人信息）+ `playlist.json`（歌单）独立维护 |
| 🎛️ 灵活控制 | `show_playlist: false` 可隐藏歌单，专注个人介绍 |
| 🔗 社交链接数组 | `social: [{label, url}]` 格式，自由添加任意平台 |
| ✉️ 匿名信集成 | 可选 `anonymail` 字段，一键跳转 dreamail.cn |
| 📱 响应式设计 | 移动端自动适配，卡片式布局 |
| 🎨 清新淡色风格 | CSS 变量统一管理，视觉轻盈温柔 |
| 🔒 安全加载 | 相对路径 + XSS 转义，避免路径冲突与注入风险 |
| 🤝 PR 注册制 | Fork → 修改 → 提交 PR → 审核合并，流程透明 |

---

## 📁 文件结构

```
suki-live/
├── index.html              # 🏠 首页（注册引导 + 功能介绍）
├── registry.html           # 📋 主播名录页（搜索 + 统计 + 卡片展示）
├── CNAME                   # GitHub Pages 自定义域名：suki.live
├── README.md               # 本文档
│
└── u/                      # 👈 子域名内容根目录
    ├── registry.json       # 📦 所有主播注册信息汇总
    └── example/            # 🎤 示例主播（注册时请复制此目录）
        ├── index.html      # 主播个人页模板（无需修改）
        ├── info.json       # 个人信息配置
        ├── playlist.json   # 歌单数据配置
        ├── bio.md          # 自我介绍（Markdown，可选）
        └── avatar.png      # 👈 头像（固定命名，自动加载）
```

> 🔹 新主播注册时，只需复制 `/u/example/` → `/u/你的ID/`，修改配置即可

---

## 🚀 快速注册指南

### 第 1 步：Fork 仓库
[🍴 点击 Fork suki-live/suki-live](https://github.com/suki-live/suki-live/fork)

### 第 2 步：创建你的目录
在你的 Fork 仓库中：
```
/u/
└── your-id/          # 👈 你的子域名标识（如 miku / starchild）
    ├── index.html    # 直接复制 example/index.html，无需修改
    ├── info.json     # 填写你的个人信息（见下方模板）
    ├── playlist.json # 填写歌单（可为空数组）
    ├── bio.md        # 自我介绍（可选，Markdown 格式）
    └── avatar.png    # 👈 头像（200x200px 以上，圆形裁剪更佳）
```

> ⚠️ **重要规范**：
> - `your-id` 将作为你的子域名：`https://your-id.suki.live`
> - 头像**必须**命名为 `avatar.png`，放在同目录下
> - 所有资源路径使用**相对路径**（如 `./info.json`），勿用绝对路径

### 第 3 步：配置 info.json

```json
{
  "name": "你的昵称",
  "tagline": "一句温柔的签名 🌸",
  "bio": true,
  "show_playlist": true,
  "anonymail": "https://dreamail.cn/send?dm=xxx",
  "social": [
    { "label": "B站", "url": "https://space.bilibili.com/xxx" },
    { "label": "Twitter", "url": "https://twitter.com/xxx" },
    { "label": "个人博客", "url": "https://example.com" }
  ],
  "created_at": "2026-01-01",
  "updated_at": "2026-02-24"
}
```

#### 🔹 字段说明

| 字段 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| `name` | string | ✅ | - | 主播昵称，显示在页面标题 |
| `tagline` | string | ❌ | - | 个性签名，显示在头像下方 |
| `bio` | boolean | ❌ | `false` | 是否加载 `bio.md` 作为自我介绍 |
| `show_playlist` | boolean | ❌ | `true` | 是否显示歌单卡片，`false` 则隐藏 |
| `anonymail` | string | ❌ | - | dreamail.cn 匿名信链接，不填则不显示按钮 |
| `social` | array | ❌ | `[]` | 社交链接数组，格式 `[{"label":"", "url":""}]` |
| `created_at` | string | ❌ | - | 注册日期（ISO 8601），用于名录排序 |
| `updated_at` | string | ❌ | - | 最后更新日期，便于追踪变更 |

### 第 4 步：配置 playlist.json

```json
{
  "title": "我的精选歌单",
  "description": "深夜治愈 · 温柔入梦 · 不定期更新",
  "items": [
    {
      "title": "夜に駆ける",
      "artist": "YOASOBI",
      "source": "网易云",
      "url": "https://music.163.com/#/song?id=123456789",
      "cover": "https://p2.music.126.net/example.jpg",
      "added_at": "2026-02-01"
    }
  ]
}
```

#### 🔹 字段说明

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `title` | string | ❌ | 歌单标题，默认「推荐歌单」 |
| `description` | string | ❌ | 歌单简介，显示在标题下方 |
| `items` | array | ✅ | 歌曲列表，可为空数组 `[]` |

##### 🔸 歌曲项字段

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `title` | string | ✅ | 歌曲名称 |
| `artist` | string | ❌ | 演唱者/创作者 |
| `source` | string | ❌ | 来源平台（网易云 / B 站 / YouTube 等） |
| `url` | string | ❌ | 播放链接，不填则不显示「播放」按钮 |
| `cover` | string | ❌ | 歌曲封面图 URL，支持外链 |
| `added_at` | string | ❌ | 添加日期，用于排序或展示「新歌」标识 |

### 第 5 步：（可选）编写 bio.md

如果 `info.json` 中 `"bio": true`，系统将自动加载同目录 `bio.md` 作为自我介绍。

支持简易 Markdown 语法：
```markdown
### 👋 你好呀～

我是 **Example**，一个喜欢用歌声讲述故事的虚拟主播✨

🎵 常驻曲风：J-POP / 治愈系 / 电子  
🎮 偶尔直播：《原神》《Arcaea》《星露谷物语》  
💬 口头禅：「今天也要好好唱歌哦～」

> 愿我的歌声，能像晚风一样，轻轻拂过你的心头🌙

🔗 [B 站主页](https://bilibili.com) · [网易云电台](https://music.163.com)
```

> ✅ 支持：标题（`#` / `##` / `###`）、**加粗**、*斜体*、[链接](url)、段落换行

### 第 6 步：更新 registry.json

编辑 `/u/registry.json`，在 `vtubers` 数组中添加你的信息：

```json
{
  "vtubers": [
    // ... 现有主播
    {
      "id": "your-id",
      "name": "你的昵称",
      "tagline": "你的签名",
      "registered_at": "2026-02-24",
      "status": "active"
    }
  ]
}
```

#### 🔹 字段说明

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `id` | string | ✅ | 子域名标识，必须与目录名一致 |
| `name` | string | ✅ | 主播昵称，用于名录卡片展示 |
| `tagline` | string | ❌ | 个性签名，显示在名录卡片 |
| `registered_at` | string | ❌ | 注册日期，用于排序 |
| `status` | string | ✅ | 状态：`active`（正常）/ `inactive`（暂停）/ `banned`（封禁） |

> ⚠️ **注意**：`registry.json` 中**无需**配置 `avatar`，系统会自动加载 `/u/{id}/avatar.png`

### 第 7 步：提交 Pull Request

1. Commit 你的修改，标题格式：`[Register] your-id`
2. Push 到你的 Fork 仓库
3. 前往 [suki-live/suki-live](https://github.com/suki-live/suki-live) 创建 Pull Request
4. 等待管理员审核（通常 24-48 小时内）
5. 合并后访问 `https://your-id.suki.live` ✨

---

## 🧪 验证你的主页

合并后，按顺序测试：

| 测试项 | 访问地址 | 预期结果 |
|--------|----------|----------|
| ✅ 子域名主页 | `https://your-id.suki.live/` | 显示你的个人页，头像/签名/社交链接正常 |
| ✅ 歌单展示 | （如配置了歌单） | 显示歌单卡片，歌曲可点击跳转 |
| ✅ 匿名信按钮 | （如配置了 anonymail） | 显示「✉️ 匿名信」按钮，点击跳转 dreamail |
| ✅ 返回首页 | 点击页脚「返回首页」 | 跳转至 `https://suki.live` |
| ✅ 名录页展示 | `https://suki.live/registry.html` | 你的卡片出现在名录中，头像正常加载 |
| ✅ 移动端适配 | 手机浏览器访问 | 布局自动适配，无横向滚动 |

---

## ❓ 常见问题

### Q：为什么访问 `your-id.suki.live` 显示 404？
A：请检查：
1. PR 是否已被合并（未合并则未部署）
2. `/u/your-id/index.html` 是否存在
3. `info.json` / `playlist.json` 是否为有效 JSON
4. 清除浏览器缓存或尝试无痕模式

### Q：头像不显示 / 显示默认灰色图？
A：请确认：
1. 头像文件命名为 `avatar.png`（大小写敏感）
2. 文件位于 `/u/your-id/avatar.png`（与 index.html 同目录）
3. 图片格式为 PNG，尺寸建议 ≥200x200px
4. 提交时图片是否成功上传（Git LFS 用户注意）

### Q：如何隐藏歌单？
A：在 `info.json` 中添加：
```json
{
  "show_playlist": false
}
```
→ 页面将不渲染歌单卡片，即使 `playlist.json` 有数据

### Q：social 链接可以加微信/QQ 吗？
A：可以！`social` 数组支持任意平台：
```json
"social": [
  { "label": "微信", "url": "weixin://dl/chat?xxx" },
  { "label": "QQ 群", "url": "https://jq.qq.com/?_wv=1027&k=xxx" },
  { "label": "粉丝群", "url": "https://t.me/xxx" }
]
```
> ⚠️ 注意：部分协议链接（如 `weixin://`）仅在移动端 App 中生效

### Q：bio.md 支持哪些 Markdown 语法？
A：当前支持：
- 标题：`#` / `##` / `###`
- 强调：`**加粗**` / `*斜体*`
- 链接：`[文字](URL)`
- 段落：空行分隔 / 单换行转 `<br/>`

> 🚧 未来可能扩展：列表、代码块、图片（需评估安全风险）

### Q：可以自定义页面样式吗？
A：当前模板为统一风格，确保平台视觉一致性。  
如需深度定制，建议：
1. Fork 后修改 `/u/your-id/index.html` 的 `<style>`
2. 或联系管理员讨论主题扩展方案

### Q：内容违规会被处理吗？
A：是的。注册即表示同意：
- ❌ 禁止色情/暴力/违法内容
- ❌ 禁止商业推广/引流链接
- ❌ 禁止冒充他人/侵犯权益
- ✅ 鼓励原创、温柔、治愈向内容

违规内容将被下架，严重者封禁子域名。

---

## 🤝 贡献指南

欢迎通过以下方式参与项目：

### 🔧 技术贡献
- 🐛 提交 Issue：反馈 Bug 或建议新功能
- 💡 提交 PR：修复问题 / 优化体验 / 补充文档
- 🎨 设计贡献：提供 UI 优化方案或暗色模式设计

### 📝 内容贡献
- ✍️ 完善文档：补充注册指南 / 常见问题
- 🌍 多语言支持：翻译 README 为其他语言
- 🎁 模板扩展：提供新风格模板（需管理员审核）

### 🌱 社区贡献
- 💬 帮助新人：在 Issue/PR 中解答注册问题
- 📢 宣传推广：在社群分享 suki.live
- ✨ 创作示例：用你的主页展示最佳实践

#### PR 提交规范
```bash
# 标题格式
[Type] 简短描述

# 示例
[Register] add user: starchild
[Fix] resolve avatar path in registry.html
[Docs] update README with show_playlist example

# Type 可选值
Register | Fix | Feature | Docs | Style | Refactor
```

---

## 🔐 隐私与安全

- 🔒 所有页面为静态 HTML，无用户数据收集
- 🔗 外部链接使用 `rel="noopener"` 防止跳转风险
- 🛡️ 用户输入经 `escapeHtml()` 转义，防 XSS 注入
- 🌐 Cloudflare Proxy 提供基础 DDoS 防护
- 📦 媒体资源建议外链（B 站/网易云等），避免仓库过大

> 💡 匿名信通过 dreamail.cn 中转，suki.live 不存储任何信件内容

---

## 📜 许可证

本项目采用 [MIT License](LICENSE) 开源：

```
Copyright © 2026 suki.live

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
```

> ✨ 你可以自由使用、修改、分发，只需保留原作者声明

---

## 💙 致谢

- [GitHub Pages](https://pages.github.com/) · 免费静态托管
- [Cloudflare Workers](https://workers.cloudflare.com/) · 边缘路由 + SSL
- [dreamail.cn](https://dreamail.cn/) · 匿名信服务支持
- 每一位驻留的主播 · 用歌声与故事，让 suki.live 有了温度

---

## 🌙 最后的话

> 「愿每一个温柔的灵魂，都能被世界轻轻接住」  
> 如果你在这里找到了共鸣，欢迎留下你的故事～  
> 如果暂时还没准备好，也没关系，我们一直在。

✨ **suki.live · 用记忆为碑，以歌声为光**

[🏠 返回首页](https://suki.live) · [📋 主播名录](https://suki.live/registry.html) · [✉️ 匿名信指引](https://dreamail.cn/)

---

> 📮 项目维护：[suki-live/suki-live](https://github.com/suki-live/suki-live)  
> 🌸 最后更新：2026-02-24