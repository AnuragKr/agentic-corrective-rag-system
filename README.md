## Agentic Corrective RAG System with LangGraph
This project implements an Agentic Corrective RAG (Retrieval Augmented Generation) system, drawing inspiration from the [Corrective Retrieval Augmented Generation](https://arxiv.org/pdf/2401.15884)  research paper. The core idea is to enhance traditional RAG systems by incorporating checks and web searches to address common challenges like poor retrieval and lack of relevant information in the knowledge base.

## Project Overview
Traditional RAG systems can suffer from issues like irrelevant or insufficient retrieved documents, leading to out-of-context or hallucinated LLM responses. This project tackles these problems by building an agentic RAG system with a dynamic workflow using LangGraph. The system integrates several key steps, including:

+ Retrieval: Fetching relevant documents from a vector database.
+ Document Grading: An LLM-based grader assesses the relevance of retrieved documents to the user's query.
+ Query Rewriting: If retrieved documents are deemed irrelevant, the user query is rephrased for a better web search.
+ Web Search: Utilizing a web search tool to find external information when internal retrieval is insufficient.
+ Answer Generation: Generating a comprehensive answer using the retrieved (and potentially web-sourced) context.

The system's workflow is visualized in the following diagram:
![](https://i.imgur.com/uhybMhT.png)


And a more detailed agentic workflow:

![](https://i.imgur.com/eV87ZwX.gif)

## Features
+ Corrective Retrieval: Automatically identifies and corrects instances of poor retrieval.
+ Dynamic Workflow: Leverages LangGraph for a flexible and intelligent decision-making process within the RAG pipeline.
+ LLM-powered Grading: Uses a Large Language Model to evaluate the relevance of retrieved documents.
+ Query Optimization: Rewrites queries to improve web search results when necessary.
+ Web Integration: Incorporates external web search (Tavily API) to augment the knowledge base.
+ ChromaDB Integration: Uses Chroma as the vector store for efficient document retrieval.
+ OpenAI Models: Utilizes OpenAI's text-embedding-3-small for embeddings and gpt-4o for LLM tasks.

## Key Components in the Notebook
+ OpenAI Embedding Models: Uses text-embedding-3-small for document embeddings.
+ Data Loading and Chunking: Processes a subset of Wikipedia data, chunks it, and creates Document objects.
+ Vector DB Creation: Initializes and persists a Chroma vector database.
+ Retriever Setup: Configures a similarity with threshold retriever to fetch documents.
+ Query Retrieval Grader: An LLM chain that grades the relevance of retrieved documents (yes or no).
+ QA RAG Chain: A standard RAG chain for generating answers from context.
+ Query Rephraser: An LLM chain to rephrase user queries for web search optimization.
+ Web Search Tool: Integrates TavilySearchResults for web queries.
+ Agentic RAG Components (LangGraph):
 * ```GraphState```: Defines the state of the agent graph.
 * ```retrieve```: Node for retrieving documents from the vector DB.
 * ```grade_documents```: Node for grading retrieved documents.
 * ```rewrite_query```: Node for rewriting the query.
 * ```web_search```: Node for performing web searches.
 * ```generate_answer```: Node for generating the final answer.
 * ```decide_to_generate```: Conditional node to decide between rewriting the query (and performing web search) or generating an answer based on document relevance.
+ Agent Graph Construction: The LangGraph ```StateGraph``` is used to define the nodes and edges, creating the flow of the agent.

## Example Queries and Outputs
The notebook demonstrates the system's behavior with different types of queries:

+ "what is the capital of India?": This query triggers retrieval from the local DB, but since not all retrieved documents are relevant, the system performs a web search and then generates a more comprehensive answer.

```bash
Markdown

The capital city of India is New Delhi. It is located in the north-central part of the country, to the west of the Yamuna River. New Delhi serves as the seat of the central government of India and is part of the National Capital Territory of Delhi.
```

+ "who won the champions league in 2024?": This query is unlikely to find relevant information in the Wikipedia subset, so it triggers a web search to provide an accurate, up-to-date answer.
```bash
Markdown

The winner of the 2024 UEFA Champions League was Real Madrid. They defeated Borussia Dortmund 2-0 in the final match held at Wembley Stadium in London, England, on June 1, 2024.
```

+ "Tell me about India": This query is well-covered by the local Wikipedia data, so the system directly generates an answer based on the retrieved documents without resorting to a web search.
```bash
Markdown

India is a country located in Asia, specifically at the center of South Asia. It is the seventh largest country in the world by area and the largest in South Asia. With a population exceeding 1.2 billion people, India is the second most populous country globally and holds the title of the most populous democracy in the world. The capital city of India is New Delhi.
... (full detailed answer)
```
