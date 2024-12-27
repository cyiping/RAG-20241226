# RAG-20241226
- 採用**柯文哲起訴書範例**
- Kaggle / GPU P100

### 測試結果
- OCR效果很差 (用 pdf2image, pytesseract)
- 使用 Hugging Face Transformers 生成嵌入向量
- 因為是採用 kaggle 環境，所以改用 faiss 當向量資料庫

### 功能說明

#### 1. `from pdf2image import convert_from_path`
- **功能**：
  - 將 PDF 文件的每一頁轉換為圖片格式。
  - 適用於處理掃描型 PDF（圖片型 PDF），供 OCR 提取文字使用。
- **用途**：
  - 讀取 PDF 文件並將其頁面轉換為 `Pillow` 圖片對象列表。

---

#### 2. `from pytesseract import image_to_string`
- **功能**：
  - 使用 Tesseract OCR 引擎從圖片中提取文字。
- **用途**：
  - 將圖片格式的 PDF 頁面或圖片中的內容轉換為文字。
  - 支援多語言（需配置相應的 Tesseract 語言包，例如繁體中文 `chi_tra`）。

---

#### 3. `from langchain.text_splitter import RecursiveCharacterTextSplitter`
- **功能**：
  - 將長文本分割為多個小塊，每塊有指定的長度（例如 1000 字），並允許塊之間有一定重疊。
  - 適用於大規模語言模型（LLM）處理的上下文分塊。
- **用途**：
  - 將長文本（例如 OCR 提取的全文）分成可控的文本塊，便於向量化和檢索。

---

#### 4. `from langchain.schema import Document`
- **功能**：
  - LangChain 中的文檔對象類型，包含文本內容和元數據。
- **用途**：
  - 將文本內容（如頁面提取的文字）與相關元數據（如頁碼、來源）一起封裝，方便進行檢索和管理。

---

#### 5. `from transformers import AutoTokenizer, AutoModel`
- **功能**：
  - 來自 Hugging Face Transformers 庫，負責：
    - **`AutoTokenizer`**：自動加載與特定模型兼容的分詞器，用於將文本轉換為模型的輸入格式。
    - **`AutoModel`**：加載預訓練的 Transformer 模型，用於生成嵌入或其他任務。
- **用途**：
  - 提供文本嵌入生成或其他 NLP 任務（如分類、翻譯）的模型支持。

---

#### 6. `import numpy as np`
- **功能**：
  - 提供高效的數值計算功能，支持多維數組和矩陣操作。
- **用途**：
  - 用於處理嵌入向量，或進行向量和矩陣的數學運算（如距離計算）。

---

#### 7. `import faiss`
- **功能**：
  - Facebook AI 提供的高效相似度檢索庫，專門用於大規模嵌入向量的檢索。
- **用途**：
  - 構建向量索引，支持快速檢索最相似的文本塊（如查詢最近鄰向量）。

---

#### 8. `import time`
- **功能**：
  - 提供與時間相關的功能，例如記錄開始和結束時間。
- **用途**：
  - 用於計算程式執行的耗時或添加延遲。

---

#### 9. `import torch`
- **功能**：
  - PyTorch 深度學習框架，支持 GPU 加速的張量計算和模型運行。
- **用途**：
  - 使用 Hugging Face Transformers 時的核心運算引擎，負責執行模型推理和張量操作。

---

### 這些模組結合使用的場景
1. **PDF 處理**：
   - 從 PDF 提取每頁作為圖片，然後通過 OCR 提取文字。
   
2. **文本處理**：
   - 將提取的文本進一步分塊，方便語言模型處理。
   
3. **向量化與檢索**：
   - 使用 Hugging Face 模型生成文本嵌入，並用 FAISS 高效檢索最相關的文本塊。
