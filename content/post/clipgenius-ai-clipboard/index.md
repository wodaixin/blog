---
title: "找不到好用的Linux剪贴板，我做了个带AI的开源版本"
description: "从Windows迁移到Linux后，找不到满意的剪贴板工具。作为电商从业者，我需要频繁复制文案、图片、视频链接。于是在Google AI Studio的帮助下，我做了ClipGenius——一个支持智能分析、多模态聊天、云端同步的AI剪贴板管理器。"
slug: clipgenius-ai-clipboard
date: 2026-04-09
categories:
    - AI
    - 技术
tags:
    - 开源项目
    - 剪贴板工具
    - Linux
    - Gemini
    - React
    - Firebase
---

> 项目地址：[github.com/wodaixin/ClipGenius](https://github.com/wodaixin/ClipGenius)

## 被Windows升级"锁机"后的意外收获

2026年的某一天，Windows自动升级后，我的电脑被锁了。

不是病毒，不是硬件故障，而是微软的激活机制出了问题。折腾了几天，各种方法都试过，最后还是没能解决。那一刻我突然意识到：我对自己的电脑，竟然没有完全的控制权。

正好那段时间，我想体验一下OpenClaw、Claude等AI工具的自部署版本——这些工具在Linux上的体验更好，也更"私密"。于是我做了一个决定：彻底切换到Ubuntu。

切换的过程整体还算顺利，但有一个小问题让我很不适应：剪贴板。

在Windows上，我习惯了两个工具：
- 系统自带的`Win+V`剪贴板历史
- 开源项目[PasteEx](https://github.com/huiyadanli/PasteEx)——可以快速将剪贴板内容保存为文件

这两个工具对于我这种需要频繁处理图片、文本的电商工作来说，简直是神器。

但在Ubuntu上，我试过很多剪贴板工具：Clipman、CopyQ、Diodon……它们要么功能简陋，要么界面老旧，要么不支持云端同步。作为电商从业者，我每天需要：

- 复制大量产品文案和描述
- 收集竞品的图片素材
- 保存Facebook广告资料库的视频链接
- 在多个设备间同步这些内容

找不到一个满意的工具，让我的工作效率大打折扣。

## 意外的转机：Google AI Studio

前段时间，我开始在Google AI Studio上尝试用AI辅助工作。当时只是想让AI帮我整理一些文案，没想到Google主动推荐了一些AI功能的使用场景。

这给了我一个想法：既然找不到好用的剪贴板工具，为什么不自己做一个？而且，既然AI这么强大，为什么不让剪贴板也变得智能一点？

于是，ClipGenius就这样诞生了。

最初的版本很简单，就是一个能记录剪贴历史的网页应用。但随着开发的深入，我逐渐加入了：
- AI自动分析内容
- 多模态聊天功能
- 云端同步
- 图片生成
- 语音对话

现在回头看，它已经远远超出了我最初的设想。

## ClipGenius能做什么？

### 1. 智能内容分类

ClipGenius会自动识别你复制的内容类型，分为6种：

| 类型 | 识别逻辑 | 使用场景 |
|------|---------|---------|
| **图片** | 文件类型检测 | 产品图、设计稿、截图 |
| **视频** | 文件类型检测 | 本地视频文件 |
| **URL** | 正则匹配 | 网页链接、参考资料 |
| **Markdown** | 特征检测（#、**、```等） | 文档、笔记 |
| **代码** | 15+种语言识别 | 代码片段、配置文件 |
| **文本** | 默认类型 | 文案、备注 |

不需要手动分类，复制后自动归档。对于我这种每天要处理几十上百条内容的人来说，这个功能省了太多时间。

### 2. AI自动分析

登录后启用自动分析，AI会为每条内容生成：

- **智能文件名**：比如`img_20260409_143052`或`product_description_summary`
- **内容摘要**：一段话概括内容要点

这个功能特别适合整理素材。以前我保存一堆图片，文件名都是`IMG_1234.jpg`，根本不知道是什么。现在AI会自动生成有意义的名字，比如`nike_running_shoes_ad`，一目了然。

AI分析支持两个提供商：
- **Gemini**：支持文本、图片、视频（推荐）
- **Minimax**：仅支持文本

### 3. 多模态AI聊天

这是我最喜欢的功能之一。

你可以把任何剪贴内容附加到对话中，然后和AI聊天：

- **分析图片**："这张产品图的设计风格是什么？"
- **优化文案**："帮我把这段描述改得更吸引人"
- **解释代码**："这段代码是做什么的？"
- **翻译内容**："把这段英文翻译成中文"

AI会流式输出回答，甚至会展示"思考过程"（Gemini的thinking功能）。对话历史会自动保存，可以随时回顾。

### 4. 特色功能：FB广告视频一键下载

作为电商从业者，我经常需要研究竞品的Facebook广告素材。FB广告资料库虽然公开，但视频下载很麻烦——通常需要打开开发者工具，找到视频CDN链接，然后手动下载。

ClipGenius简化了这个流程：

1. 在FB广告资料库找到视频
2. 复制视频的CDN链接（通常是`scontent-xxx.xx.fbcdn.net`开头）
3. 粘贴到ClipGenius
4. 自动识别并下载为本地视频

下载机制很智能：
- 优先使用系统代理（如果你用Clash等工具）
- 降级到CORS代理
- 失败则保存为URL链接

**注意**：目前这个功能只支持Facebook CDN链接。未来计划集成yt-dlp等工具，支持更多视频平台。

### 5. 云端同步（可选）

**重要说明：ClipGenius无需登录即可使用，所有功能在本地都能正常工作。登录只是为了启用云端同步功能。**

如果你需要跨设备同步，可以使用Firebase：

- **实时同步**：在电脑上复制，手机上立即可见
- **本地优先**：所有数据先存到IndexedDB，即使离线也能用
- **访客模式**：不登录也能使用，数据只存本地
- **免费额度**：Firestore提供慷慨的免费额度，个人使用完全够用

**同步限制：**
- ✅ 支持：文本、URL、Markdown、代码
- ✅ 支持：图片（base64编码，单个文件<1MB）
- ❌ 不支持：视频文件（超过1MB限制）

Firestore单个文档大小限制为1MB，因此大文件（如视频）无法同步。这些文件只会保存在本地IndexedDB中。

**扩展建议：**

如果你有技术能力，可以自行扩展：
- 替换为其他云存储（如AWS S3、阿里云OSS）
- 集成更多AI提供商（Claude、OpenAI等）
- 添加文件压缩和分片上传
- 实现增量同步机制

项目代码结构清晰，修改起来不难。这也是开源的意义所在。

同步策略是"双写"：本地变更立即写入IndexedDB，同时上传到Firestore；远程变更会覆盖本地（云端优先）。这样既保证了响应速度，又避免了数据丢失。

### 6. 图片生成

基于Gemini的文字转图像功能：

- **标准模式**：使用免费的Gemini API
- **专业模式**：使用付费的AI Studio密钥

虽然我不常用这个功能，但偶尔需要快速生成一张配图时，还是挺方便的。

### 7. 语音对话

基于Gemini 3.1 Flash Live的实时语音交互。可以直接和AI语音对话，适合开车或不方便打字的场景。

## 实际使用场景

### 场景1：产品文案管理

电商工作中，我需要为不同平台准备不同版本的产品描述。以前是复制到记事本，现在直接用ClipGenius：

1. 复制多个版本的文案
2. AI自动生成摘要（比如"短版文案"、"详细描述"）
3. 需要时快速搜索和复用
4. 云端同步，手机上也能查看

### 场景2：竞品分析素材收集

研究竞品时，我会收集大量素材：

- 产品图片：自动分类为image
- 广告视频：FB链接自动下载
- 落地页链接：保存为URL
- 文案描述：保存为text

所有内容都有AI生成的摘要，后续整理时一目了然。

### 场景3：代码片段管理

虽然我不是专业程序员，但开发ClipGenius的过程中，我也积累了不少常用代码片段：

- Firebase配置代码
- React组件模板
- CSS样式片段

ClipGenius会自动识别代码语言，提供语法高亮，比传统的代码片段工具更直观。

### 场景4：跨设备工作流（需要登录）

如果你配置了Firebase并登录，可以实现跨设备同步：

- 早上在手机上看到好的广告案例，复制链接
- 到公司后，电脑上的ClipGenius已经同步了
- 点开链接，分析创意，记录笔记
- 笔记也会同步回手机，随时可以查看

**注意**：视频文件不会同步，只会保存在本地。如果你主要处理文本、图片、链接，同步功能会很方便。

## 技术实现

### 技术栈

| 类别 | 技术选择 | 原因 |
|------|---------|------|
| 前端框架 | React 19 + Vite | 最新特性，开发体验好 |
| 样式 | Tailwind CSS v4 | 快速开发，易于定制 |
| AI SDK | @google/genai | 官方SDK，功能完整 |
| 后端 | Firebase | 免费额度足够，实时同步 |
| 本地存储 | IndexedDB | 离线可用，容量大 |
| 动画 | motion/react | 流畅的交互体验 |
| 国际化 | i18next | 支持中英文切换 |

### 架构设计

ClipGenius采用"本地优先"的架构：

```
用户操作
    ↓
立即写入 IndexedDB（本地）
    ↓
异步上传到 Firestore（云端）
    ↓
其他设备实时接收更新
```

这样设计的好处：
- **响应快**：不需要等待网络请求
- **离线可用**：访客模式完全本地化
- **数据安全**：本地和云端双重备份

### 智能内容检测

内容类型检测是核心功能之一。以代码检测为例：

```typescript
function detectCodeLanguage(text: string): string | null {
  const rules: [RegExp, string][] = [
    [/^\s*\{[\s\S]*\}\s*$/, "json"],
    [/^def |^from .+ import/, "python"],
    [/^func |^package |:= /, "go"],
    [/^fn |^let mut |^impl /, "rust"],
    // ... 15+ 种语言规则
  ];
  
  for (const [regex, lang] of rules) {
    if (regex.test(text)) return lang;
  }
  return null;
}
```

通过正则表达式匹配语言特征，准确率很高。

### FB视频下载机制

视频下载的实现比较有意思：

```typescript
async function downloadFBVideo(url: string) {
  // 1. 先尝试直接fetch（会使用系统代理）
  try {
    const response = await fetch(url);
    if (response.ok) {
      return await response.blob();
    }
  } catch (err) {
    console.log("Direct fetch failed");
  }
  
  // 2. 降级到CORS代理
  const proxyUrl = `https://corsproxy.io/?${encodeURIComponent(url)}`;
  const response = await fetch(proxyUrl);
  return await response.blob();
}
```

这种"优雅降级"的策略，保证了在不同网络环境下都能工作。

## 如何开始使用

### 部署方式

ClipGenius是开源项目，需要自行部署。最简单的方式是使用Docker：

```bash
# 克隆项目
git clone https://github.com/wodaixin/ClipGenius.git
cd ClipGenius

# 配置环境变量
cp .env.example .env
# 编辑 .env，填入 Firebase 和 Gemini API 配置

# 使用 Docker Compose 启动
docker-compose up -d
```

访问 `http://localhost:8080` 即可使用。

### 配置说明

需要准备两个API密钥：

1. **Firebase配置（仅需要云同步时配置）**
   - 访问 [Firebase Console](https://console.firebase.google.com/)
   - 创建新项目
   - 启用Authentication（Google登录）和Firestore
   - 复制配置信息到`.env`
   - **注意**：Firestore免费额度对个人使用完全够用，但视频文件不会同步到云端

2. **Gemini API密钥（需要AI功能时配置）**
   - 访问 [Google AI Studio](https://aistudio.google.com/app/apikey)
   - 创建API密钥
   - 填入`.env`的`VITE_GEMINI_API_KEY`

**如果你只想本地使用，可以不配置Firebase，所有功能都能正常工作。**

详细配置步骤见项目文档。

### 为什么不提供在线服务？

有人可能会问：为什么不做成SaaS，直接提供在线服务？

原因有几个：

1. **成本考虑**：AI API调用、Firebase存储都需要成本，免费提供不现实
2. **隐私保护**：剪贴板内容可能包含敏感信息，自部署更安全
3. **开源精神**：我希望这是一个思路分享，而不是商业产品
4. **可定制性**：自部署可以根据需求修改代码，添加功能
5. **项目定位**：这是个人实验项目，不是成熟产品

如果你有技术基础，自部署其实很简单。如果没有，也可以部署到Google Cloud Run等平台，成本很低（甚至可能在免费额度内）。

## 开发过程中的收获

### 1. AI不是万能的，但很有用

最初我对AI的期望很高，以为它能自动完成所有开发工作。实际上：

- AI擅长写重复性代码（组件模板、CRUD逻辑）
- AI不擅长架构设计和复杂逻辑
- 需要人来把控方向和质量

但即便如此，AI也大大提升了开发效率。很多以前需要查文档的问题，现在直接问AI就能解决。

### 2. 开源项目的文档很重要

ClipGenius的文档写了很久，包括：
- 快速入门指南
- 功能使用说明
- 架构设计文档
- API参考手册
- 部署指南

好的文档不仅帮助用户，也帮助自己理清思路。

### 3. 模块化设计便于扩展

项目采用了清晰的模块化结构：
- AI提供商可插拔（Gemini、Minimax）
- 存储层可替换（IndexedDB、Firestore）
- 组件高度解耦

这意味着如果你想：
- 替换为其他云存储（AWS S3、阿里云OSS）
- 添加新的AI提供商（Claude、OpenAI）
- 修改UI样式和交互

都可以很方便地实现，不需要大规模重构。

### 4. 用户反馈很宝贵

虽然项目刚开源不久，但已经收到一些反馈：

- 有人建议支持更多视频平台
- 有人希望增加标签功能
- 有人想要导出功能

这些反馈让我意识到，工具的价值在于解决真实问题，而不是堆砌功能。

## 未来计划

ClipGenius还有很多可以改进的地方：

### 短期计划
- [ ] 支持更多视频平台（YouTube、抖音等）
- [ ] 增加标签和文件夹功能
- [ ] 支持批量导出
- [ ] 优化移动端体验

### 长期计划
- [ ] 集成更多AI提供商（Claude、OpenAI等）
- [ ] 支持团队协作功能
- [ ] 开发浏览器插件
- [ ] 开发桌面客户端（Electron）

如果你对这些功能感兴趣，欢迎贡献代码或提Issue。

## 写在最后

从Windows被"锁机"，到切换Ubuntu，再到开发ClipGenius，这个过程让我深刻体会到：

**有时候，限制反而是创造的起点。**

如果不是Windows升级出问题，我可能不会切换到Linux；如果不是Linux缺少好用的剪贴板工具，我可能不会开发ClipGenius；如果不是有了真实的需求，我可能不会深入学习React和AI技术。

我不是专业的前端开发者，也不是AI专家。但因为有真实的痛点，加上AI的辅助，我也能做出一个还算不错的工具。

### 关于项目的现状

需要坦诚地说：**ClipGenius还有很多bug，代码也不够完善。**

我开源这个项目，主要是想分享一个思路：如何用AI辅助开发，如何将剪贴板、AI、云同步结合起来。至于代码质量、功能完善度，还有很大的提升空间。

**关于后续维护：**
- 我可能不会持续修复公开版本的bug
- 即使修复，也可能只在我的私人版本中进行
- 如果你发现问题，欢迎提Issue，但不保证会及时响应
- 更欢迎你Fork后自己改进，这才是开源的意义

这不是一个"产品"，而是一个"实验"。如果它能给你一些启发，或者帮你解决一些问题，那就足够了。

如果你也在用Linux，也苦于找不到好用的剪贴板工具，不妨试试ClipGenius。如果你也想体验OpenClaw、Claude等AI工具的自部署版本，Ubuntu确实是个不错的选择。

如果你有任何建议或问题，欢迎在GitHub上提Issue或PR。但请理解，这只是我的一个个人项目，不是商业产品。

**开源的意义，不在于代码有多完美，而在于分享思路和可能性。**

---

**项目链接**：
- GitHub：[github.com/wodaixin/ClipGenius](https://github.com/wodaixin/ClipGenius)
- 文档：[项目文档](https://github.com/wodaixin/ClipGenius/tree/main/docs)

**技术栈**：React 19 · Vite · Firebase · Gemini · Tailwind CSS · TypeScript

**许可证**：MIT License
