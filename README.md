# LLM Evaluation Pipeline for Sentiment Analysis(Movie Reviews)

## Situation
Evaluating sentiment analysis models across different LLM providers is challenging because outputs vary in format. Different language models often give very different answers when asked to analyze text. Some respond clearly, while others struggle to follow instructions or provide results in the right format. This makes it hard to compare them fairly and understand which ones are most reliable for sentiment analysis.

In this project, evaluation is performed on the **SST‑2 (Stanford Sentiment Treebank)** dataset, which consists of **movie reviews extracted from Rotten Tomatoes**. Each review has been **human‑annotated** for sentiment, with labels assigned as either *positive (1)* or *negative (0)*. Neutral examples were removed to make the task strictly binary. With ~67K training examples and 872 validation examples, SST‑2 is a widely recognized benchmark for **movie review sentiment analysis**.

---

## Task
The goal was to design a unified evaluation pipeline that:
- Benchmarks Groq‑hosted and Hugging Face models on the SST‑2 dataset.
- Ensures consistent prompts and structured outputs.
- Tracks performance, errors, and resource usage for fair comparison.

---

## Action
- Selected 100 balanced samples from the SST‑2 dataset (49 negative, 51 positive).
- Applied a standardized JSON‑format prompt across all models.
- Implemented a custom Hugging Face model manager for caching and quantization.
- Integrated Opik for dataset conversion, inference logging, and metric tracking.
- Evaluated Groq‑hosted Llama models and Hugging Face Llama/Gemma models.

---

## Result

| Model                  | Provider      | Accuracy | F1 Score | Precision | Recall | Notes                                                                 |
|-------------------------|--------------|----------|----------|-----------|--------|----------------------------------------------------------------------|
| **Llama‑3.1‑8B‑Instant** | Groq         | 0.61     | 0.61     | 0.13      | 0.13   | Produced valid JSON outputs, but struggled with positive class detection. |
| **Llama‑3.3‑70B‑Versatile** | Groq      | 0.64     | 0.64     | 0.15      | 0.15   | Slightly better than 8B, but still low precision/recall for positives. |
| **Llama‑3.2‑3B‑Instruct** | Hugging Face | 0.50     | 0.50     | 0.00      | 0.00   | Failed due to gated access; evaluation shows no positive predictions. |
| **Gemma‑2B‑Instruct**   | Hugging Face | 0.86     | 0.86     | 0.37      | 0.37   | Best performer in this run; balanced predictions with higher precision/recall. |

---

## Interpretation
- **Groq models (8B & 70B)**: Delivered consistent outputs but often defaulted to negative predictions, leading to low precision and recall for positives.  
- **Hugging Face Llama‑3.2**: Could not be properly evaluated due to access restrictions, resulting in poor scores.  
- **Gemma‑2B**: Achieved the strongest performance, with higher accuracy and balanced precision/recall, showing it could reliably identify both positive and negative sentiments.  

---

## 💡 Hands‑On Experience Gained
Through this project, I developed practical skills in:
- **Generative AI Evaluation** – Running multiple LLMs on the same dataset and comparing their outputs  
- **LLM Integration** – Combining Groq‑hosted and Hugging Face models into one unified workflow  
- **Prompt Design** – Crafting clear, structured prompts to enforce consistent JSON outputs  
- **Model Management** – Building a custom Hugging Face model manager for caching, quantization, and memory control  
- **Dataset Preparation** – Sampling and converting SST‑2 data into Opik format for reproducible evaluation  
- **Error Handling & Debugging** – Managing issues like failed responses and resource constraints  
- **Evaluation Metrics** – Applying precision, recall, F1, ROUGE, and BERTScore to measure model quality  
- **Experiment Logging** – Using Opik to track results, errors, and traces for observability  

---

## 🛠️ Tech Stack
- **Python 3.12** – Core programming language for pipeline logic  
- **PyTorch** – Deep learning framework for inference  
- **Transformers (Hugging Face)** – Pre‑trained models and tokenizers  
- **Groq API (OpenAI‑compatible)** – Hosted Llama models for free‑tier inference  
- **Opik** – Evaluation and observability framework (dataset conversion, trace logging)  
- **Datasets (Hugging Face)** – Loading SST‑2 benchmark dataset  
- **BitsAndBytes** – Quantization for memory optimization  
- **Accelerate** – Streamlined inference across devices  
- **Rouge‑Score & BERTScore** – Metrics for text evaluation  
- **Pandas & tqdm** – Data handling and progress visualization  

---
## 📈 Impact
By completing this project, I strengthened my ability to:
- Compare models across providers in a fair, transparent way  
- Handle practical challenges like output parsing and evaluation consistency  
- Design reproducible pipelines that can be extended to other NLP tasks  

## Overall
The pipeline highlighted:
- The strengths of Groq models for free, fast inference.
- The importance of authentication for Hugging Face gated models.
- That **Gemma‑2B is a strong open‑weight option for sentiment tasks, especially movie review sentiment analysis**.
