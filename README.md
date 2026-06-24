# Anti-Money Laundering (AML) & Circular Fraud Prevention with BigQuery GraphRAG

This repository contains a notebook focusing on **Anti-Money Laundering (AML) & Circular Fraud prevention with BigQuery Graph Retrieval-Augmented Generation (GraphRAG)**.

## The Business Challenge
Businesses face significant challenges from financial crimes like money laundering and circular fraud, leading to substantial financial losses, regulatory penalties, and reputational damage. Combating money laundering is hampered by legacy systems that generate numerous false positives, burdening compliance teams and obscuring truly illicit activities within an estimated $2 trillion global flow. Meanwhile, circular fraud schemes are particularly difficult to detect because they involve complex, closed loops of transactions that appear legitimate individually, requiring advanced analytical techniques, often leveraging graph-based analysis, to uncover the hidden networks and patterns of deception.

## The BigQuery Graph Analytics Solution
A modern data platform like BigQuery with native graph capabilities and vector search solves money laundering and circular fraud challenges by performing GraphRAG (Graph Retrieval-Augmented Generation). Traditional platforms perform keyword or purely semantic searches, missing complex multi-hop financial trails. An integrated graph paradigm detects illicit activity via three mechanics:

* **Multi-Hop Traversal (Zero-ETL)**: Rather than duplicating data or performing resource-heavy relational JOIN operations, a property graph creates a semantic layer over structured data. It traverses cyclic or deep layers (e.g., entity A -> entity B -> entity C -> entity A) to spot circular loops instantly.
* **Hybrid Retrieval (Graph + Vector Search)**: Unstructured logs (e.g., Know Your Customer (KYC) audit flags, suspicious notes) are converted into vector embeddings. Vector search identifies the start node semantics, and Graph Queries (GQL) pull the topology of interconnected accounts.
* **False Positive Reduction**: By passing relationship structures and transaction magnitudes to LLMs, reasoning frameworks discern between high-risk cycles and normal behaviors (e.g., small monetary gifts vs. high-volume shell routing).

## Scenario: Uncovering a Money Laundering Ring
A bank is investigating a series of suspicious activities involving several individuals:
* **Doe**: Owner of a suspected shell company involved in offshore fund routing.
* **Jacoby**: Under audit for high-volume suspicious transfers from business accounts.
* **Menville**: A retail customer whose account recently failed KYC verification and has a large loan repayment due.
* **Smith**: A regular retail customer who occasionally transfers small amounts to friends (the "Innocent Bystander").

**The Mystery:** Menville's account is failing KYC checks. On the surface, it looks like a simple compliance issue. However, the transaction graph reveals a complex money trail: `Doe -> Jacoby -> Menville -> Loan`.

**The Challenge:** Menville also received a small transfer of $150 from **Smith**. In a standard investigation, simple association might flag everyone. GraphRAG allows the LLM to see the entire context—the size of the transfers and the audit logs of the senders—to conclude that while Smith is part of the graph, he is not part of the crime.

## High-Level Flow
1. **Consolidate Data:** Create a BigQuery dataset and schema to consolidate data across customers, accounts, and loans. Create unstructured data to track audits. 
2. **Build the Graph:** Create a Property Graph in BigQuery connecting these entities using native BigQuery Graph DDL.
3. **Graph Exploration:** Visualize customers, accounts, and loans using graph queries to identify multi-hop relationships.
4. **GraphRAG Retriever:** Generate vector embedding for audit logs. Run Vector Search and GQL traversal queries to pull the audit logs for every entity found in the path, providing the LLM with a 360° view of the syndicate.
5. **LLM Reasoning:** Execute LLM chains to identify the fraudsters and eliminate false positives, accurately pointing out the risk to the loan repayment while ignoring benign connections.

## Getting Started
Open the [`graph_rag_aml_with_bigquery.ipynb`](graph_rag_aml_with_bigquery.ipynb) notebook to run the end-to-end solution.

### Prerequisites
* A Google Cloud Project
* BigQuery and Vertex AI enabled
* In the notebook, update the `[GCP PROJECT ID]` placeholder with your actual Google Cloud Project ID.

## References
* [BigQuery Property Graphs Overview](https://cloud.google.com/bigquery/docs/graph-overview)
* [BigQuery Graph Query Language (GQL) Overview](https://cloud.google.com/bigquery/docs/reference/standard-sql/graph-intro)
* [BigQuery ML AI.EMBED Function](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-ai-embed)
* [BigQuery Vector Search and Distance Functions](https://cloud.google.com/bigquery/docs/reference/standard-sql/mathematical_functions#cosine_distance)
* [LangChain Google Integrations](https://python.langchain.com/docs/integrations/platforms/google)

## License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.
