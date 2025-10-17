<div align="center">

# 🏆 [Deep RAG Benchmark](https://github.com/boluo2077/deep-rag-benchmark)

[![Knowledge Base](https://img.shields.io/badge/Knowledge%20Base-GitLab%20Handbook-blue.svg)](https://gitlab.com/gitlab-com/content-sites/handbook)
[![Dataset](https://img.shields.io/badge/Dataset-250%20Questions-green.svg)](datasets)
[![License](https://img.shields.io/badge/License-Apache%202.0-orange.svg)](LICENSE)

**[ English | [中文](README.zh-CN.md) ]**

</div>

---

## 💡 Why This Dataset

<table>

<tr>
<td>

### Current Pain Points

- ❌ Lack of real benchmarks based on enterprise internal knowledge bases
- ❌ Mainstream RAG solutions perform poorly on multimodal and deep semantic questions
- ❌ Lack of universal benchmarks for other RAG solutions
<br>

</td>
</tr>

<tr>
<td>

### Project Highlights

- ✅ Based on the open-source enterprise knowledge base **[The GitLab handbook](https://gitlab.com/gitlab-com/content-sites/handbook)** version `2025/10/11 10:41`
- ✅ Question coverage: [Single-Text](datasets/single_text.jsonl), [Multimodal](datasets/multimodal.jsonl), [Negation Exclusion](datasets/negation_exclusion.jsonl), [Temporal Reference](datasets/temporal_reference.jsonl), [Contextual Anaphora](datasets/contextual_anaphora.jsonl), [Multi-hop Reasoning](datasets/multi-hop_reasoning.jsonl)
- ✅ Includes original knowledge base files, can be used to evaluate most RAG solutions; rich metadata for easy evaluation and tuning
<br>

</td>
</tr>

</table>

> Mainstream RAG solutions: Vector similarity retrieval + Keyword matching retrieval + Reranking

<br>

## ⚠️ Important Notice

<table>

<tr>
<td>❗</td>
<td>

**Evaluation scripts need to be written by yourself**  
Currently only provides datasets, will be supplemented in future versions

</td>
</tr>

<tr>
<td>🤖</td>
<td>

**Answers are for reference only**  
Data is AI-generated and manually filtered, but 100% accuracy cannot be guaranteed

</td>
</tr>

</table>

<br>

## 😅 Problems with Mainstream RAG Solutions

<table>
<tr>
<td width="25%">

### 🚫 Negation Blindness
```
👤 User: "What beverages contain no caffeine?"
🤖 RAG: "Starbucks latte contains 150mg caffeine..."
👤 User: "Which departments haven't submitted Q3 budget?"
🤖 RAG: "Departments submitted: Marketing, R&D..."
👤 User: "😤😤😤 [table flip.gif]"
```
**Root Cause**: Ignores "not", "no", "except"

</td>
<td width="25%">

### ⏰ Temporal Awareness Disorder
```
👤 User: "Today's USD exchange rate?"
🤖 RAG: "March 2023 rate was 6.89..."
👤 User: "Last quarter's revenue growth?"
🤖 RAG: "Q3 2019 revenue grew 15%..."
👤 User: "😤 Do you know which day is 'today'?"
```
**Root Cause**: Doesn't understand "today", "yesterday", "last quarter"

</td>
<td width="25%">

### 💬 Context Amnesia
```
👤 Round 1: "Tesla's latest earnings?"
🤖 "Q3 net profit $2.3B..."
👤 Round 2: "What about their self-driving?"
🤖 "Self-driving refers to vehicles without..."
👤 User: "😤😤😤 [keyboard smash.gif]"
```
**Root Cause**: Each conversation is a "brand new start"

</td>
<td width="25%">

### 🔗 Multi-hop Reasoning Deficit
```
👤 HR: "What was Zhang San's supervisor's performance last year?"
🧠 Normal thinking: Zhang San→Architecture→Li Si→Performance→A+
🤖 RAG thinking: Here's Zhang San's resume and last year's performance!
🤖 RAG: "Zhang San...performance...last year..."
👤 HR: "😤😤😤 [blood pressure spikes.gif]"
```
**Root Cause**: Single-threaded only, can't reason A→B→C

</td>
</tr>
</table>

> Deep dive into failure cases **[👉🏻 Why-Do-Mainstream-RAG-Solutions-Suck-So-Hard.md](Why-Do-Mainstream-RAG-Solutions-Suck-So-Hard.md)**

<br>

## 📈 Data Distribution

| 🏷️ Type | 📁 File | 📊 Count | 💡 Core Challenge |
|:---|:---:|:---:|:---|
| 📄 Single-Text | [single_text.jsonl](datasets/single_text.jsonl) | **100** | Basic semantic retrieval capability test |
| 🖼️ Multimodal | [multimodal.jsonl](datasets/multimodal.jsonl) | **50** | Image-text hybrid retrieval capability test |
| 🚫 Negation Exclusion | [negation_exclusion.jsonl](datasets/negation_exclusion.jsonl) | **25** | Negation word recognition (not, no) |
| ⏰ Temporal Reference | [temporal_reference.jsonl](datasets/temporal_reference.jsonl) | **25** | Temporal reasoning (today, last quarter) |
| 💬 Contextual Anaphora | [contextual_anaphora.jsonl](datasets/contextual_anaphora.jsonl) | **25** | Multi-turn dialogue reference (it, this) |
| 🔗 Multi-hop Reasoning | [multi-hop_reasoning.jsonl](datasets/multi-hop_reasoning.jsonl) | **25** | Chain reasoning retrieval (A → B → C) |

<br>

## 🗂️ Data Format

```
{
  "id": 0,                          // 🆔 Unique identifier
  "type": "Type",                   // 🏷️ Question type
  "context": "Previous question",   // 💬 Dialogue context (only for contextual_anaphora.jsonl)
  "question": "Question",           // ❓ Test question
  "think": "Retrieval strategy",    // 🧠 Reasoning process (to understand correct retrieval path)
  "retrieval": [                    // 🔍 Step-by-step retrieval path
    {
      "File path 0": ["Substring 0", "Substring 1"],
      "File path 1": ["Substring 2", "Substring 3"]
    },
    {
      "File path n": ["Substring n-1", "Substring n"]
    }
  ],
  "answer": "Answer"                // ✅ Standard answer (for exact match verification)
}
```

<br>

## 📁 Project Structure

```
📁 deep-rag-benchmark/
│
├── 📂 datasets/                         # Dataset folder
│   ├── single_text.jsonl               # 📄 Single-Text (100 items)
│   ├── multimodal.jsonl                # 🖼️ Multimodal (50 items)
│   ├── negation_exclusion.jsonl        # 🚫 Negation Exclusion (25 items)
│   ├── temporal_reference.jsonl        # ⏰ Temporal Reference (25 items)
│   ├── contextual_anaphora.jsonl       # 💬 Contextual Anaphora (25 items)
│   └── multi-hop_reasoning.jsonl       # 🔗 Multi-hop Reasoning (25 items)
│
├── 📂 content/handbook/                 # 🏢 GitLab Handbook knowledge base
│   ├── about/                          # About handbook
│   ├── communication/                  # Communication guidelines
│   ├── company/                        # Company information
│   ├── engineering/                    # Engineering dept
│   ├── finance/                        # Finance dept
│   ├── marketing/                      # Marketing dept
│   ├── sales/                          # Sales dept
│   └── ...                             # More department docs
│
├── 📂 images/                           # 🖼️ Multimodal data images
│   ├── finance/expenses/               # Finance-related images
│   ├── marketing/                      # Marketing-related images
│   ├── tools-and-tips/                 # Tool usage images
│   └── ...
│
├── 📄 README.md                         # English documentation
├── 📄 README.zh-CN.md                   # Chinese documentation
├── 📄 Why-Do-Mainstream-RAG-Solutions-Suck-So-Hard.md
├── 📄 Why-Do-Mainstream-RAG-Solutions-Suck-So-Hard.zh-CN.md
└── 📄 LICENSE                           # Apache-2.0 open source license
```

<br>

## 🎯 Question Types

---

### 📄 Single-Text (100 items)

<details>
<summary><b>Click to expand</b></summary>

#### 📋 Characteristics
- Tests **basic retrieval capabilities** of RAG systems
- Answers come from continuous text within a single document

#### 💡 Example

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

### 🖼️ Multimodal (50 items)

<details>
<summary><b>Click to expand</b></summary>

#### 📋 Characteristics
- Tests multimodal information integration capability of RAG systems
- Answers **must come from images**, not present in knowledge base text

#### 💡 Example

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

### 🚫 Negation Exclusion (25 items)

<details open>
<summary><b>Click to expand</b></summary>

#### 📋 Characteristics
- Questions contain **"not", "no", "except"** and other negation words
- Contains highly similar **distractor keywords**, easy to mis-retrieve

#### 💡 Example

```json
{
  "id": 0,
  "type": "Negation Exclusion",
  "question": "Within GitLab's organizational structure, which marketing-related phase focused on turning a prospect into a happy user is not a formal 'Division' or 'Department' like Sales or Engineering?",
  "think": "The question is heavily loaded with terms related to formal company structure, such as 'organizational structure', 'Division', and 'Department'. These keywords are designed to mislead the RAG system into searching `company/structure.md`. However, the question is asking for a phase in the marketing journey, which is detailed in `marketing/_index.md`. The correct answer is one of the stages of the customer journey.",
  "retrieval": [ // retrieval[0] is incorrect retrieval! retrieval[1:] is correct retrieval
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

### ⏰ Temporal Reference (25 items)

<details>
<summary><b>Click to expand</b></summary>

#### 📋 Characteristics
- Questions contain **"today", "this quarter", "last month"** and other relative time expressions
- Need to parse temporal reference first, then retrieve corresponding data

#### 💡 Example

```json
{
  "id": 0,
  "type": "Temporal Reference",
  "question": "When did the current fiscal quarter begin?",
  "think": "The current date is November 11, 2025. First, I need to determine the current fiscal quarter by checking the definitions in the knowledge base. The file 'finance/_index.md' states that 'Q4: November 1 through January 31'. Since November 11 falls within this range, the current quarter is Q4. The start date for Q4 is November 1. The current calendar year is 2025, so the full date is 2025-11-01.",
  "retrieval": [  // Reference time: November 11, 2025, Tuesday 11:11:11
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

### 💬 Contextual Anaphora (25 items)

<details>
<summary><b>Click to expand</b></summary>

#### 📋 Characteristics
- Questions contain **"it", "they", "this", "that"** and other anaphoric expressions
- Need to combine with `context` (previous conversation round) to understand referent
- Questions are extremely brief and vague, retrieval without context will fail

#### 💡 Example

```json
{
  "id": 0,
  "type": "Contextual Anaphora",
  "context": "What is the prefix for Group Channels in Slack?",
  "question": "And for the next one?",
  "think": "The user is asking about the prefix for the next channel category listed after 'Group Channels'. Without context, 'the next one' is ambiguous. A naive search might look for general categories, possibly landing in the marketing handbook which discusses departments, leading to an incorrect file. The correct approach is to locate 'Group Channels (g_)' in 'communication/chat.md', identify the following category, 'Location Channels (loc_)', and then find its prefix.",
  "retrieval": [ // retrieval[0] is incorrect retrieval! retrieval[1:] is correct retrieval
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

### 🔗 Multi-hop Reasoning (25 items)

<details>
<summary><b>Click to expand</b></summary>

#### 📋 Characteristics
- Answers require **2+ steps of cross-document retrieval**
- `retrieval` contains multiple file paths
- Questions don't contain keywords from the final answer document

#### 💡 Example

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

**If this project helps you, please click the ⭐ Star in the upper right corner!**

*Your Star is our motivation for continuous improvement 💪*

![Star History Chart](https://api.star-history.com/svg?repos=boluo2077/deep-rag-benchmark&type=Date)

---

<a href="#-deep-rag-benchmark">
  <img src="https://img.shields.io/badge/⬆️-Back%20to%20Top-blue?style=for-the-badge" alt="Back to Top">
</a>

</div>