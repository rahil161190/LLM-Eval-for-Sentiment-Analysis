## 📊 

### Situation
Evaluating sentiment analysis models across different LLM providers is challenging because outputs vary in format.Different language models often give very different answers when asked to analyze text. Some respond clearly, while others struggle to follow instructions or provide results in the right format. This makes it hard to compare them fairly and understand which ones are most reliable for sentiment analysis.

### Task
The goal was to design a unified evaluation pipeline that:
- Benchmarks Groq‑hosted and Hugging Face models on the SST‑2 dataset.
- Ensures consistent prompts and structured outputs.
- Tracks performance, errors, and resource usage for fair comparison.

### Action
- Selected 100 balanced samples from the SST‑2 dataset (49 negative, 51 positive).
- Applied a standardized JSON‑format prompt across all models.
- Implemented a custom Hugging Face model manager for caching and quantization.
- Integrated Opik for dataset conversion, inference logging, and metric tracking.
- Evaluated Groq‑hosted Llama models and Hugging Face Llama/Gemma models.

### Result
| Model                  | Provider   | Accuracy | F1 Score | Precision | Recall | Notes                                                                 |
|-------------------------|-----------|----------|----------|-----------|--------|----------------------------------------------------------------------|
| **Llama‑3.1‑8B‑Instant** | Groq      | 0.61     | 0.61     | 0.13      | 0.13   | Produced valid JSON outputs, but struggled with positive class detection. |
| **Llama‑3.3‑70B‑Versatile** | Groq   | 0.64     | 0.64     | 0.15      | 0.15   | Slightly better than 8B, but still low precision/recall for positives. |
| **Llama‑3.2‑3B‑Instruct** | Hugging Face | 0.50 | 0.50     | 0.00      | 0.00   | Failed due to gated access; evaluation shows no positive predictions. |
| **Gemma‑2B‑Instruct**   | Hugging Face | 0.86 | 0.86     | 0.37      | 0.37   | Best performer in this run; balanced predictions with higher precision/recall. |

---

### Interpretation
- **Groq models (8B & 70B)**: Delivered consistent outputs but often defaulted to negative predictions, leading to low precision and recall for positives.  
- **Hugging Face Llama‑3.2**: Could not be properly evaluated due to access restrictions, resulting in poor scores.  
- **Gemma‑2B**: Achieved the strongest performance, with higher accuracy and balanced precision/recall, showing it could reliably identify both positive and negative sentiments.  
- Overall, the pipeline highlighted the **strengths of Groq models for free, fast inference** and the **importance of authentication for Hugging Face gated models**, while demonstrating that Gemma‑2B is a strong open‑weight option for sentiment task
