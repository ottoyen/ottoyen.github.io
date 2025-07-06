---
title: "「一周出Demo，半年用不好」──Vibe Coding 走向穩定上線的 Gap"
date: 2025-07-06T00:00:00-00:00
categories:
  - Reflection
tags:
  - Vibe Coding
---

> Vibe Coding快但常陷「能跑不能用」。解法：雙軌開發，Demo留Sandbox，正式版走平台化流程；用成熟度門檻，測試、IaC、SLO、DR逐級到位。引入Policy as Code與FinOps Guardrails提前檢核，Service Catalog與Golden Path優化DevEx。搭配成熟度評分、事故分享與成本透明，實現又快又穩，為企業帶來持續價值與競爭優勢。<div class="notice">{{ notice-2 | markdownify }}</div>

### 

| **Gap**                        | **典型情境**                            | **風險**                          |
| ------------------------------ | --------------------------------------- | --------------------------------- |
| **能跑 VS 能用**               | Demo 時順跑通，但缺測試、監控、回滾機制 | 上線後 Bug 難以定位，無法快速恢復 |
| **概念車 VS 量產車**           | POC 採硬寫死參數、手動部署              | 日後規模化全靠人力補洞，成本暴增  |
| **Prototype VS Go-Production** | 快速堆功能，欠缺安全、治理、法遵檢核    | 金融、醫療等領域易踩法規紅線      |
| **一周出Demo但半年用不好**     | Demo很快，但永遠走不到可上線的狀態      | 團隊士氣受挫，時程反而被拖長      |

------



## **解法總覽：從「秀技術」到「交付價值」**

1. **雙軌開發模型（Dual-Track）**

   - **Exploration 軌**：保留 Vibe Coding 的高速實驗優勢，限定在 *Sandbox or Feature Branch*，不連接生產資料。
   - **Delivery 軌**：以 **Platform Engineering** 提供「鋪好路 (Paved Road)」——標準化模板、CI/CD、測試框架、觀測性與安全閘門。

   > 兩條軌道以 *pull request* 或 *feature toggle* 交會，讓 demo 可逐步升級為 Production-Ready。

2. **里程碑化成熟度模型（M0-M3）**

| **等級**          | **交付物**     | **必要門檻**                                         |
| ----------------- | -------------- | ---------------------------------------------------- |
| **M0 Prototype**  | 能展示核心流程 | 隔離環境、可重製指令 (e.g., Makefile / DevContainer) |
| **M1 MVP**        | 小範圍真實用戶 | 單元 + 整合測試 ≥ 60% 覆蓋、IaC 自動部署             |
| **M2 Pilot**      | 部分正式流量   | 加密機制、SLO & Alert、Blue-Green or Canary          |
| **M3 Production** | 全量上線       | 災難復原 (DR) 演練、持續性穿測、FinOps 成本基線      |



1. **雲端治理與法遵 Shift-Left**
   - **Policy as Code**（如 OPA/Gatekeeper）在 Merge 時即檢查稽核條件。
   - **自動化 Threat Modeling & SBOM**，確保開源依賴可追溯。
   - **FinOps Guardrails**：開發階段預設低規格 + 自動停機，避免「Demo 用完忘了關」。
2. **平台化 DevEx（Developer Experience）**
   - **Service Catalog**：一鍵產生符合企業標準的微服務骨架。
   - **Golden Paths**：文件＋範例程式＋範本 GitHub Actions，降低學習曲線。
   - **內部社群 & Gemba Walk(現場走訪)**：持續收集痛點，迭代平台功能與 CLI 工具。

------



## **從 Demo 到上線：實務時間表範例**

| **週期**       | **目標**     | **關鍵活動**                                       |
| -------------- | ------------ | -------------------------------------------------- |
| **Week 1-2**   | M0 Prototype | Vibe Coding 快速驗證 → 產出 README＋錄影 Demo      |
| **Week 3-6**   | M1 MVP       | 重構為標準骨架、撰寫測試、IaC 腳本、CI/CD Pipeline |
| **Week 7-10**  | M2 Pilot     | 上跨區域低流量、加入觀測性儀表板、成本監控         |
| **Week 11-12** | Go-Live (M3) | Chaos Drill、法遵審核、變更審批、全流量切換        |

> **要訣：讓「快」只在 學習回饋循環，而不是在 跳過必要工程。**

------



## **行動清單（Checklist）**

1. 建立 **Vibe Sandbox 專案範本**，預設不可連接生產 VPC。
2. 將 **CI/CD Gate** 與 **Policy as Code** 整合；Fail Fast。
3. 推行 **Maturity Scorecard**，每週評估專案位階與缺口。
4. 成立 **Platform Guild**，定期分享真實 incident case，提升工程自覺。
5. 對管理層透明化 **Cycle Time、MTTR、單位功能成本**，證明「鋪路」投資回報。

------



### **結語**

Vibe Coding 最大的價值在於 **速度** 與 **創意激盪**；而企業最重視的卻是 **可靠交付**。透過雙軌模型、成熟度門檻、Platform Engineering 與 Shift-Left 治理，我們可以把看似相衝的兩個世界—「一周出Demo」與「穩定維運五年」—縫合在一起，真正做到 **又快又穩、既能跑又能用**。