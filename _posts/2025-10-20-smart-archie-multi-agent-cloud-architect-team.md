---
title: "Smart Archie：以 Multi-Agent 打造雲端架構師團隊"
date: 2025-10-20T00:00:00-00:00
categories:
  - Talk
tags:
  - 國泰金控技術年會
  - Multi-Agent
  - Cloud First
  - Smart Archie
  - 雲端架構師
  - Vibe Coding
  - Context Engineering
  - Prompt Engineering
---

> 重點聚焦於國泰金控的雲端轉型策略以及如何利用多智能體（Multi-Agent）技術解決雲端架構設計的挑戰，實現自動化交付。

{% capture notice-2 %}
#### 會議資訊

* 會議名稱：[2025 國泰金控技術年會](https://www.cathaytechcon.com.tw/2025CTC)
* 演講時間：2025-10-20 14:10
* 相關連結：[YouTube](https://www.youtube.com/watch?v=ZCZLW75RDdI) [相關報導](https://www.ithome.com.tw/news/172490)
  {% endcapture %}

<div class="notice">{{ notice-2 | markdownify }}</div>



#### 一、 雲端優先思維 (Cloud First)

演講首先介紹國泰金控的雲端化升級目標，旨在讓各子公司能以更高速度和彈性探索 AI 應用的可能性，將 **「Cloud First」** 提升為推動持續創新的企業文化。 國泰集團的七年雲端轉型計畫始於 2020 年，分為三個階段：

1. **Cloud Ready (2020)：** 啟動集團上雲計畫，確定上雲方向和遷移計畫。
2. **Cloud Adoption (2021-2025)：** 目標是五年內讓 100 套系統上雲，預計在今年 (2025) 達成此目標。
3. **Cloud First (目前階段)：** 透過雲端優先加速 IT 現代化。雲端優先的思維是 **"Cloud by default"**，即未來在規劃新系統或新業務時，預設優先考量雲端解決方案。目標包括解決技術債、實現永續發展、加速業務創新，以及導入零信任（zero trust）等現代化資安治理。

#### 二、 雲端架構設計的真實挑戰

在推動大規模上雲的過程中，國泰金控面臨雲端架構師資源稀缺的挑戰。主要有三大痛點：

1. **敏捷開發 (Agile)：** 產品迭代速度快，雲端服務更新頻繁，導致架構設計需要不斷溝通、討論與重新設計。
2. **合規審查 (Compliance)：** 作為金融業，架構設計必須符合資安、法規要求，審查流程繁瑣且需耗費人工時間來確保合規性。
3. **跨雲治理 (Governance)：** 國泰金控使用多雲環境（如 AWS、GCP、Azure），不同雲服務特性差異大，導致雲端架構標準化和治理難度增加。

#### 三、 Multi-Agent 解決方案：Smart Archie

為了解決上述挑戰，國泰金控利用生成式 AI (GenAI) 發展了 **Smart Archie** 產品，希望能透過 AI 協助實現雲端架構的標準化與自動化。 團隊從單純使用提示工程（Prompt Engineering）、導入 RAG（檢索增強生成），最終選擇了 **Multi-Agent 架構**，因為單一 Agent 存在提示詞失控、知識庫結構混亂以及 Context Window 限制等問題。Multi-Agent 策略的優勢在於能實現任務拆解、專業分工，並提高系統的穩定性。

Smart Archie 採用 **4+1 的 Multi-Agent 架構**：

- **Architect Agent (核心指揮官)：** 負責分析需求、決策、任務分配與調度。
- **DaC Agent (Diagram as Code)：** 負責自動生成雲端架構圖。
- **Solution Agent：** 依據需求場景，輸出完整的雲端服務組合、技術介紹與替代方案，並確保架構合規性。
- **TCO Agent (Total Cost of Ownership)：** 進行粗略的成本預估，提供資源優化建議並輸出成本預估報告。
- **IaC Agent (Infrastructure as Code)：** 將最終架構轉換為 **Terraform 程式碼**，方便部署到雲端環境並無縫整合至 CI/CD 流程。 該平台的基礎架構是依循 Cloud First 策略，建構在 AWS 環境上，並使用 AWS Bedrock 原生 AI 服務。

#### 四、 實作與未來藍圖

Smart Archie 的 Demo 展示了從輸入需求到自動生成可部署的**多雲架構圖**、服務方案、成本估算，到最終輸出 IaC 程式碼的**一鍵式**流程，將過去需數小時甚至數天的設計流程縮短至數分鐘，大幅提高了工作效率。

在 AI 協作開發方面，團隊鼓勵使用 **Vibe Coding** 的雙核心策略：

1. **情境工程 (Context Engineering)：** 透過提供完整的上下文、使用行業術語和提示重構，確保 AI 精準理解需求。
2. **PDCA 開發流程 (Plan, Do, Check, Act)：** 運用這個管理理論來處理 AI 程式碼的不確定性和品質問題，讓 AI 先進行規劃，再執行實作。

未來，Smart Archie 將整合進國泰的 Cloud Ready Platform 中，發展為 **AI 驅動的智能雲端治理閉環**，涵蓋四個階段：需求規劃與架構評估、成本規劃與治理審核、實作部署與自動化、持續運營與優化。最終目標是讓 Smart Archie 作為 **AI 雲端架構師**，與 Smart IaC（AI 平台工程師）、Smart Advisor 等組成 AI 輔助基礎平台，提供一站式雲端轉型服務。

整體而言，這場演講展示了國泰金控如何應對金融業在雲端轉型中遇到的專業人才與效率挑戰，並以 **Multi-Agent** 技術作為核心，打造出高度自動化、合規且敏捷的雲端架構設計平台。
