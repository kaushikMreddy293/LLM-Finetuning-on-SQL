# LLM-Finetuning-on-SQL


# Natural Language to SQL: Fine-tuning LLMs with LoRA

A project focused on enhancing SQL generation capabilities of Large Language Models through Low-Rank Adaptation fine-tuning.

## 📋 Overview

This project addresses the limitations of general-purpose LLMs when generating SQL queries:
- Syntactically incorrect or inefficient queries
- Hallucination of schema elements
- Inconsistent adherence to SQL best practices

By fine-tuning smaller language models with LoRA, we achieve significant improvements in SQL generation quality while maintaining reasonable resource requirements.

## 🔍 Approach

### Models
- **Llama 3.2-1B-Instruct** (25GB RAM requirement)
- **Qwen2-0.5B-Instruct** (10GB RAM requirement)

### Fine-tuning Method
- **Low-Rank Adaptation (LoRA)** for parameter-efficient tuning
- Targeted the attention mechanism (q_proj, v_proj matrices)
- Training dataset: Wildlife conservation SQL examples from `gretelai/synthetic_text_to_sql`

### LoRA Configuration
```python
lora_config = LoraConfig(
    r=8,                     # Rank of the LoRA matrices
    lora_alpha=16,           # Scaling factor
    lora_dropout=0.1,        # Dropout probability
    target_modules=["q_proj", "v_proj"],
    bias="none",
    task_type="CAUSAL_LM"
)
```

### Training Parameters
- 8 epochs
- Learning rate: 2e-5
- Batch size: 8

## 📊 Results

### Example: Basic Aggregation

**Query:** What is the total number of animals adopted by each community?

**Before Fine-tuning (Llama 3.2-1B):**
```sql
SELECT Community, SUM(AnimalsAdopted) AS TotalAnimalsAdopted 
FROM CommunityEducation 
WHERE Community NOT IN ('CommunityA', 'CommunityB', 'CommunityC') 
GROUP BY Community;
```
*Issues:* Incorrect WHERE clause (excludes communities), misunderstands query intent, would return empty results

**After Fine-tuning (Llama 3.2-1B with LoRA):**
```sql
SELECT Community, SUM(AnimalsAdopted) AS TotalAdopted
FROM CommunityEducation
GROUP BY Community;
```
*Improvements:* Correct GROUP BY implementation, no unnecessary WHERE clause, appropriate column aliasing, follows SQL best practices



## 🔑 Key Observations

- **Query Correctness:** Dramatic improvement in SQL syntax and structure
- **Schema Understanding:** Better utilization of provided schema information
- **SQL Best Practices:** More consistent use of appropriate GROUP BY clauses
- **Hallucination Reduction:** Less invention of non-existent constraints

## 💻 Interactive UI

The project includes a simple, intuitive interface built with Gradio:
- Input fields for natural language query and database context
- Runs directly in Google Colab
- Customizable styling for improved user experience
- Real-time SQL generation with explanations

 <img width="623" alt="image" src="https://github.com/user-attachments/assets/a5bf86d0-19d0-4303-bf9e-13524a3d3674" />

 <img width="623" alt="image" src="https://github.com/user-attachments/assets/11ed0028-f60d-4965-86cd-d1bb411b2680" />

 <img width="623" alt="image" src="https://github.com/user-attachments/assets/c729ba99-193a-47d2-8bd5-4b5499f223d2" />





## Future Work

- Expand training dataset with more diverse SQL patterns
- Optimize for additional SQL dialects
- Evaluate with formal metrics (execution accuracy, result correctness)
- Integrate with database connectors for end-to-end validation

## 🛠️ Setup and Installation

```bash
# Run Python notebooks on Google Collab
  Purchase RAM space if needed

```




