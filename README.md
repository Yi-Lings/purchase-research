# 🛒 购物研究助手 (Purchase Research Skill)

一款 AI 智能体 Skill，在聊天中自动识别购物意图，完成**小红书口碑搜索 → 全网测评收集 → 多平台比价 → 综合推荐**的全流程。

## 功能

- **需求识别**：自动检测用户的购买/推荐/比价意图，无需手动切换工具
- **小红书口碑**：搜索真实用户评价，聚合高赞笔记观点
- **全网测评**：补充什么值得买、专业评测等第三方信息
- **多平台比价**：对比京东、拼多多（百亿补贴）、慢慢买等渠道价格
- **综合推荐**：按场景/预算给出推荐，每条附成分、价格、竞品差异等理由

## 环境要求

- [Claude Code](https://claude.ai/code)（v0.2.24+）
- 可选：OpenCLI（开启后走小红书 OpenAPI，否则降级为 WebSearch 兜底）

## 安装说明

### 方式一：AI 智能体直接安装（推荐）

在 Claude Code 中直接输入：
```
请帮我装 https://github.com/Yi-Lings/purchase-research 这个 skill
```

### 方式二：手动安装

```bash
git clone https://github.com/Yi-Lings/purchase-research.git
cp purchase-research/SKILL.md /path/to/.claude/skills/purchase-research/
```

安装后重启 Claude Code 即可自动激活。

## 使用说明

触发关键词（任一说即可）：

| 中文 | 英文 |
|------|------|
| "我要买XXX" | "purchase research" |
| "帮我推荐XXX" | "product review" |
| "XXX值得买吗" | "shopping guide" |
| "XXX好不好用" | "price compare" |
| "求推荐XXX" | — |

Skill 会自动执行以下流程：
1. **前置检查** — 验证登录态，失败降级为 WebSearch
2. **明确需求** — 追问预算、使用场景等关键要素
3. **小红书搜索** — 聚合真实用户口碑（OpenCLI AI 问答 + 高赞笔记）
4. **全网测评** — 补充专业评测信息
5. **电商比价** — 比对各平台到手价
6. **综合输出** — 按场景给出推荐清单，来源受限时标注

## License

MIT
