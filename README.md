## Диалоговая система для консультирования пациентов по медицинской документации 

### 📄 About
В данном проекте представлен чат-бот, предназначенный для помощи пациентам, разработанный методами Retrieval-Augmented Generation (RAG). Чат-бот предоставляет ответы на основе инструкций к противоопухолевым препаратам, взятых с сайта «Государственный реестр лекарственных средств» (ГРЛС). 

### ⚙️ Features
**• Ensemble Retriever:** FAISS + BM25 c Reciprocal Rank Fusion (RRF)

**• Reranking:** Cross-Encoder (BAAI/bge-reranker-v2-m3)

**• Question & Answer Generation:** GPT-4o, Meta-Llama-3.1-8B-Instruct

**• Evaluation:** Precision@k, Recall@k и метрики из библиотеки RAGAS (LLM Based Context Precision, Response Relevancy, Faithfulness)

### 📊 Dataset
Датасет состоит из пар вопросов и ответов. У каждого найденного фрагмента есть метаданные:

- `"mnn"` – международное непатентованное наименование (действующее вещество);
- `"trade_name"` – торговое наименование;
- `"drug_id"` – ID лекарства в ГРЛС;
- `"file_name"` – названия файла инструкции;
- `"chunk_id"` – номер (ID) чанка;
- `"document_id"` – номер (ID) инструкции для удобства логирования;
- `"score"` – оценка релевантности, данная Cross-Encoder;
- `"text"` – текст чанка.

Датасет имеет следующий вид:
```
{
  "Кармустин": {
    "mnn_questions": [
      {
        "question": "Опишите, как кармустин воздействует на раковые клетки?",
        "question_chunks": [
          {
            "mnn": "Кармустин",
            "trade_name": "БиКНУ",
            "drug_id": "a87f8bf4-c9cf-4581-90a3-cf037a953518",
            "file_name": "d9de5e80-5158-42b7-a51a-6d414cda6a5e.pdf",
            "chunk_id": 4,
            "document_id": 1494,
            "score": 0.6413100537906452,
            "text": "Перед применением препарата…"
          }
        ],

"answer": "Кармустин относится к группе…"
        "answer_chunks": [
          {
            "mnn": "Кармустин",
            "trade_name": "БиКНУ",
            "drug_id": "a87f8bf4-c9cf-4581-90a3-cf037a953518",
            "file_name": "d9de5e80-5158-42b7-a51a-6d414cda6a5e.pdf",
            "chunk_id": 1,
            "document_id": 1824,
            "score": 0.9356999454877224,
            "text": "Он может навредить им, даже если симптомы…"
          }
        ]
      }
    ],
    "trade_name_questions": {
      "БиКНУ": [
        {
          "question": "Какова рекомендуемая дозировка БиКНУ при лечении глиобластомы?",
          "question_chunks": [
            {
              "mnn": "Кармустин",
              "trade_name": "БиКНУ",
              "drug_id": "a87f8bf4-c9cf-4581-90a3-cf037a953518",
              "file_name": "d9de5e80-5158-42b7-a51a-6d414cda6a5e.pdf",
              "chunk_id": 0,
              "document_id": 364,
              "score": 0.9926256346917622,
              "text": "БиКНУ, 100 мг, лиофилизат для приготовления…"
            }
          ],

"answer": "Рекомендуемая начальная доза…"
          "answer_chunks": [
            {
              "mnn": "Кармустин",
              "trade_name": "БиКНУ",
              "drug_id": "a87f8bf4-c9cf-4581-90a3-cf037a953518",
              "file_name": "d9de5e80-5158-42b7-a51a-6d414cda6a5e.pdf",
              "chunk_id": 3,
              "document_id": 1704,
              "score": 0.9039745112122062,
              "text": "Неизвестно, проникают ли компоненты…"
            }
          ]
        }
      ]
    }
  }
}
```

### 📁 Data Access
Из-за ограничений на размер файлов на GitHub некоторые материалы хранятся на Google Drive:

🔗 [Developing a dialogue system for guiding patients through medical documentation](https://drive.google.com/drive/folders/1Ep5nAj1kOB0OG4K6YA6oaCNyKqOHsTvN?usp=sharing)

- `Instructions.zip` – исходные инструкции в формате PDF;
- `updated_ocr_instructions` – извлеченные инструкции с помощью Tesseract-OCR; 
- `updated_instructions` – извлеченные инструкции с помощью PyMuPDF; 
- `cleaned_chunks` – инструкции после удаления "шумов";
- `chunked_instructions` – чанки инструкций;
- `embedded_chunks` – векторизированные чанки;
- `faiss` – индексированные чанки;
- `test_dataset_28_mnn.json` – сгенерированный датасет;
- `evaluated_test_dataset_28_mnn.json` – датасет с указанием оценок, рассчитанных по метрикам из RAGAS.
