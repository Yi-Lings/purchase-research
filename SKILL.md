---
name: purchase-research
description: >-
  购物研究助手。用户说"我要买XXX"、"帮我推荐XXX"、"XXX值得买吗"、"XXX好不好用"、"求推荐XXX"时触发。
  流程：小红书搜口碑 → 全网搜测评 → 电商比价 → 综合推荐（每条附理由）。
  也触发于 "purchase research"/"product review"/"shopping guide"/"price compare" 等英文表达。
  注意：只要用户表达购买/推荐/比价意图就触发，即使没明确说"小红书"或"比价"。
---

# 购物研究助手

## 前置条件

- opencli 命令加 `--window background`；先跑一条测试，不通则全走 WebSearch

## 工作流

### Step 1: 明确需求

确认三要素：**买什么、预算、特殊需求**。缺的追问。

### Step 2: 小红书搜索

OpenCLI 可用：
```
opencli xiaohongshu search "<关键词>" -f json --window background
opencli xiaohongshu note "<URL>" -f json --window background
```
优先读点赞 >200 的笔记，评论区高频品牌重点记。

兜底（OpenCLI 不通）：
```
WebSearch "site:xiaohongshu.com <品类> <品牌> 推荐 测评"
```
**必须带 `site:xiaohongshu.com` 或具体品牌名**，泛搜易出聚合站。

### Step 3: 全网搜索补充

```
# 消费评测方向（避开股票/财报用负向词 -股票 -财报 -金融）
WebSearch "<品类> 推荐 测评 红黑榜"
WebSearch "<品牌A> vs <品牌B> 对比 哪个好"
```
优先什么值得买（smzdm），用户测评质量高。

### Step 4: 电商比价

搜索优先级：smzdm 成交价 → 慢慢比比价 → 电商直接搜
```
WebSearch "smzdm <产品名> 好价"
WebSearch "慢慢买 <产品名> 价格"
WebSearch "京东 <产品名> 价格"
WebSearch "拼多多 <产品名> 百亿补贴"
```
**铁律**：核对单支/单斤价防标题党；标注历史好价 vs 当前价；多平台对比；注明是否需领券/会员。

### Step 5: 综合输出

```markdown
# 🛒 <产品名> 推荐 & 比价

## 口碑概况 | 品牌/产品 | 来源 | 口碑关键词
## 各平台价格对比 | 平台 | 规格 | 到手价 | 备注
## 最终推荐 | 你的情况 | 买什么 | 理由 | 去哪买 | 多少钱
```

**每条推荐附理由**（核心成分、价格优势、适用场景、竞品差异中至少两项）。来源受限时结果开头加 `> 注：` 说明局限性。
