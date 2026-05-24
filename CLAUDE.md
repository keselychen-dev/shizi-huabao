# 识字画报 (shizi-huabao)

AI 驱动的儿童识字卡片生成器，面向 3-9 岁儿童，支持主题自由创作和部编版课本同步。

在线地址：https://keselychen-dev.github.io/shizi-huabao/

## 产品定位

这是一个 Vibe Coding 产品——用户（主要是家长和老师）输入一个场景主题或选择课本课文，AI 自动生成精美的识字画报。产品核心价值是让认字变成有趣的视觉体验。

## 技术架构

**单页应用，零构建**：一个 `index.html` + 一个数据文件 `textbook_data_compact.js`，用浏览器直接打开即可运行。

- **前端**：Tailwind CSS（CDN）、原生 JavaScript、Noto Sans SC 字体
- **LLM API**：OpenAI 兼容格式，默认 DeepSeek，用于生成词汇和组词
- **图片生成**：kie.ai 平台，支持 GPT Image 2 和 Nano Banana Pro 两个模型
- **部署**：GitHub Pages（需要 CORS 代理，在设置中配置）

## 两种模式

### 主题模式
- 输入场景主题（如"超市""动物园"），可多选批量生成
- 三档难度：启蒙级（3-5岁，5-8词）、进阶级（5-7岁，10-15词）、提高级（7-9岁，15-20词）
- 流程：选主题 → LLM 生成提示词 → 图片 API 生成画报

### 课本模式（部编版）
- 覆盖小学 1-6 年级全部 12 册的课后生字
- 支持多课选择，每课独立生成
- 输出 HTML 识字卡（即时渲染，可打印/PDF）+ AI 精美图片
- 生字数据来源于 `textbook_data_compact.js`

## 文件说明

| 文件 | 用途 |
|------|------|
| `index.html` | 全部 HTML/CSS/JS，约 1350 行 |
| `textbook_data_compact.js` | 部编版 1-6 年级生字数据（含拼音），约 77KB |
| `CLAUDE.md` | 本文件，项目上下文 |

以下文件不会推到 GitHub（在 .gitignore 中排除）：
- `prd.md` — 原始提示词模板
- `api-GPT-2.md` / `api-nano banana.md` — API 文档
- `textbook_data.json` — 原始数据（compact 版本已够用）
- `小学部编版语文课本/` — PDF 课本（版权文件）
- `图片生成/` — 生成的图片缓存

## 关键代码结构（index.html）

- `getSystemPrompt(level)` — 根据难度等级动态生成系统提示词
- `generatePrompt()` — 主题模式主流程，支持多主题批量
- `generateTextbookCards()` — 课本模式 HTML 卡片渲染
- `generateAIForCards()` — 课本模式 AI 图片生成（按课文逐课生成）
- `generateImage()` — 主题模式图片生成（按主题逐个生成）
- `callLlmApi()` — OpenAI 兼容格式的 LLM 调用
- `createImageTask()` / `pollTaskStatus()` — kie.ai 异步图片生成

## API 配置

默认配置在 `DEFAULTS` 对象中，用户可在设置面板中修改，存储在 localStorage：
- LLM：DeepSeek API（`https://api.deepseek.com/v1`）
- 图片：kie.ai（`https://api.kie.ai`）
- CORS 代理：可选，GitHub Pages 部署时需要

## 本地开发

```bash
cd "/Users/kelsey/Library/Mobile Documents/com~apple~CloudDocs/@零星AI/2-网页-AI+教育-儿童识字小报生成器/"
python3 -m http.server 8080
# 打开 http://localhost:8080
```

## 设计决策备忘

- **字体**：识字卡使用楷体（KaiTi/STKaiti），适合儿童认字
- **批量生成**：多主题/多课文各自独立生成，不会合并到一张图
- **课本生字提取**：用 Python pdfplumber 从 PDF 提取，pypinyin 补拼音
- **HTML 卡片优先**：课本模式先渲染 HTML 卡片（即时可打印），用户可选择性生成 AI 图片
- **打印适配**：`@media print` 隐藏所有 UI，只保留卡片内容
