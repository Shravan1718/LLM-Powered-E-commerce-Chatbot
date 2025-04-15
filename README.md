# 🤖 Flipkart E-commerce Chatbot

## Objective
To build an intelligent chatbot for Flipkart that:
- **Answers customer FAQs** using semantic search + LLM (Large Language Models).
- **Provides product recommendations** by interpreting natural language queries, converting them into SQL, and displaying results conversationally.

## Deployment Platform
- **Frontend**: Built and deployed using **Streamlit**

## Functionality Overview
### 1. Dual Mode Bot
The chatbot serves two main purposes:
- **FAQ Mode**: 
  Answers questions such as:
  - "Do you support Cash on Delivery (COD)?"
  - "What is the return policy?"
  
- **Product Query Mode**:
  Provides product details like:
  - "Show me top 3 Nike shoes with rating > 4 and more than 500 reviews"

### 2. Data Sources and Storage
#### A. Product Data
- **Source**: Scraped from [Flipkart - Shoes Category]
- **Columns Captured**:
  - `product_link`
  - `title`
  - `brand`
  - `price`
  - `discount`
  - `avg_rating`
  - `total_ratings`
  
- **Storage**:
  - Initially saved to a **CSV** file
  - Then loaded into a **SQLite** database (`products` table)

#### B. FAQs
- **Stored in ChromaDB**
  - Each FAQ includes:
    - `question` (used for semantic similarity)
    - `answer` (stored as metadata)
  - Supports **semantic search** to find the most relevant question

## Bot Workflow
### Step 1: Semantic Router
LLM-based classification of user queries into categories:
- **FAQ**
- **Product (SQL)**

### Step 2: FAQ Flow
1. **User query** → Semantic similarity search against stored FAQs
2. **Matched answer + original query** → Passed to LLM
3. **Final conversational response** generated and displayed in the chatbot

### Step 3: Product Flow
1. **User query** (e.g., "top 3 Nike shoes...") → Passed to LLM
2. **LLM generates SQL query**, for example:

   ```sql
   SELECT * FROM products
   WHERE brand LIKE '%nike%'
     AND avg_rating > 4
     AND total_ratings > 500
   ORDER BY avg_rating DESC
   LIMIT 3;
3. Query is executed on the SQLite database
4. Results + original query → Passed to LLM
5. Final conversational response generated for chatbot display

## Tech Stack
- **Selenium**: Web scraping from Flipkart.
- **Streamlit**: Interactive web interface.
- **ChromaDB**: Semantic search for FAQs.
- **Groq**: SQL query generation from natural language.
- **Pandas**: Data processing and manipulation.
- **SQLite**: Relational database for product data.
- **dotenv**: Environment variable management.
- **Semantic Router**: Query classification (FAQ vs SQL).
- **HuggingFaceEncoder**: Query encoding using pre-trained models.

## LLM Usage Summary
| Stage           | Purpose                                      |
|-----------------|----------------------------------------------|
| **Semantic Routing** | Classify user query type (FAQ vs SQL)   |
| **FAQ Response** | Generate conversational answers             |
| **SQL Generation** | Convert natural language to SQL           |
| **Product Response** | Format query result into a response    |

## Chatbot Features
- FAQ handling via semantic search and LLM for intelligent responses.
- Product querying through SQL generation based on natural language queries.
- Ability to retrieve top-rated products based on different parameters, such as brand and rating.

