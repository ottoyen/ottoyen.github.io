---
title: "從 Cloud First 到 Agent First：企業 IT 為何要提前為 AI Agent 打底"
date: 2026-07-01T15:30:00+08:00
categories:
  - Talk
tags:
  - 臺灣雲端大會
  - Cloud First
  - Agent First
  - AI Agent
  - AI Scrum Team
  - Agent Platform
  - Platform Engineering
  - AgentOps
header:
  teaser: /assets/images/agent-first-hero.webp
---

> AI Agent 的想像很美，但企業現場很硬。真正的瓶頸往往不是模型夠不夠聰明，而是企業能否讓 Agent 安全取得資料、使用工具、呼叫 API、參與流程，並且全程可觀測、可治理、可稽核。這場演講從我的個人 Agent 實驗、AI Scrum Team 的「6+1」模式，一路談到企業級 Agent Platform，以及 Cloud First 為何正好替 Agent First 打下基礎。

{% capture notice-2 %}
#### 會議資訊

* 會議名稱：[2026 iThome 臺灣雲端大會](https://cloudsummit.ithome.com.tw/2026/)
* 演講時間：2026-07-01 15:30–16:00
* 演講地點：台北南港展覽館 2 館 7 樓 701D
* 相關連結：[🗓️官方議程](https://cloudsummit.ithome.com.tw/2026/agenda)   [📕PDF簡報]({{ site.baseurl }}/assets/從 Cloud First 到 Agent First v1.pdf)   [🎵Podcast](https://audio.ottoyen.dev/ottoyen/2026-07-01-from-cloud-first-to-agent-first.m4a)
  {% endcapture %}

<div class="notice">{{ notice-2 | markdownify }}</div>

![雲端基礎建設逐步轉化為由人與 AI Agent 協作的數位工作環境]({{ site.baseurl }}/assets/images/agent-first-hero.webp){: .align-center}
*Cloud First 建立數位基礎建設，Agent First 則讓數位勞動力能在其上安全工作。*

## 1. AI Agent 的想像很美，但企業現場很硬

這幾年我在雲端大會分享的內容，大多與上雲有關。去年提報這個題目時，我還不知道 2026 年的 AI 會發展到哪裡，只覺得 AI Agent 應該會成為重要議題。到了今年上半年，從個人使用的 Agent、Coding Agent，到各家雲端業者推出的企業方案，市場真的快速升溫。

但我愈實際使用，愈確定一件事：**Agent 最大的瓶頸不是模型，而是企業系統能否被安全接入。**

在對話視窗裡問問題很容易；要讓 Agent 真的工作，就必須讓它接觸資料、權限、API、文件、帳號與既有流程。個人使用時，這些問題還能靠自己的信任和手動修正處理；到了團隊和企業，任何模糊的授權都可能被放大。

因此，我把 Agent 能力的放大分成三個層次：

1. **個人 Agent**：看見 Agent 能持續工作的真實能力，也看見它的邊界。
2. **團隊 Agent**：讓 AI 不再是用完就關掉的工具，而是參與共同流程的第七位成員。
3. **企業 Agent**：當 Agent 從一個變成數百個，問題核心就轉向平台工程、治理與營運。

Agent First 不是把 AI 接到企業裡，而是把企業變成 AI Agent 可以安全工作的地方。

## 2. 個人 Agent：第一次覺得 AI 真的在工作

今年二月開始，我讓 Agent 真正住進自己的電腦。四十多天裡，我做了三十多個 one page、十二個 skills，也使用過四個不同的 AI agents。它會替我整理科技新聞、抓取文章全文、查資料、翻譯，甚至把我真正關心的內容做成每天開車時可以收聽的 Podcast。

第一次讓我有強烈「Aha Moment」的，是請它幫我訂武陵農場的露營位。櫻花季營位很難搶，我先讓它每三十分鐘檢查一次。第一次發現空位時，它只通知我；等我看到訊息再打開網站，名額早已被搶光。

我於是調整指令：發現空位就直接協助訂位，不要只通知。後來它真的完成了。那一刻的感受很不一樣——它不是回答了我一個問題，而是持續監控環境，等條件成立後替我完成工作。

但這個例子同時也顯示了邊界。訂位進入刷卡付款時，我曾嘗試提供卡號，Agent 拒絕接受，因為它判斷這是高風險行為。Agent 愈有能力，我們就愈不能只問「它會做什麼」，還要問「哪些事可以直接做、哪些必須先問、哪些永遠不能做」。

### 從 Prompt 到 Skill

使用 Agent 時，我們會透過對話反覆調整做法。當一件工作做過一兩次、流程已經穩定，下一步就可以請 Agent 把它沉澱成 skill，形成可重複執行的工作流。

![個人把一次性需求交給 AI Agent，經過資料收集、整理、產出，最後沉澱為可重複執行的工作流]({{ site.baseurl }}/assets/images/agent-first-personal-workflow.webp){: .align-center}
*Agent 的能力不只是問答，而是把 Prompt 逐步沉澱成每天都能運作的 Skill。*

例如每日新聞摘要會經過抓取來源、整理全文、產生摘要、上傳 NotebookLM，再生成語音；另一個流程會進入 The Verge 的每篇文章取得全文，而不是只看標題；GitHub 編輯精選 Podcast 則會完成選題、整理與語音產出。

真正的挑戰不是成功一次，而是今天成功、明天更新後仍然能工作。模型具有不確定性，Agent Runtime 和外部網站也會變動，因此個人 Agent 很快就會碰到類似 SRE 的課題：失敗如何重試、輸出如何驗證、異常如何被發現、流程如何修復。

## 3. Agent Harness：能力愈強，愈需要安全紅線

一個 Agent 的表現不只由模型決定。模型、Agent Runtime、skills、工具權限、記憶與 guardrails 共同構成 Agent Harness。好的 Harness 應該讓工作流不會因為更換模型或工具就全部失效，也要讓不同執行環境維持一致的行為邊界。

我會把動作分成三層：

- **可以直接執行**：整理資訊、監控公開狀態、產出分析草稿。
- **必須先詢問**：登入帳號、提交表單、修改正式資料、對外發布。
- **必須拒絕**：未授權金流、敏感資料外流與高風險權限操作。

![AI Agent 通過多層授權閘門，安全任務可通行，敏感操作需要人工核准，高風險行為則被阻擋]({{ site.baseurl }}/assets/images/agent-first-guardrails.webp){: .align-center}
*治理不是把 Agent 關起來，而是讓它知道可以安全走到哪裡，以及何時必須把決定交還給人。*

個人場景已經會遇到帳號、金流與個資；企業場景只會把問題放大十倍、百倍。沒有清楚邊界的 Agent，不可能因為換成更強的模型就突然變得安全。

## 4. AI Scrum Team：六位真人，加上一位 AI 成員

到了五月，公司開始把 Agent 機制導入開發團隊，形成 AI Scrum Team。過去常用「兩個披薩」形容一個約十到十二人的敏捷團隊；既然 AI 已經進入開發流程，我們嘗試把人的編制縮小為六人：

- PM／Product Owner 一人。
- Design／UI/UX 一人。
- RD 三人，涵蓋前端、後端與架構。
- QA 一人。

再加上一位 AI Member，成為「6+1」。

![六位真人成員與一位 AI 成員共同圍繞桌面規劃 Sprint，AI 以隊友身份參與討論]({{ site.baseurl }}/assets/images/agent-first-scrum-team.webp){: .align-center}
*AI 不是開在瀏覽器裡、用完就關掉的工具，而是有身份、資源與工作協議的團隊成員。*

這個「+1」不是一個聊天頁面。它有自己的主機、帳號、身份與存取範圍，可以持續存在於團隊流程中。Sprint Planning 時，它能整理需求背景、找相依性、補足驗收條件；Daily Scrum 時，能整理進度與阻塞；Development 階段，可以參與 code review、測試、文件和 PR 摘要；到了 Review 或 Retro，則協助沉澱決策、缺陷模式和下一輪改善。

6 月 23 日 Anthropic 發表 [Claude Tag](https://www.anthropic.com/news/introducing-claude-tag)，讓團隊能在 Slack 頻道直接 `@Claude` 指派任務。它會在授權的 channel 中累積共同上下文、使用指定工具、非同步完成工作，再回到 thread 報告結果。這與我們設計「第七位成員」時的想法非常接近：不是問完就消失，而是留在流程裡，替團隊保存共同脈絡。

## 5. 它是在監督團隊，還是在幫助團隊？

Agent 進入團隊後，很快會出現一個敏感問題：它知道任務進度、branch 是否合併、誰遇到阻塞，也能整理個別工作量。那麼，它究竟是管理者派來監督團隊的工具，還是幫助大家完成 sprint goal 的成員？

這不是功能問題，而是目標設計問題。

如果 Agent 的目標是找出誰沒做事，團隊會把它視為壓力來源，最後只剩形式化配合，甚至不願意讓它進入真正重要的對話。若它的目標是補足上下文、提醒阻塞、減少重工，替團隊扛下繁瑣流程，它就更可能成為大家願意合作的隊友。

有一個團隊讓自己的 AI Member 回答「你是來監督大家，還是真正的 member？」它給出一個很誠實的判準：如果它消失，只有一個人發現，它還只是工具；如果好幾位成員都感覺團隊少了什麼，這個角色才算真的長出來。

因此，AI 成員同樣需要 onboarding：

1. **給它身份**：帳號、主機、repo、API 與文件存取權。
2. **給它規範**：哪些可以做、哪些要先問、哪些永遠不能做。
3. **給它上下文**：產品目標、團隊慣例、Definition of Done 與架構原則。
4. **給它觀測**：能追蹤做過什麼、使用哪些工具、產出什麼結果。

## 6. FinOps Agent：從目標清楚、價值可量化的工作開始

當每個團隊都可能有第七位成員，下一個問題自然是：企業應該先從哪一種 Agent 開始？

我認為 FinOps Agent 是很好的切入點，因為目標明確——替大家節省不必要的雲端成本。它需要理解成本、政策、標籤、預算和雲端定價，也需要底層的 Policy Engine、Tagging Engine 與成本資料。

當這些基礎能力準備好，FinOps Agent 就能在工程師提交 IaC 變更和 Pull Request 時，自動檢查預估成本與政策；遇到異常時通知工程師，需要核准時交還給人，確認無誤後才進入部署流程。

這類 Agent 不必一開始就取得所有權限，也不需要重做整個企業流程，卻能很快證明 Agent 參與團隊工作的具體價值。

## 7. Cloud First 其實是在替 Agent First 打底

團隊 Agent 成效不錯後，企業很快會想把它複製到更多團隊與流程。但許多 PoC 停住，並不是模型不夠聰明，而是卡在資料、權限、API、流程、治理與安全。

國泰從 Cloud Ready、Cloud Adoption 走到 Cloud First，完成百套系統上雲。過程中建立的 Landing Zone、IAM、Gateway、隔離環境、標準化流程、資安與治理機制，當時看起來都是在處理雲端管理；回頭看，這些正好也是 Agent 可以安全工作的基礎。

大型金融集團還必須同時面對監理、安全、既有系統、跨公司治理和組織共識。合規不能事後再補，身份與權限也不能等 Agent 上線後才設計。Agent 不是一個獨立 App，而是會觸碰整個企業能力的數位勞動力。

完整的 Agent Platform 至少需要八項核心能力：

- Identity：Agent 是誰？
- Knowledge：它可以看哪些知識？
- Tool／API：它可以呼叫哪些系統？
- Workflow：它如何嵌入企業流程？
- Observability：如何知道它做了什麼？
- Security：如何控管它的行為？
- Audit：如何追蹤與稽核？
- Lifecycle：如何上線、更新與退場？

![以 Agent Runtime 為核心，周圍整合身份、知識、工具、流程、觀測、安全、稽核與生命週期能力的平台]({{ site.baseurl }}/assets/images/agent-first-platform.webp){: .align-center}
*企業級 Agent Platform 必須把八項核心能力整合在同一個可營運、可治理的平台上。*

企業 Agent 的發展可以依序分為 **Build、Scale、Governance、Optimize**。先證明 Agent 能解決問題，再讓它接上 Runtime、Gateway、Apps、APIs 與 Tools；規模增加後，逐步導入 registry、identity and access、policy guardrails 與 audit trail；最後透過觀測、評估、回饋、成本與效能調校持續優化。

治理當然重要，但不要在 Build 階段就用過重規範壓住所有實驗。正確做法是先守住必要底線，再隨著規模擴大治理力道。治理者也必須理解技術，才能在創新速度與風險之間建立真正可執行的平衡。

## 8. 打造一個 Agent 很容易，管理五百個才是真正挑戰

一個個人 Agent 可以靠信任與手動修正；一個團隊 Agent 需要 JD、onboarding、權限和工作協議；當企業裡有五百個 Agent，就需要平台化管理身份、授權、工具目錄、知識庫、審計、觀測、成本與生命週期。

![個人 Agent、團隊 Agent 與企業級 Agent 艦隊依序擴大，最後由共同平台與治理機制協調運作]({{ site.baseurl }}/assets/images/agent-first-scale.webp){: .align-center}
*規模從個人、團隊走向企業後，真正的挑戰從打造 Agent 轉為管理 Agent。*

這也是為什麼我認為平台工程在 Agent 時代會變得更重要。多年來管理雲端資源和應用系統累積的治理邏輯，可以延伸到 Agent；改變的是管理對象，從 digital infrastructure 轉成 digital workforce。AgentOps 也可以借鏡 CloudOps、DevOps、MLOps 與 FinOps 的成熟做法。

Cloud First 建立的是彈性、標準、治理、安全且可營運的數位基礎建設；Agent First 則是在這個工作場所裡，形成能安全接入企業系統、跨流程協作、被觀測也被稽核的數位勞動力。

**企業今天為雲端、資料、權限、流程與治理打的底，就是明天 AI Agent 能不能真正上工的分水嶺。**
