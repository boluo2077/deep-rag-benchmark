<div align="center">

# 🏆 [Deep RAG Benchmark](https://github.com/boluo2077/deep-rag-benchmark)

[![Knowledge Base](https://img.shields.io/badge/Knowledge%20Base-GitLab%20Handbook-blue.svg)](https://gitlab.com/gitlab-com/content-sites/handbook)
[![Dataset](https://img.shields.io/badge/Dataset-250%20Questions-green.svg)](datasets)
[![License](https://img.shields.io/badge/License-Apache%202.0-orange.svg)](LICENSE)

**[ [English](README.md) | 中文 ]**

</div>

---

## 💡 为什么需要这个数据集

<table>

<tr>
<td>

### 当前痛点

- ❌ 缺乏基于企业内部知识库的真实 Benchmark
- ❌ 在多模态和深层语义问题上，主流 RAG 方案表现不佳
- ❌ 缺乏面向其他 RAG 方案的通用 Benchmark
<br>

</td>
</tr>

<tr>
<td>

### 项目亮点

- ✅ 基于开源企业知识库 **[The GitLab handbook](https://gitlab.com/gitlab-com/content-sites/handbook)** `2025/10/11 10:41` 版本
- ✅ 问题覆盖：[单文本](datasets/single_text.jsonl)、[多模态](datasets/multimodal.jsonl)、[否定排除](datasets/negation_exclusion.jsonl)、[时间指代](datasets/temporal_reference.jsonl)、[上下文指代](datasets/contextual_anaphora.jsonl)、[多跳推理](datasets/multi-hop_reasoning.jsonl)
- ✅ 包含知识库原文件，可用于测评大多数 RAG 方案；元数据丰富，易于评估调优
<br>

</td>
</tr>

</table>

> 主流 RAG 方案： 向量相似度检索 + 关键词匹配检索 + 重排序

<br>

## ⚠️ 重要声明

<table>

<tr>
<td>❗</td>
<td>

**测评脚本需自行编写**  
当前仅提供数据集，后续版本将补充

</td>
</tr>

<tr>
<td>🤖</td>
<td>

**答案仅供参考**  
数据由 AI 构造，经人工筛选，但无法保证 100% 准确合理

</td>
</tr>

</table>

<br>

## 😅 主流 RAG 方案的问题

<table>
<tr>
<td width="25%">

### 🚫 否定词失明症
```
👤 用户："不含咖啡因的饮料有哪些？"
🤖 RAG："星巴克拿铁含有150mg咖啡因..."
👤 用户："哪些部门没有提交Q3预算？"
🤖 回答："已提交的部门：市场部、研发部..."
👤 用户："😤😤😤 [掀桌子.gif]"
```
**病因**：对"不"、"没有"、"除了"视而不见

</td>
<td width="25%">

### ⏰ 时间感知障碍
```
👤 用户："今天的美元汇率？"
🤖 RAG："2023年3月汇率为6.89..."
👤 用户："上季度的营收增长率？"
🤖 RAG："2019年Q3营收增长15%..."
👤 用户："😤 你知道"今天"是哪天吗？"
```
**病因**：不知道"今天"、"昨天"、"上季度"

</td>
<td width="25%">

### 💬 上下文健忘症
```
👤 Round 1："特斯拉最新财报？"
🤖 "Q3净利润23亿美元..."
👤 Round 2："他们的自动驾驶呢？"
🤖 "自动驾驶是指车辆在没有..."
👤 用户："😤😤😤 [摔键盘.gif]"
```
**病因**：每轮对话都是"崭新的开始"

</td>
<td width="25%">

### 🔗 多跳推理障碍
```
👤 HR："张三的上级去年绩效？"
🧠 正常人的思维：张三→架构→李四→绩效→A+
🤖 RAG的思维：这里有张三简历和去年绩效！
🤖 RAG："张三...绩效...去年..."
👤 HR："😤😤😤 [血压飙升.gif]"
```
**病因**：只会单线程，无法 A→B→C 推理

</td>
</tr>
</table>

> 深入了解失败案例 **[👉🏻 Why-Do-Mainstream-RAG-Solutions-Suck-So-Hard.zh-CN.md](Why-Do-Mainstream-RAG-Solutions-Suck-So-Hard.zh-CN.md)**

<br>

## 📈 数据分布

| 🏷️ 类型 | 📁 文件 | 📊 数量 | 💡 核心挑战 |
|:---|:---:|:---:|:---|
| 📄 单文本 | [single_text.jsonl](datasets/single_text.jsonl) | **100** | 基础语义检索能力测试 |
| 🖼️ 多模态 | [multimodal.jsonl](datasets/multimodal.jsonl) | **50** | 图文混合检索能力测试 |
| 🚫 否定排除 | [negation_exclusion.jsonl](datasets/negation_exclusion.jsonl) | **25** | 否定词识别（不是、没有） |
| ⏰ 时间指代 | [temporal_reference.jsonl](datasets/temporal_reference.jsonl) | **25** | 时间推理（今天、上季度） |
| 💬 上下文指代 | [contextual_anaphora.jsonl](datasets/contextual_anaphora.jsonl) | **25** | 多轮对话指代（它、这个） |
| 🔗 多跳推理 | [multi-hop_reasoning.jsonl](datasets/multi-hop_reasoning.jsonl) | **25** | 链式推理检索（A → B → C） |

<br>

## 🗂️ 数据格式

```
{
  "id": 0,                          // 🆔 唯一标识
  "type": "类型",                    // 🏷️ 问题类型
  "context": "上个问题",             // 💬 对话上下文（仅用于 contextual_anaphora.jsonl）
  "question": "问题",                // ❓ 测试问题
  "think": "检索思路",               // 🧠 推理过程（用于理解正确检索路径）
  "retrieval": [                    // 🔍 分步检索路径
    {
      "文件路径0": ["短子串0", "短子串1"],
      "文件路径1": ["短子串2", "短子串3"]
    },
    {
      "文件路径n": ["短子串n-1", "短子串n"]
    }
  ],
  "answer": "答案"                   // ✅ 标准答案（可精确匹配验证）
}
```

<br>

## 📁 项目结构

```
📁 deep-rag-benchmark/
│
├── 📂 datasets/                         # 数据集文件夹
│   ├── single_text.jsonl               # 📄 单文本 (100条)
│   ├── multimodal.jsonl                # 🖼️ 多模态 (50条)
│   ├── negation_exclusion.jsonl        # 🚫 否定排除 (25条)
│   ├── temporal_reference.jsonl        # ⏰ 时间指代 (25条)
│   ├── contextual_anaphora.jsonl       # 💬 上下文指代 (25条)
│   └── multi-hop_reasoning.jsonl       # 🔗 多跳推理 (25条)
│
├── 📂 content/handbook/                 # 🏢 GitLab Handbook 知识库
│   ├── about/                          # 关于手册
│   ├── communication/                  # 沟通规范
│   ├── company/                        # 公司信息
│   ├── engineering/                    # 工程部
│   ├── finance/                        # 财务部
│   ├── marketing/                      # 市场部
│   ├── sales/                          # 销售部
│   └── ...                             # 更多部门文档
│
├── 📂 images/                           # 🖼️ 多模态数据图片
│   ├── finance/expenses/               # 财务相关图片
│   ├── marketing/                      # 营销相关图片
│   ├── tools-and-tips/                 # 工具使用图片
│   └── ...
│
├── 📄 README.md                         # 英文文档
├── 📄 README.zh-CN.md                   # 中文文档
├── 📄 Why-Do-Mainstream-RAG-Solutions-Suck-So-Hard.md
├── 📄 Why-Do-Mainstream-RAG-Solutions-Suck-So-Hard.zh-CN.md
└── 📄 LICENSE                           # Apache-2.0 开源协议
```

<br>

## 🎯 问题类型

---

### 📄 单文本（100 条）

<details>
<summary><b>点击展开</b></summary>

#### 📋 特点
- 测试 RAG 系统的**基础检索能力**
- 答案来源于单个文档内的连续文本

#### 💡 示例

```json
{
  "id": 0,
  "type": "Single-Text",
  "question": "What is the name of the group that consists of VPs who report directly to a member of E-Group?",
  "think": "I need to find the definition of different leadership groups within GitLab's structure. I will search the 'company/structure.md' document for terms like 'VP', 'E-Group', and 'reports directly'. The section on 'VP-Directs' provides a clear definition that matches the question.",
  "retrieval": [
    {
      "company/structure.md": [
        "VP-Directs are a subset of GitLab VPs who report directly to a member of E-Group. They are members of the VP-Directs Group."
      ]
    }
  ],
  "answer": "VP-Directs Group"
}
```

</details>

<br>

### 🖼️ 多模态（50 条）

<details>
<summary><b>点击展开</b></summary>

#### 📋 特点
- 考验 RAG 系统的多模态信息整合能力
- 答案**必须来自图片**，知识库文本中不存在

#### 💡 示例

```json
{
  "id": 0,
  "type": "Multimodal",
  "question": "What is the final item listed on the approved office supplies list?",
  "think": "The user is asking for the last item on the 'Approved Office Supplies List'. I will search the provided documents for this list. The document 'finance/expenses.md' contains a section on 'Office Supplies & consumables' and an image with the markdown `![Approved Office Supplies List](/images/finance/expenses/approved-office-supplies-list.png)`. The text in the document does not contain the full list, so I must retrieve the image to find the last item.",
  "retrieval": [
    {
      "finance/expenses.md": [
        "![Approved Office Supplies List](/images/finance/expenses/approved-office-supplies-list.png)"
      ]
    }
  ],
  "answer": "Document shredding fees"
}
```

</details>

<br>

### 🚫 否定排除（25 条）

<details open>
<summary><b>点击展开</b></summary>

#### 📋 特点
- 问题包含 **"不是"、"没有"、"除了"** 等否定词
- 存在高度相似的**干扰关键词**，易误检索

#### 💡 示例

```json
{
  "id": 0,
  "type": "Negation Exclusion",
  "question": "Within GitLab's organizational structure, which marketing-related phase focused on turning a prospect into a happy user is not a formal 'Division' or 'Department' like Sales or Engineering?",
  "think": "The question is heavily loaded with terms related to formal company structure, such as 'organizational structure', 'Division', and 'Department'. These keywords are designed to mislead the RAG system into searching `company/structure.md`. However, the question is asking for a phase in the marketing journey, which is detailed in `marketing/_index.md`. The correct answer is one of the stages of the customer journey.",
  "retrieval": [ // retrieval[0] 为错误检索！retrieval[1:] 为正确检索
    {
      "company/structure.md": [
        "Divisions: the area under one executive. e.g. the Engineering division. Departments: lead by Directors or VPs and comprise multiple teams or sub-departments e.g. the Infrastructure department within the Engineering division"
      ]
    },
    {
      "marketing/_index.md": [
        "The journey's purpose is to provide a clear and cohesive experience to a prospect/customer from the minute they become Aware of GitLab, to Considering the platform and Converting into a happy customer, and then of course Expanding over time and becoming an Evangelist."
      ]
    }
  ],
  "answer": "Converting"
}
```

</details>

<br>

### ⏰ 时间指代（25 条）

<details>
<summary><b>点击展开</b></summary>

#### 📋 特点
- 问题包含 **"今天"、"本季度"、"上个月"** 等相对时间
- 需先解析时间指代，再检索对应数据

#### 💡 示例

```json
{
  "id": 0,
  "type": "Temporal Reference",
  "question": "When did the current fiscal quarter begin?",
  "think": "The current date is November 11, 2025. First, I need to determine the current fiscal quarter by checking the definitions in the knowledge base. The file 'finance/_index.md' states that 'Q4: November 1 through January 31'. Since November 11 falls within this range, the current quarter is Q4. The start date for Q4 is November 1. The current calendar year is 2025, so the full date is 2025-11-01.",
  "retrieval": [  // 基准时间：2025年11月11日 周二 11:11:11
    {
      "finance/_index.md": [
        "Q4: November 1 through January 31"
      ]
    }
  ],
  "answer": "2025-11-01"
}
```

</details>

<br>

### 💬 上下文指代（25 条）

<details>
<summary><b>点击展开</b></summary>

#### 📋 特点
- 问题包含 **"它"、"他们"、"这个"、"那个"** 等指代词
- 需结合 `context`（上一轮对话）理解指代对象
- 问题极度简短且宽泛，单独检索必错

#### 💡 示例

```json
{
  "id": 0,
  "type": "Contextual Anaphora",
  "context": "What is the prefix for Group Channels in Slack?",
  "question": "And for the next one?",
  "think": "The user is asking about the prefix for the next channel category listed after 'Group Channels'. Without context, 'the next one' is ambiguous. A naive search might look for general categories, possibly landing in the marketing handbook which discusses departments, leading to an incorrect file. The correct approach is to locate 'Group Channels (g_)' in 'communication/chat.md', identify the following category, 'Location Channels (loc_)', and then find its prefix.",
  "retrieval": [ // retrieval[0] 为错误检索！retrieval[1:] 为正确检索
    {
      "marketing/_index.md": [
        "The GitLab Marketing team operates as one team and is organized by the following departments: Integrated Marketing, Brand and Product Marketing"
      ]
    },
    {
      "communication/chat.md": [
        "Location Channels (loc_) | These channels are used to help GitLab team-members who are in the same general region of the world to talk about get-togethers and other location-specific items."
      ]
    }
  ],
  "answer": "loc_"
}
```

</details>

<br>

### 🔗 多跳推理（25 条）

<details>
<summary><b>点击展开</b></summary>

#### 📋 特点
- 答案需要 **2步以上跨文档检索**
- `retrieval` 包含多个文件路径
- 问题中不含最终答案所在文档的关键词

#### 💡 示例

```json
{
  "id": 0,
  "type": "Multi-hop Reasoning",
  "question": "Through a multi-hop reasoning process, what is the designated output for the department managed by the executive who uses the '#ciso' communication channel?",
  "think": "First, I need to find the executive role associated with the '#ciso' channel in 'communication/chat.md'. The table 'Getting in touch with the e-group' shows this is the CISO. Second, I need to find the output of the CISO's division in the 'Organized by Output' table within 'company/structure.md'. This table lists the Security division's output.",
  "retrieval": [
    {
      "communication/chat.md": [
        "| CISO | #ciso |"
      ]
    },
    {
      "company/structure.md": [
        "| Security | Enable trust |"
      ]
    }
  ],
  "answer": "Enable trust"
}
```

</details>

---

<div align="center">

**如果这个项目对你有帮助，请点击右上角的 ⭐ Star！**

*你的 Star 是我们持续改进的动力 💪*

![Star History Chart](https://api.star-history.com/svg?repos=boluo2077/deep-rag-benchmark&type=Date)

---

<a href="#-deep-rag-benchmark">
  <img src="https://img.shields.io/badge/⬆️-回到顶部-blue?style=for-the-badge" alt="回到顶部">
</a>

</div>