# AI Novel Generator: 長篇小說自動化創作系統
本專案是一個基於 n8n 的深度自動化工作流，專為解決 AI 創作長篇小說時的「邏輯斷層」與「設定遺忘」而設計。系統結合了 Claude 3.5/4.5 Sonnet、Supabase 向量資料庫 與 RAG 技術，打造出具備長期記憶與風格一致性的數位作家。
<br>
<br>

##核心技術亮點
<br>

###1. 多層級脈絡管理 (Context Management)
不同於一般單次對話，本流程將資料分為三層輸入 AI，確保劇情不跑偏：


* **主線層 (Primary)**：直接導入上一章完整內容，確保文字接續自然無縫。
* **伏筆層 (Secondary)**：從資料庫拉取過往所有章節的摘要 (Summaries)，維持設定一致性。
* **風格層 (RAG)**：從 ref_novel 向量池檢索相似風格片段，引導 AI 學習特定的描寫節奏，而非盲目生成。
<br>


###2. 結構化輸出與 JS 解析
為了解決 LLM 有時會輸出多餘廢話的問題，本流程內建了強大的 JavaScript 解析節點：
強制 JSON 提取：自動定位輸出中的 { } 區塊。
自動格式校正：確保標題、章節號、摘要與正文被精確拆分，方便後續寫入資料庫。

<br>

###3. 多渠道自動同步
Supabase：永久儲存結構化章節，作為 AI 的長期記憶庫。
Google Docs：即時同步寫作進度，方便作者隨時進行人工潤稿與校對。

<br>
<br>

##專案檔案說明
workflows/AI_novel_creator.json: 核心工作流檔案。
data/: 存放 Supabase 資料表結構建議與 CSV 範例。
prompts/: 存放針對「恐怖懸疑/規則怪談」調教的高階 System Prompt。

<br>
<br>


##快速開始
<br>
###1. 匯出與匯入
下載 workflows/AI_novel_creator.json。
在 n8n 介面點擊 Import from File... 並選擇該檔案。

<br>

###2. 設定憑證 (Credentials)
請在以下節點填入您的 API Key：
<br>

Anthropic API: 建議使用 claude-3-5-sonnet 或更新版本。
OpenAI API: 用於 Embeddings 向量運算。
Supabase: 需填入專案 URL 與 Service Role Key。
Google Docs: 需完成 OAuth2 授權。

<br>
<br>

##3. 啟動創作
點擊 Execute Workflow，系統將自動讀取最後一章進度，並開始撰寫全新內容。

<br>
<br>


###提示詞工程 (Prompt Engineering)


本專案的 Prompt 經過深度調優，包含：
戰力體系維持：強制 AI 監控事件難度指標。
RAG 隔離機制：嚴格禁止 AI 抄襲參考片段中的專有名詞，僅允許學習其「感官描寫比例」與「敘事留白」。
