---
title: "Smart Archie 雲端架構圖智能生成助手"
date: 2024-10-15T00:00:00-00:00
categories:
  - Speech
tags:
  - 雲端架構
  - Diagram-as-Code
  - 生成式AI
  - RAG
  - Chain of Thought
  - Auto-Refinement
  - Cathay 6R
---

> 在國泰金控推動大規模上雲的這幾年，我最大的感受是：架構師永遠不夠用。
> 當各子公司同時啟動 Cloud Ready 專案，動輒上百張雲端架構圖要在短時間內產出，還得符合內部 Cathay 6R 方法論與金融業合規條款──這幾乎是不可能的任務。於是，我們動手打造了 Smart Archie，一個結合 LLM 與 Diagram-as-Code 的「雲端架構生成助手」。

{% capture notice-2 %}
#### 會議資訊

* 會議名稱：[2024 AWS 生成式 AI 創新產業應用日](https://aws.amazon.com/tw/events/2024-genai-day/)
* 演講時間：2024-10-15 13:00
* 相關連結：[PDF簡報]({{ site.baseurl }}/assets/Smart Archie 雲端架構生成助手_v4.pdf) [相關報導](https://aws.amazon.com/tw/events/taiwan/interviews/cathay_financial_holdings/)
  {% endcapture %}

<div class="notice">{{ notice-2 | markdownify }}</div>



### 1. 為什麼需要 Smart Archie？

1. **產能失衡**：上雲專案暴增，但資深架構師有限。
2. **格式混亂**：每位架構師偏好的繪圖工具與圖例不同，導致跨部門溝通成本高。
3. **缺乏沿革**：許多專案只保留最終架構圖，需求演進與修訂過程難以追溯。

> **關鍵思考**
>  與其持續「人海戰術」畫圖，不如把「畫圖」本身視為可以被自動化的程式碼生成流程。

------

### 2. 技術選型：Diagram-as-Code 的二分法



| Tool                  | 優點                                               | 缺點                              |
| --------------------- | -------------------------------------------------- | --------------------------------- |
| **Diagrams (Python)** | 內建 AWS 圖示多、程式碼易被 LLM 產生、版本控制友善 | 學習曲線陡、通用性較低            |
| **PlantUML**          | 通用領域廣、語法簡潔                               | 需另外引入 AWS 圖庫、複雜圖易失真 |

我們最終採 **雙軌並行**：以 Diagrams 為主要產線，PlantUML 作為備援。

------

### 3. Prompt Engineering：從 0% 到 100% 編譯率

1. **Naïve Prompt** → 0 % 可編譯
2. 加入 **Chain-of-Thought** → 還是 0 %
3. 加入 **RAG（Icon 檢索）** → 8 %
4. **Few-Shot** 範例 → 83 %
5. Few-Shot + RAG → **100 %**
6. 再疊上 **Auto-Refinement** → 需求符合度 98 %，平均 2 分鐘內完成微調。

> **心得**
> CoT 有助於 LLM 思考流程，但 **範例（Few-Shot）與即時知識檢索（RAG）** 才是提高穩定度的關鍵；Auto-Refinement 則讓模型學會「自我補救」，大幅降低人工回補。

------

### 4. Smart Archie 的三大核心能力

1. **架構圖初版生成**
   - 讀取上雲問卷與政策，30 秒內輸出符合公司守則的雲端架構圖（Diagrams/PlantUML）。
2. **自然語言微調**
   - 像對同事講話一樣：「把資料庫換成 Amazon RDS PostgreSQL 高可用版」，即可自動修版並記錄版本差異。
3. **版本沿革追蹤**
   - 每一次修訂都被 Git 追蹤，方便審計與跨組織溝通。

------

### 5. 成效與展望

- **時間成本**：一張典型三層式架構圖，從過去人工 3 ~ 4 小時，縮減至 **< 2 分鐘**。
- **品質一致性**：所有圖例符合內部政策，不再需要人工比對。
- **未來整合**：Smart Archie 已透過 **Cloud Ready Platform** 與「知識解惑」「IaC 生成」等模組串聯，形成一站式雲治理工作流。

------

### 6. 結語

Smart Archie 的誕生證明了：**當我們用「程式碼」思維重塑「繪圖」流程，LLM 就能成為雲端架構師的最佳拍檔**。在 AI 與雲端浪潮交會的此刻，唯有不斷解構問題、迭代實驗，才能讓工具真正落地、放大人類專業的槓桿。
