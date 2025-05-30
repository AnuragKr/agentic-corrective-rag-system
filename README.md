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
