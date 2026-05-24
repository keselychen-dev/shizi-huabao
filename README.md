# 识字画报

AI 驱动的儿童识字卡片生成器，让认字变成有趣的视觉体验。

## 功能特点

### 主题模式
输入任意场景主题（超市、动物园、海洋…），AI 自动生成对应的精美识字画报。
- 支持**多主题批量生成**，一键出多张
- 三档难度分级：
  - 启蒙级（3-5岁）：5-8个词，超大字号，带互动游戏
  - 进阶级（5-7岁）：10-15个词，含量词组词
  - 提高级（7-9岁）：15-20个词，完整版式

### 课本模式（部编版）
同步小学语文教材，覆盖 1-6 年级全部 12 册课后生字。
- 支持多课选择，每课独立生成识字卡
- **HTML 识字卡**：即时渲染，可直接打印或保存为 PDF
- **AI 精美版**：调用图片模型生成绘本风格画报

### 双模型支持
- **GPT Image 2**：画质高，细节丰富
- **Nano Banana Pro**：速度快，适合快速预览

## 在线使用

👉 [https://keselychen-dev.github.io/shizi-huabao/](https://keselychen-dev.github.io/shizi-huabao/)

> 首次使用需在「设置」中配置 API Key。部署在 GitHub Pages 时需填写 CORS 代理地址。

## 本地运行

```bash
git clone https://github.com/keselychen-dev/shizi-huabao.git
cd shizi-huabao
python3 -m http.server 8080
```

浏览器打开 `http://localhost:8080` 即可使用。

## 需要的 API Key

| 用途 | 获取地址 |
|------|----------|
| LLM（默认 DeepSeek） | [platform.deepseek.com](https://platform.deepseek.com/) |
| 图片生成（kie.ai） | [kie.ai/api-key](https://kie.ai/api-key) |

在页面右上角「设置」中填入即可，支持多种 LLM 提供商（DeepSeek、通义千问、智谱 GLM、Moonshot、OpenAI）。

## 技术栈

- 纯前端单页应用，零构建、零依赖
- Tailwind CSS（CDN）
- OpenAI 兼容格式 LLM API
- kie.ai 异步图片生成 API

## License

MIT
