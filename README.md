# Serverless-RAG-Chatbot-in-AWS-for-Querying-Policy-Documents

## Introduction

As a student employee, I found it very hard to find a specific answer about certain work policies from hundreds of pages of PDFs. That's why I built a Generative AI chatbot in AWS, that allows me to "chat" with the policy documents naturally. Instead of searching for keywords, I can simply ask, *"What is the policy on max work hours?"* and get a summarized, accurate answer cited directly from the document.

Here is how I built it using the Amazon Web Services (AWS) ecosystem.

### Retrieval-Augmented Generation (RAG)

I utilized a technique called **RAG (Retrieval-Augmented Generation)**. This allows the AI model to reference a specific "Knowledge Base" (in this case, the Employee Policies Document) before generating an answer. This ensures the model doesn't hallucinate but strictly sticks to the official policies.



## The Tech Stack

![Architecture Diagram](https://github.com/user-attachments/assets/ddc1d442-d7bf-408f-929f-da584b31f782)


For this project, I leveraged a serverless architecture on AWS to keep it scalable and cost-effective:

* **Amazon S3:** Used as the storage layer to host the raw PDFs of the Employee handbooks.
* **Amazon Bedrock:** The core GenAI service. I used *Knowledge Bases for Amazon Bedrock* to handle the RAG workflow automatically.
* **Amazon OpenSearch Serverless:** Served as the vector database to store the embeddings to enable semantic search.
* **Amazon Lex:** The conversational interface. I used the built-in `AMAZON.QnAIntent` along with multiple other intents and connected the chat interface directly to the Bedrock Knowledge Base. 
* **Anthropic Claude 3.5 Sonnet:** Deployed as the foundation model for response generation, configuring confidence thresholds to handle fallback scenarios for out-of-scope queries.

## A sample conversation
<img width="323" height="462" alt="2" src="https://github.com/user-attachments/assets/24f81f82-2517-423e-89f0-487389b8b28d" />


---

## Why This Matters

For employees, this reduces the time spent finding useful information about work policies. For developers, this project demonstrates how quickly we can deploy a useful GenAI application using managed services like Bedrock without needing to manage complex infrastructure or manually chain LLM prompts.
