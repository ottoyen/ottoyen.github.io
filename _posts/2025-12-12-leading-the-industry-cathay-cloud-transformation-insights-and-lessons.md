---
title: "走在同業前面：國泰的雲端轉型洞察與啟示"
date: 2025-12-12T11:00:00+08:00
categories:
  - Talk
tags:
  - 雲端轉型
  - Cloud Ready
  - Cloud First
  - CCoE
  - FinOps
  - Smart Archie
  - Multi-Agent
  - WebConf
header:
  teaser: /assets/images/cathay-cloud-transformation-hero.webp
---

> 金融業上雲，真正困難的從來不只是選擇哪一朵雲，而是如何在監理、安全、組織與既有系統之間，讓一次成功變成可重複、可治理的能力。這場演講回顧國泰從 Cloud Ready、Cloud Adoption 走向 Cloud First 的七年旅程，也分享我一路上最深的體會：雲端轉型其實是一場信心工程。

{% capture notice-2 %}
#### 會議資訊

* 會議名稱：[WebConf Taiwan 2025](https://webconf.tw/)
* 演講時間：2025-12-12 11:00–11:45
* 演講地點：F 棟
* 相關連結：[🗓️官方議程](https://webconf.tw/speakers/13)   [📕PDF簡報]({{ site.baseurl }}/assets/走在同業前面_國泰的雲端轉型洞察與啟示_v3.pdf)   [🎵AI Podcast]({{ site.baseurl }}/assets/走在同業前面_國泰的雲端轉型洞察與啟示.m4a)
  {% endcapture %}

<div class="notice">{{ notice-2 | markdownify }}</div>

![一條連續路徑串起安全地基、規模化治理與 AI 就緒的雲端未來]({{ site.baseurl }}/assets/images/cathay-cloud-transformation-hero.webp){: .align-center}
*我們不是把系統搬到雲上就結束，而是一路從「能上雲」走到「預設用雲」。*

## 1. 敢說走在同業前面，先回答「為什麼要上雲」

這是我第一次在 WebConf 談雲端。前一場講者才提到，當年大約六成議程都和 AI 有關；我的題目卻完全沒有 AI，反而顯得稀有。

不過既然題目敢寫「走在同業前面」，我就得先交代憑什麼。從 2020 年啟動集團七年雲端轉型計畫，到公開宣布完成百套系統上雲，外部已經留下不少紀錄；我甚至搜尋到有人只靠公開資料，把國泰上雲寫成一篇碩士論文，卻沒有訪問任何我認識的人。演講當下，我看到的最新內部數字是 123 套。國泰對外資料則以「[超過百套系統上雲，正式從 Cloud Ready 進入 Cloud First](https://www.cathayholdings.com/holdings/lastest_news/news_archive/newsarticle?newsID=8-0C1qzaP0aGC2qHopqJCg)」描述這個里程碑。

但數量不是我最想談的重點。金融業上雲，最常被問的是「會不會比較省錢？」我的答案一直是：**不一定。架構沒有設計好，上雲一樣很貴。**

真正值得換取的是速度、彈性與可靠性。信用卡活動或大型售票帶來的瞬間流量，很難靠地端設備為少數尖峰長期備妥；金融服務又不能隨便停機。雲端的彈性伸縮與託管服務，讓我們能用不同方式處理這兩個問題。上雲不是為了追流行，而是為了替金融科技建立更能快速反應的體質。

## 2. Cloud Ready：先把抽象的風險說成人話

2019 年，主管機關為金融業上雲打開一扇小門；國泰從 2020 年進入 Cloud Ready。那時不能突然選一個系統就搬上去，我們得先準備網路與 Landing Zone、管理治理、組織人才，以及雲原生應用的開發方式。

最常遇到的問題是：「把資料放到雲端服務商的機房，他們半夜偷看怎麼辦？」

標準答案是責任共享模型，但光講 IaaS、PaaS、SaaS，法遵、資安與風控同仁未必立刻有感。我後來改用辦公室租賃來比喻：國泰把樓層租給其他公司，不代表房東可以半夜破門翻資料；承租人也不會把機密攤在桌上，而會鎖櫃、加密並管理鑰匙。雲端同樣如此，服務商與使用者各有責任，資料所有權與保護措施也不能因為「租用」就消失。現行規範同樣要求金融機構保有資料所有權、控管儲存地，並採取加密與金鑰管理措施，可參考[金管會雲端委外規範](https://law.fsc.gov.tw/LawContent.aspx?id=FL040528&media=print)。

![辦公大樓由業者維護基礎設施，租戶仍掌握自己上鎖的資料與工作空間]({{ site.baseurl }}/assets/images/cathay-cloud-transformation-shared-responsibility.webp){: .align-center}
*責任共享不是把風險全部交給雲端服務商，而是把每一層該由誰保護說清楚。*

金融業的另一個現實，是技術語言一旦進入規範，差一個詞就可能多出大量溝通成本。我曾在政府文件裡看到 SaaS 被翻成「雲端化服務」，還分成「套裝型」。我當場很困惑：如果有雲端化服務，是不是也會有地端化服務？我們花了不少力氣釐清 IaaS、PaaS、SaaS 的邊界。這類事情看起來像文字問題，實際上會直接影響審查、架構與採用方式。

因此 Cloud Ready 不是單一技術專案，而是五條軌道同時推進：

1. 進行雲端安全評估與差異分析。
2. 導入 ISO 27017、CSA CCM 等雲端安全框架。
3. 透過培訓、Workshop、Onsite 輔導與 Office Hour 建立種子團隊。
4. 以 CCMA 評估系統，搭配 Cathay 6R 決定 Rehost、Re-platform、Refactor、Rewrite、Replace 或 Retain。
5. 建立 MVC 與 Landing Zone，把 VPC、IAM、CI/CD、IaC、安全機制與託管服務真正跑起來。

回頭看，這一階段最重要的產物不是一張架構圖，而是信心：讓資安知道風險如何被控制，讓開發知道雲端不是摸不到的黑盒子，也讓主管知道上雲有方法、有順序、有退路。

## 3. Cloud Adoption：一套能上雲，不代表一百套能上雲

2021 年起，我們進入 Cloud Adoption。單一系統成功後，問題立刻改變：大規模遷移需要成熟 SOP；雲端支出快速增加，需要 FinOps；多雲環境也讓維運、安全與治理複雜許多。2023 年主管機關改採更明確的風險基礎管理，並簡化部分申請範圍，也替金融業規模化採用雲端創造了條件，可參考[金管會修法說明](https://www.fsc.gov.tw/uploaddowndoc?file=newlaw%2F202308251646480.pdf)。

![單一雲端工作負載經過中央治理與標準化軌道，擴展為秩序井然的企業級規模]({{ site.baseurl }}/assets/images/cathay-cloud-transformation-scale-governance.webp){: .align-center}
*從一套到百套，真正增加的不只是資源數量，而是治理、維運與成本管理的複雜度。*

我們在 2023 年底成立雲端策略發展部，扮演 CCoE 的角色，設置上雲戰情室、Sandbox，並把 DevSecOps、Policy as Code、SRE、ITSM、平台工程與 FinOps 納入共同運作。各子公司也需要對應的引導組織；金控不能只在總部喊「要上雲」，還得讓銀行、人壽、產險與證券都知道怎麼做。

Sandbox 尤其重要。新技術導入時，如果完全不允許同仁犯錯，大家就不敢碰，也不可能累積真正的操作經驗。我們需要在正式環境之外，提供一個有邊界、可觀測、出錯也能復原的練習場。治理的目的不是把所有路封死，而是讓團隊知道可以在哪裡安全試驗，以及越過哪條線之前必須停下來確認。

多公雲不是為了蒐集品牌，而是不同時期與場景的結果。早期應用系統選擇 Google Cloud，與台灣已有資料中心有關；數據團隊偏好 AWS；OA 與生產力工具則自然連到 Microsoft 365 與 Azure。最後形成混合雲加多公雲，也必須承擔跨雲治理的複雜度。

FinOps 則像管理水電。地端伺服器買下去，預算已經支出；雲端資源一開啟，水龍頭就開始流。除了巡查閒置資源、調整過度配置、管理儲存分層與跨區傳輸，也要預估未來用量，透過承諾使用取得更好的價格。**優化雲端不是一味砍資源，而是把資源放在最有價值的地方。**

我們也把評估、設計、審查、維運與資產盤點沉澱成 Cloud Ready Platform 與 Cathay Information Asset。當方法論被做成平台，轉型才不必每個專案重新發明一次。

## 4. Cloud First：生成式 AI 帶來第二波雲端需求

到了 2025 年，雲端開始從選項變成預設。Cloud First 的意思不是任何系統都不顧條件直接上雲，而是新系統先評估雲端；即使暫時留在地端，也盡量採用 cloud-native 架構與 API，替未來移動保留彈性。

生成式 AI 讓這個策略更有感。2023 年起，數據團隊開始提出 GPU 機房需求，有時一估就是一百、兩百張 H100。當時詢價，一張 GPU 約百萬元台幣；廠商評估的散熱系統租用費，一年也要兩、三千萬元。更麻煩的是，採購與建置可能花掉一年，而 GPU 三、四年就進入下一個汰換週期。

我們因此反問：能不能先在雲端按需租用，完成模型與架構的可行性驗證，再決定是否自建？團隊實際看到結果後，才逐步放下「一定要先蓋 GPU 機房」的想法。雲端在這裡提供的不是比較便宜的口號，而是**先小額驗證、成功再擴大**的選擇權。

企業使用生成式 AI 當然不能只有算力。身分、授權、稽核、網路隔離、DLP 與資料治理都必須一起進來。我們傾向使用三大雲提供的企業級 GenAI 服務，把 AI 納入既有 VPC 與治理框架。因為 AI 的燃料是資料，而資料與 AI 的底層又是雲；多年 Cloud Ready 與 Cloud Adoption 的累積，剛好替這一波需求打好地基。

![雲端基礎設施承托資料流與儲存，再由最上層 AI 將資料轉化為決策]({{ site.baseurl }}/assets/images/cathay-cloud-transformation-cloud-data-ai.webp){: .align-center}
*AI 不會憑空創造企業價值：Cloud 是地基，Data 是燃料，AI 才能成為能力。*

## 5. Smart Archie：不要讓一個 Agent 假裝什麼都會

大規模上雲後，架構設計出現三個痛點：敏捷迭代讓需求反覆改變；金融業政策與安全要求很多；多雲架構圖又缺乏一致格式。這促使我們開發 Smart Archie，讓 GenAI 協助產生架構、套用 Policy as Code，並把設計流程標準化。

Smart Archie 一開始是 Prompt-based LLM，後來加入 Knowledge Base 成為 RAG，再演進到能拆解任務的 Agent。如果把所有能力塞進同一個 Agent，Prompt 會失控、Knowledge Base 難維護，Context Window 也容易過載，所以我們最後選擇 Multi-Agent：

- Architect Agent 負責理解需求、分派任務與整合結果。
- DaC Agent 以 Diagram as Code 產生架構關係。
- Solution Agent 分析服務組合並檢核政策。
- TCO Agent 估算成本與提出資源優化建議。
- IaC Agent 把架構轉成可進入 CI/CD 的 Terraform。

![一位使用者提出需求，由中央架構師 Agent 協調四個專職 Agent 完成設計、分析、估算與交付]({{ site.baseurl }}/assets/images/cathay-cloud-transformation-smart-archie-team.webp){: .align-center}
*Multi-Agent 的價值不是多放幾個機器人，而是把複雜任務拆成可管理、可檢查的專業分工。*

Demo 裡，使用者只要補上一個新需求，系統就會重新分析方案、更新架構與成本，最後輸出 IaC。這讓架構師把時間從重複繪圖與搬資料，移回真正需要人判斷的技術決策。Smart Archie 也建構在 AWS 與 Amazon Bedrock 上，證明 Cloud First 不只是政策，而是我們自己開發產品時採取的實作路徑。

這個產品也讓我更確定，自研不代表什麼都要自己守到底。我們早期花力氣做過成本估算，後來雲端服務商推出更成熟的現成能力，接進來效果更好，我就請團隊直接採用。真正可持續的平台，應該能替換元件、吸收新的外部能力，而不是把過去寫過的每一行程式碼都當成不能放手的資產。

## 6. 最後的答案：轉型是一場信心工程

回顧整段旅程，我把洞察整理成 People、Process、Technology 三件事。

**People：組織文化比技術更難升級。** DDT、Cloud Ready 與 CCoE 的作用，是讓雲端導入從阻力變成信心工程。大家願意相信方向，才會願意改變工作方式。

**Process：標準化是規模化的前提。** Cathay 6R、SOP 與 Cloud Ready Platform，讓不同系統與團隊可以沿著相同軌道前進。沒有標準化，就無法治理百套系統。

**Technology：雲與 AI 重新分配人的時間。** 程式碼、架構圖與 IaC 都能由 AI 協助產出；人的價值則更集中在問題定義、洞察與判斷。

![三位同事把共識、標準流程與 AI 技術匯聚成同一條通往決策的路徑]({{ site.baseurl }}/assets/images/cathay-cloud-transformation-people-process-tech.webp){: .align-center}
*文化決定願不願意走，流程決定能不能一起走，技術則決定可以走多快。*

所以我在最後仍然提醒大家，AI 時代需要兩類能力。Networking、Cloud Architecture、Cloud-Native Development、DevOps、SRE、Data & AI、Cloud Security 等 Hard Skills，是你能否提出好問題的地基；洞察與批判思考、創意、自我管理、溝通協作與領導影響力等 Soft Skills，則決定你能把 AI 的能力放大到什麼程度。

**AI 取代的是技術操作，而不是專業能力。**

國泰所謂「走在同業前面」，不是因為所有系統都已經在雲上，也不是因為我們沒有繞路；而是我們把每一次碰撞變成方法，把方法變成平台，再把平台變成整個組織可重複使用的能力。真正的雲端轉型，不是完成一次遷移，而是讓下一次改變來臨時，組織已經準備好。
