# 🧠 Analyzing SQL Query Generation Using Language Models on E-commerce Schema

This report summarizes findings from a series of experiments designed to assess the ability of a large language model to generate accurate SQL queries from natural language questions (NLQs), based on a predefined **e-commerce database schema**.

## 📋 Evaluation Setup

Each prompt included:
- The full database schema 🗃️  
- Task instructions 🧾  
- Sample natural language queries (NLQs) 💬  
- Expected SQL query output 🧑‍💻  

The generated queries were analyzed for:
- ✅ **Syntactic correctness**
- 🎯 **Semantic alignment** with the NLQ
- 🔗 **Consistency** with the database schema

---

## 📈 Accuracy and Performance

In most cases, the model produced **accurate** and **syntactically valid** SQL queries.

### ✅ Successful Examples

- **NLQ**: *"Return the names of customers who bought products in the 'Electronics' category."*  
  **Result**:  
  - Used correct `JOIN`s between `customers`, `orders`, `order_items`, and `products`.  
  - Applied `WHERE p.category = 'Electronics'` and `DISTINCT` to avoid duplicates.

- **NLQ**: *"Return the total amount spent by each customer."*  
  **Result**:  
  - Correctly aggregated `total_amount` from the `orders` table.  
  - Used `GROUP BY` on customer names, yielding expected outcomes.

These examples show the model's **strong grasp of SQL semantics** and table relationships.

---

## 🔄 Variations and Limitations

Despite overall strong performance, some issues were observed:

- 🔁 **Context Over-reliance**:  
  Slight changes to the NLQ (e.g., changing "Electronics" to "Clothing") occasionally failed to update the `WHERE` clause, suggesting over-dependence on prior context.

- 🧵 **Extraction Fragility**:  
  When SQL was extracted from markdown (e.g., using `split("```sql3")[-1]`), any deviation from expected markdown formatting caused parsing errors. This revealed fragility in automated processes.

---

## 📌 Critical Observations

### 💪 Strengths
- Accurate use of:
  - `JOIN`s
  - Filtering with `WHERE`
  - Aggregations with `SUM`, `GROUP BY`
- Strong semantic alignment with the NLQs
- Reliable performance when input structure matched training patterns

### ⚠️ Weaknesses
- Repeated outputs or incorrect reuse of previous answers
- Markdown formatting sensitivity affecting automated parsing
- Occasional hallucinations or misinterpretation in modified prompts

---

## 🧾 Conclusion

The model demonstrates solid performance under:
- Structured prompts
- Clearly defined schema
- Well-formatted examples

However, it is sensitive to:
- Input and format variations
- Contextual bleed from previous prompts

### 🔧 Recommendations for Future Work
- Fine-tune on schema-specific data 📊  
- Improve robustness in SQL extraction and post-processing 🛠️  
- Implement prompt templates with dynamic schema integration  
- Include fallback mechanisms in production systems 🔄

---

> 🔍 **Note**: Always validate outputs in production environments to ensure correctness and reliability.

