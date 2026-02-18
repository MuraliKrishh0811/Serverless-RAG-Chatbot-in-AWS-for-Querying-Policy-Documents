# A Simple RAG-Powered Chatbot in AWS using Amazon Lex & Bedrock

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

---

## Prerequisites

Before deploying this architecture, ensure you have the following:
* An active **AWS Account** with administrative privileges.
* Access granted in Amazon Bedrock for the following models:
  * **Anthropic Claude 3.5 Sonnet**
  * **Amazon Titan Text Embeddings G1**

## Setup Instructions

### 1. Document Storage (Amazon S3)
1. Create an Amazon S3 bucket (e.g., `employee-policy-documents-xyz`).
2. Upload the PDF files containing the employee policies or handbooks into the bucket.

### 2. Knowledge Base (Amazon Bedrock & OpenSearch)
1. Navigate to **Amazon Bedrock** -> **Knowledge bases** and click **Create knowledge base**.
2. Point the data source to your newly created S3 bucket.
3. Select **Amazon Titan Text Embeddings G1** as the embedding model.
4. Allow Bedrock to create a new **Amazon OpenSearch Serverless** vector store automatically.
5. Once created, select your data source and click **Sync** to vectorize and index the PDFs. 

### 3. Conversational Interface (Amazon Lex)
1. Navigate to **Amazon Lex** and create a new blank bot.
2. Add the built-in `AMAZON.QnAIntent`.
3. Under the Fulfillment section for this intent, enable the Bedrock integration.
4. Select **Anthropic Claude 3.5 Sonnet** as the generative model and paste your **Knowledge Base ID**.
5. Configure the `FallbackIntent` to handle graceful failures and out-of-scope questions.
6. **Important:** Navigate to IAM and ensure the auto-generated Lex Service Role has the `AmazonBedrockFullAccess` policy (or scoped-down equivalent) attached.
7. Click **Build** and use the test console to verify the bot.

## Usage

Once the Lex bot is built and synced with the Bedrock Knowledge Base, you can test it directly in the AWS Lex console by asking natural language questions. 

