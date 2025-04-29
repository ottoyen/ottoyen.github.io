---
title: "CDK x Terraform 跨雲演出 (Multi-Cloud Container Applications with CDK for Terraform)"
date: 2020-08-12T00:00:00-00:00
categories:
  - Talk
tags:
  - CDK
  - Terraform
  - Serverless 
  - 雲端治理
  - 金融上雲
  - 多雲策略
  - 基礎設施即代碼（IaC）
---

> 企業紛紛邁入雲端轉型的浪潮中，如何有效地進行跨雲（Multi-cloud）部署已成為眾多工程師與管理者的核心議題。在這場技術分享中，以「CDK x Terraform 跨雲演出」為題，展示了如何運用CDK for Terraform (CDKTF) 來輕鬆打造跨雲的容器化應用架構。

{% capture notice-2 %}
#### 會議資訊

* 會議名稱：[Taiwan CDK Meetup](https://cdkmeetup.kktix.cc/events/fristmeetup2)
* 演講時間：2020-08-12 19:50
* 相關連結：[PDF簡報]({{ site.baseurl }}/assets/CDK x Terraform 跨雲演出.pdf)
  {% endcapture %}

<div class="notice">{{ notice-2 | markdownify }}</div>



## 緣起：從「工程師的懶惰」開始

如果你和我一樣，有過「資料庫 Schema 都設完了，還得苦寫 CRUD API」的血淚史，就能懂那句——*「寫一支 Level 3 RESTful API 要多久？3 小時、30 分鐘，還是 3 分鐘？」*
 **時間就是研發成本**，而工程師的懶惰其實是驅動「自動化」的最佳燃料。

------

## 我們想解決的痛點 vs. 爽點

- **Pain Point**：傳統專案流程冗長、跨雲環境配置複雜
- **爽點（Delight Point）** 
  1. 一鍵佈版
  2. Serverless
  3. https 預設就有
  4. 固定 URL，不用再記一長串 IP
  5. 多語言支援（TypeScript、Python、Go…想寫就寫）
  6. 免費！

> 結論？**把願望交給 CDK for Terraform（CDKTF）**——用熟悉的程式語言宣告基礎建設，想佈到 AWS、GCP 還是 Azure，甚至自家私有雲，都能一次搞定。

------

## CDKTF 的跨雲魔法

| 舊做法                   | CDKTF 作法                                       |
| ------------------------ | ------------------------------------------------ |
| 手刻 HCL、到處複製貼上   | 用程式碼（TS/ Python …）宣告，邏輯抽象化         |
| 每個雲供應商都要重學 CLI | 用同一段程式碼、切換 Provider 就能佈到不同雲     |
| 版本／權限／命名雜亂     | CDK App 統一管理 Stack，支援原生 Diff 與 Preview |

> **Demo 時刻**：現場直接把一套容器化的應用，同步佈到「多雲」環境，從打指令到服務上線不到十分鐘——自動 TLS、固定 URL，全都幫你準備好。

------

## 寫給行動派的三個重點

1. **把 IaC 變成「寫程式」這件最熟悉的事**——降低心智負擔
2. **跨雲一致性**——先寫一份 Stack，後面只是換 Provider
3. **從小專案到企業級治理**——原生支援多環境、多帳號的 Pipeline，Compliance 也能嵌進去

------

## 給金融業／大企業的一句話

當你要在合規與創新之間拉扯，**CDKTF 提供的是「同一套守則、多雲落地」的可能性**。

------

## 結語

> **懶惰是工程師的美德**——但別停在抱怨流程。用 CDKTF，把「懶」轉化成驅動創新的引擎，一鍵實現你的多雲願望！

------

