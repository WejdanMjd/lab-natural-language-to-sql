# Report: Analyzing SQL Query Generation Using Language Models on E-commerce Schema

This report summarizes findings from a series of experiments conducted to evaluate the ability of a large language model to generate accurate SQL queries based on natural language questions, given a predefined e-commerce database schema.

The evaluation was structured around a consistent template where natural language questions (NLQs) were embedded into a prompt containing:

- The database schema
- Task instructions
- Sample SQL queries

The generated SQL queries were extracted and analyzed for **syntactic correctness**, **semantic alignment** with the question, and **consistency** with the database schema.

---

## ✅ Accuracy and Performance

In most instances, the model successfully produced accurate and syntactically valid SQL queries.

- **Example 1:** For the question _"Return the names of customers who bought products in the 'Electronics' category"_, the model correctly:
  - Used JOINs across `customers`, `orders`, `order_items`, and `products`
  - Applied filtering with `WHERE p.category = 'Electronics'`
  - Used `DISTINCT` to eliminate duplicate customer names

- **Example 2:** For _"Return the total amount spent by each customer"_, the model:
  - Correctly used `SUM(o.total_amount)`
  - Joined `customers` with `orders`
  - Grouped by customer name

These demonstrate the model’s capacity for handling **aggregations** and **multi-table relationships** effectively.

---

## ⚠️ Variations and Limitations

While generally correct, a few notable issues were observed:

- **Inconsistent Outputs on Minor Changes:** When changing a query slightly (e.g., replacing `"Electronics"` with `"Clothing"`), the generated SQL sometimes reused old outputs or unrelated templates, failing to reflect the new condition (`WHERE p.category = 'Clothing'` missing).
  
- **Fragile String Extraction:** Using string parsing methods like:
  ```python
  SQL[0].split("```sql3")[-1].split("```")[0].split(";")[0].strip() + ";"
