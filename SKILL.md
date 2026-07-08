---
name: purchase-research
description: >-
  购物研究助手。用户说"我要买XXX"、"帮我推荐XXX"、"XXX值得买吗"、"XXX好不好用"、"求推荐XXX"时触发。
  流程：小红书搜口碑 → 全网搜测评 → 电商比价 → 综合推荐（每条附理由）。
  也触发于 "purchase research"/"product review"/"shopping guide"/"price compare" 等英文表达。
  注意：只要用户表达购买/推荐/比价意图就触发，即使没明确说"小红书"或"比价"。
---

# 购物研究助手

## 前置

- `opencli auth status` 确认 xiaohongshu `logged_in: true`，未登录则提示登录
- browser 命令（search / ask / note / comments）末尾加 `--window background`（非全局 flag，auth/list 等不支持）
- 命令失败走 WebSearch 兜底，**不准留空**
- **搜索源返回 0 条结果，必须在输出开头加 `> 注：` 说明来源局限性**

## Step 1: 明确需求

确认三要素：**买什么、预算、特殊需求**。缺的追问。

## Step 2: 小红书搜索

```
# 确认登录态
opencli auth status | grep xiaohongshu
# 显示 logged_in: false → 提示登录

# 搜笔记
opencli xiaohongshu search "<关键词>" -f json --window background

# AI 问答（推荐，效率最高）
opencli xiaohongshu ask "<关键词> 推荐" --window background -f json
# ask 返回 AI 总结 + 引用笔记原文节选，优于逐条读 note
```
优先读点赞 >200 的笔记，评论区高频品牌重点记。

**登录处理：**
- `logged_in: true` 但 search 仍返回 `AUTH_REQUIRED`（exitCode: 77）→ session 过期，提示重新登录
- `AUTH_REQUIRED` → 「请打开 Edge/Chrome 登录 xiaohongshu.com，然后告诉我已登录」
- 登录后重试 → 成功则继续，**失败则走 WebSearch**
- 用户拒绝登录 → 直接 WebSearch

**WebSearch 兜底：**
```
WebSearch "site:xiaohongshu.com <品类> <品牌> 推荐 测评"
```
`site:xiaohongshu.com` 常被反爬返回空，**空时必须在输出开头加 `> 注：`**

## Step 3: 全网搜索补充

```
WebSearch "<品类> 推荐 测评 红黑榜"
WebSearch "<品牌A> vs <品牌B> 对比 哪个好" -股票 -财报 -金融
```
优先什么值得买（smzdm），用户测评质量高。

## Step 4: 电商比价

搜索优先级：smzdm 成交价 → 慢慢比比价 → 电商直接搜
```
WebSearch "smzdm <产品名> 好价"
WebSearch "慢慢买 <产品名> 价格"
WebSearch "京东 <产品名> 价格"
WebSearch "拼多多 <产品名> 百亿补贴"
```
**铁律**：核对单支/单斤价防标题党；标注历史好价 vs 当前价；多平台对比；注明是否需领券/会员。

## Step 5: 综合输出

```markdown
# 🛒 <产品名> 推荐 & 比价

## 口碑概况 | 品牌/产品 | 来源 | 口碑关键词
## 各平台价格对比 | 平台 | 规格 | 到手价 | 备注
## 最终推荐 | 你的情况 | 买什么 | 理由 | 去哪买 | 多少钱
```

**每条推荐附理由**（核心成分、价格优势、适用场景、竞品差异中至少两项）。来源受限时加 `> 注：`。
