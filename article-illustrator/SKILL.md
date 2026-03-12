---

name: article-illustrator description: | Generates high-quality SVG illustration assets for articles, series content, and personal IP branding. Use this skill for: WeChat article covers, knowledge cards, series overview maps, framework visualizations, quote cards, comparison charts, and flow diagrams.

Trigger immediately when the user says: 帮我做配图, 把这个可视化, 做张图, 我需要一张封面, 配图, 知识卡片, make an illustration, visualize this, create a diagram, I need a cover image, make this into a graphic.

## Output style is restrained and premium — avoids generic AI aesthetics. Delivers production-ready SVG files that can be directly published or converted to PNG via browser screenshot.

# Article Illustrator Skill

将文章内容和框架，转化为高质量、可发布的品牌配图。

## 核心理念

**好配图 = 信息密度适中 × 品牌一致性 × 视觉层级清晰**

图不是装饰，是论点的延伸。每张图必须能独立传达一个完整观点。

---

## Step 1：明确需求

```
必确认：
1. 这张图要传达的核心信息？（一句话）
2. 图片类型？（见类型库）
3. 用于什么平台？（微信/知乎/小红书/PPT）

选确认：
- 品牌色系？（用户提供，或使用默认暖橙系）
- 是否需要跨图品牌一致性？（系列配图）
- 必须出现的文字内容？
```

---

## Step 2：图片类型库

|类型|用于|布局特征|
|---|---|---|
|**封面图**|文章封面、系列第一张|大字留白 + 品牌色侧栏，远看清楚|
|**对比图**|新旧对比、好坏对比|左右或上下分栏，两侧用不同底色|
|**流程图**|步骤、机制、因果链|竖向流动（微信）/ 横向（PPT）|
|**框架图**|多维度模型、矩阵、层级|几何形状 + 颜色编码|
|**金句卡片**|系列结尾、单独传播|极简大字，大量留白|
|**知识地图**|系列规划、知识体系全景|分层结构，颜色分域|

**快速对照：** 「封面/第一张」→ 封面图 · 「对比/新旧/传统vsAI」→ 对比图 · 「步骤/流程/怎么做」→ 流程图 · 「框架/模型/体系」→ 框架图 · 「金句/这句话/结尾」→ 金句卡片 · 「全景/地图/系列规划」→ 知识地图

---

## Step 3：品牌系统

**默认品牌系统（暖橙·克制高级）：**

```javascript
// 尺寸
W_ = 750, H_ = 1050  // 微信手机竖屏最优

// 颜色
O  = '#F26419'  // 品牌橙（克制使用，只用于强调）
BK = '#1C1C1C'  // 近黑（主要文字）
G2 = '#7A7A7A'  // 中灰（次要文字）
G4 = '#E8E8E8'  // 分割线
WH = '#FAF9F6'  // 暖白背景
W  = '#FFFFFF'  // 纯白卡片

// 字体
中文：PingFang SC
英文/数字：Georgia

// 品牌识别（每张图左上角）
<rect x="40" y="40" width="36" height="4" fill="${O}"/>  // 横
<rect x="40" y="40" width="4"  height="36" fill="${O}"/> // 竖
```

**用户自定义品牌：** 询问主色 · 强调色 · 背景色 · 品牌标识元素

**系列一致性规则：**

- 同系列图：相同背景色 + 相同角标位置 + 相同布局模板
- 字体不超过2种

---

## Step 4：生成 SVG

用 Node.js 生成，输出到 `/mnt/user-data/outputs/`。

**标准代码结构：**

```javascript
const fs = require('fs');

// 品牌常量
const O  = '#F26419', BK = '#1C1C1C', G2 = '#7A7A7A';
const G4 = '#E8E8E8', WH = '#FAF9F6', W  = '#FFFFFF';
const W_ = 750, H_ = 1050;

// 辅助函数
const t = (x, y, str, size, color, anchor='start', weight='normal') =>
  `<text x="${x}" y="${y}" text-anchor="${anchor}"
   font-family="'PingFang SC',Georgia,sans-serif"
   font-size="${size}" fill="${color}" font-weight="${weight}">${str}</text>`;

const rule = (x, y, w, c=G4) =>
  `<rect x="${x}" y="${y}" width="${w}" height="1" fill="${c}"/>`;

// 品牌角标（每张图必须有）
const brandCorner = `
  <rect x="40" y="40" width="36" height="4" fill="${O}"/>
  <rect x="40" y="40" width="4"  height="36" fill="${O}"/>`;

// 图片内容
let body = `<rect width="${W_}" height="${H_}" fill="${WH}"/>
${brandCorner}
// ... 具体内容`;

const svg = `<svg width="${W_}" height="${H_}"
  viewBox="0 0 ${W_} ${H_}" xmlns="http://www.w3.org/2000/svg">
  ${body}</svg>`;

fs.writeFileSync('/mnt/user-data/outputs/文件名.svg', svg);
```

详细各类型代码模板 → 读取 `references/svg-templates.md`

---

## Step 5：视觉质量自查

```
布局
□ 主要信息在视觉重心（上1/3或正中）
□ 四边留白至少 40px
□ 文字不贴边，内边距至少 16px

文字
□ 标题 > 正文，层级差至少 6px
□ 中文每行不超过 20 字
□ 行高 = 字号 × 1.5

颜色
□ 品牌橙只用于1-2处，不泛滥
□ 深色文字在浅色背景（对比度足够）
□ 同系列颜色一致

整体
□ 这张图能独立传达核心观点吗？
□ 手机屏幕看，主要信息还清晰吗？
```

---

## 系列配图工作流

一篇文章通常需要5-6张图，按以下顺序生成，**每张确认后再生成下一张**：

```
01 封面图      → 建立视觉基调
02 核心对比图  → 传达最重要的认知转变
03 机制/流程图 → 解释为什么
04 框架图      → 给出系统性理解
05 方法图      → 告诉读者怎么做
06 金句卡片    → 最适合传播的结尾
```

---

## SVG 转 PNG 说明

告知用户：用浏览器打开 `.svg` 文件 → 右键另存为 / 截图 → 得到 PNG。

---

## 注意事项

- 内容优先于美观，图的第一目标是传达信息
- 留白是设计的一部分，不要填满所有空间
- 品牌橙要克制，力量来自稀缺
- 一次生成一张，确认后再生成下一张