# Arabic-to-SQL Translation
This project is a seq2seq model that converts Arabic natural language questions into SQL queries. It aims to simplify database interaction for Arabic speakers by eliminating the need to understand SQL syntax. Given an Arabic question and a database schema, the model generates a valid SQL query to retrieve the required data.
# Project Architecture
## 1. Data Pipeline
- Data Collection: Uses the Spider dataset (AR_spider.jsonl) containing Arabic questions paired with SQL queries and database schemas
- Schema Extraction: Extracts database schemas from SQLite files to understand table structures and relationships
- Text Normalization:
  - Arabic text processing using CAMeL Tools (normalization, diacritic removal)
  - SQL query standardization (formatting, keyword capitalization)

## 2. Model Architecture
- Base Model: T5 (Text-to-Text Transfer Transformer) small model fine-tuned for text-to-SQL tasks
- Enhancements:
  - Added Arabic-specific tokens from CAMeLBERT vocabulary
  - Incorporated SQL special tokens (operators, keywords)
  - Modified model architecture with:
    - Increased d_model (768) and d_ff (3072) dimensions
    - More layers (8) and attention heads (12)
    - Adjusted dropout rate (0.1)

## 3. Training Process
- Input Format: Combines database schema with Arabic question
```
Database schema:
[TABLE]table1[/TABLE]([COLUMN]col1[/COLUMN], [COLUMN]col2[/COLUMN])
[TABLE]table2[/TABLE]([COLUMN]col3[/COLUMN])

Translate Arabic to SQL: [Arabic question]
```
- Training Parameters:
  - Batch size: 8 (with gradient accumulation)
  - Learning rate: 1e-5 with cosine scheduling
  - 10 epochs with early stopping
  - Beam search (5 beams) for generation

 ## 4. Constraints and Validation
- SQL Grammar Constraints: Ensures generated queries follow SQL syntax
- Schema Constraints: Validates that tables/columns exist in the schema
- Output Validation: Checks for basic SQL validity before returning results


# Key Features
1. Arabic Language Support:
   - Handles Arabic script variations and diacritics
   - Incorporates common Arabic tokens from CAMeLBERT

2. Schema-Aware Generation:
  - Explicitly represents database structure in the input
  - Validates generated queries against the schema

3. Robust Training:
  - Custom learning rate scheduling
  - Combined evaluation metrics (exact match + token accuracy)
  - Error handling for invalid SQL generation

# Results
The model achieves:
- Progressive reduction in training loss over epochs
- Improved token-level accuracy in SQL generation
- Capability to handle various SQL query structures (SELECT, WHERE, JOIN, etc.)

# Future Improvements
1. Retrieval-Augmented Generation (RAG) Integration
2. Enhanced Schema Understanding
3. Advanced Validation:
4. Prompt Engineering
