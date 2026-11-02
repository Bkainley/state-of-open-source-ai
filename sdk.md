Software Development Toolkits
=============================

{term}`LLM` software development kits (SDKs) and frameworks help developers integrate generative AI capabilities into applications. They commonly provide model-provider interfaces, reusable components, integrations, examples, and documentation. Their scope varies: some focus on application orchestration, some on data ingestion and retrieval, and others on routing requests across model providers.

An SDK or framework can reduce boilerplate, but it does not automatically ensure legal compliance, security, reliability, or production readiness. These depend on implementation, provider terms, data handling, testing, and operational controls.

.. _llm-sdks:

Comparison of LLM SDKs
----------------------

.. list-table::
   :header-rows: 1
   :widths: 15 24 22 22 22 14 30

   * - SDK / framework
     - Typical use cases
     - Data and retrieval
     - Model support
     - Languages
     - Primary strengths
     - Considerations
   * - `LangChain <https://www.langchain.com/>`_
     - Tool-using agents and LLM applications
     - Integrations with document loaders, retrievers, and vector stores
     - Broad model-provider integrations
     - Python, JavaScript / TypeScript
     - Fast application development, model and tool abstractions, middleware
     - For complex stateful workflows, consider LangGraph; abstraction choices should be evaluated against application needs
   * - `LlamaIndex <https://www.llamaindex.ai/>`_
     - Data-centric LLM applications, RAG, search, and agents over private or domain-specific data
     - Data connectors, indexing, retrieval, reranking, and query workflows
     - Multiple providers through integrations and configurable model interfaces
     - Python, TypeScript
     - Strong focus on data ingestion, retrieval, and context construction
     - Retrieval quality and production behavior depend on data preparation, evaluation, and deployment design
   * - `LiteLLM <https://www.litellm.ai/>`_
     - Unified access to model providers; routing and gateway operations
     - Embedding endpoints and provider integrations; not primarily a RAG framework
     - Broad provider coverage through a unified interface
     - Python SDK; OpenAI-compatible gateway usable by many client languages
     - Common API shape, retries, fallbacks, routing, budgets, and gateway controls
     - Provider feature parity is not guaranteed; verify parameters and behavior for each model
   * - `Haystack <https://haystack.deepset.ai/>`_
     - Modular RAG pipelines, search applications, and agents
     - Explicit components and pipelines for document stores, retrieval, processing, and generation
     - Integrations with hosted and local model providers
     - Python
     - Composable, inspectable pipelines with a clear distinction between deterministic pipelines and agentic execution
     - Requires designing and wiring components to fit the application's use case

.. seealso::

   `Hugging Face Transformers — Awesome Transformers <https://github.com/huggingface/transformers/blob/main/awesome-transformers.md>`_

Why use LLM SDKs and frameworks?
--------------------------------

LLM SDKs and frameworks can provide several benefits:

1. **Less boilerplate and improved developer experience:** Common interactions with models, prompts, tools, and data sources can be implemented through reusable interfaces.
2. **Flexibility:** Applications can combine providers and components, and may replace individual integrations as requirements change.
3. **Observability and debugging:** Supported tracing, logging, and evaluation integrations can help developers inspect requests and application behavior. Logging prompts or responses requires careful handling of sensitive data.
4. **Security controls:** Some frameworks and gateways support authentication, access policies, guardrails, or budget controls. These features must be configured and reviewed; a framework alone does not secure an application.
5. **Collaboration and maintainability:** Standardized components and documented workflows can make systems easier for teams to understand, test, and maintain.
6. **Operational support:** Depending on the tool, features such as retries, streaming, routing, and fallbacks can help address common integration concerns.

The choice of toolkit should be based on the requirements of the application: data and retrieval needs, model-provider flexibility, control over execution, language support, deployment constraints, observability, and maintenance.

LangChain
---------

.. image:: https://docs.langchain.com/img/brand/wordmark.png
   :alt: LangChain logo
   :align: center

`LangChain <https://www.langchain.com/>`_ is an open-source framework for building LLM applications and agents. It provides model and tool abstractions, integrations, a prebuilt agent loop, and middleware for extending agent behavior. LangChain is available for Python and JavaScript / TypeScript.

For custom, stateful workflows that need more direct control over execution, `LangGraph <https://www.langchain.com/langgraph>`_ provides lower-level orchestration primitives. `LangSmith <https://www.langchain.com/langsmith>`_ offers tools for tracing, testing, evaluating, and monitoring applications.

### Models and provider integrations

LangChain offers interfaces for working with chat models and other model types, along with integrations for a range of providers. Model capabilities and supported parameters vary by provider and model. Consult the `LangChain documentation <https://docs.langchain.com/>`_ and the relevant integration reference for current details.

### Retrieval and document processing

Applications can load and transform documents, split them into chunks, generate embeddings, and retrieve relevant context from supported vector stores or other retrievers. These components are often used to build retrieval-augmented generation (RAG) systems. Retrieval quality depends on factors such as source quality, chunking, embedding choice, indexing, and retrieval configuration.

See the current documentation for `document loaders <https://docs.langchain.com/oss/python/integrations/document_loaders>`_ and `vector stores <https://docs.langchain.com/oss/python/integrations/vectorstores>`_.

### Tools and agents

Tools expose functions or services that an agent can call, such as searching a database or invoking an API. LangChain's `create_agent` provides a prebuilt agent loop that combines a model with tools. Middleware can be used to add application-specific behavior such as human review, context management, or guardrails.

Agents can choose actions dynamically, so their behavior, latency, and cost may vary. For tasks requiring a known sequence of steps, a deterministic workflow may be easier to test and control.

### Prompting and structured output

Prompt templates help construct model inputs from variables and application context. Structured-output features can guide supported models to return data in a specified schema. Prompts and output schemas should be validated and tested; they do not guarantee correctness.

### Advanced orchestration

LangGraph is designed for workflows involving state, branching, persistence, streaming, retries, and human-in-the-loop checkpoints. It can combine deterministic steps with model-driven actions. This is useful when a standard agent loop does not provide sufficient control.

LangChain and its ecosystem can accelerate development, but they do not by themselves guarantee scalability, low latency, or production reliability. These properties require application-level testing, deployment design, resource management, and monitoring.

LlamaIndex
----------

.. image:: https://static.premai.io/book/sdk-llama-index.jpg
   :alt: LlamaIndex banner
   :align: center

`LlamaIndex <https://www.llamaindex.ai/>`_ is a data framework for building LLM applications over private, enterprise, or domain-specific data. Its ecosystem focuses on connecting data sources, preparing and indexing data, retrieving relevant context, and building query, workflow, and agent-based applications. It provides high-level interfaces as well as lower-level components for customization.

Refer to the `LlamaIndex documentation <https://docs.llamaindex.ai/>`_ for current packages, APIs, and examples. The project ecosystem and package organization evolve; use current documentation rather than relying on older import paths or examples.

### Data connectors and ingestion

Data connectors (often called readers) bring data into an application from sources such as files, databases, APIs, and enterprise systems. Ingestion workflows may transform documents, extract metadata, split content into nodes or chunks, and send the resulting data to storage or indexes.

A connector is not necessarily an ETL system in the broad sense: its exact responsibilities depend on the integration and configured ingestion pipeline.

### Indexing and retrieval

LlamaIndex provides components for organizing data and retrieving relevant context. Common RAG designs include vector retrieval, keyword retrieval, metadata filtering, hybrid retrieval, and reranking. The underlying storage may be a vector database, document store, or another supported system.

The best retrieval design depends on the dataset and query patterns. Evaluate relevance, coverage, latency, and cost using representative questions rather than assuming one index type is universally best.

### Query engines and chat workflows

Query engines combine retrieval and response synthesis to answer questions over data. Chat-oriented workflows add conversation history or other context to support multi-turn interactions. Current LlamaIndex APIs include configurable components and workflows; exact modes and interfaces may vary by release.

### Agents and tools

LlamaIndex agents can use model-driven tool calling to search data or invoke external functions. Tools may read information or perform actions, depending on the permissions granted by the application. External actions should be validated and protected with appropriate authorization, confirmation, and audit controls.

### Customization and integrations

LlamaIndex offers components for customizing ingestion, retrieval, reranking, response synthesis, and workflow logic. It can be integrated into broader application stacks, but the details depend on the relevant integration and version.

LlamaIndex is especially relevant when data ingestion and retrieval are central to the application. It is not inherently “production-ready” merely by being a framework: deployment, security, evaluation, monitoring, and reliability remain application responsibilities.

LiteLLM
--------

`LiteLLM <https://www.litellm.ai/>`_ provides a unified interface for calling many LLM providers. Its Python SDK uses a common request and response shape, while the LiteLLM Proxy (Gateway) provides an OpenAI-compatible API endpoint for applications and teams.

### Unified completions and model access

LiteLLM offers common interfaces for model calls across providers. This can simplify switching providers or integrating multiple models, but supported features and parameters differ. Check the `provider documentation <https://docs.litellm.ai/docs/providers>`_ for model-specific behavior.

### Embeddings and other capabilities

LiteLLM includes interfaces for embedding models and supports numerous providers. Its available capabilities extend beyond the older OpenAI and Azure OpenAI-only description. See the current `embedding documentation <https://docs.litellm.ai/docs/embedding/supported_embedding>`_ and provider references.

### Streaming, routing, and resilience

Streaming responses can be enabled where supported by the selected provider and model. LiteLLM's Router can provide load balancing, retries, and fallback behavior across configured deployments.

### Proxy / Gateway features

The self-hosted gateway can provide centralized model access, virtual keys, budgets and cost tracking, and related operational controls. These capabilities can help organizations manage model usage, but they require secure configuration and operational oversight.

LiteLLM is primarily an interoperability and model-access layer. It is complementary to application and RAG frameworks such as LangChain, LlamaIndex, or Haystack rather than a direct replacement for their orchestration or data-processing capabilities.

Haystack
--------

`Haystack <https://haystack.deepset.ai/>`_ is an open-source Python framework for building search, RAG, and agentic AI applications. Its modular architecture is based on reusable components connected into pipelines.

### Components and pipelines

Haystack components handle tasks such as document conversion, preprocessing, embedding, retrieval, routing, generation, and evaluation. Pipelines are explicit graphs that define how data flows between components. They can include branches, loops, and multiple paths, making application logic inspectable and testable.

### Document stores and retrieval

Haystack integrates with document stores and retrievers for search and RAG use cases. Developers can select and configure components to match the data source, retrieval strategy, and model provider.

### Agents and tools

Haystack also supports agents that use tools and model-driven control flow. A pipeline is generally preferable when the steps are known and predictability matters; an agent is useful when the sequence of actions must be selected dynamically. They can also be combined.

Haystack is a strong candidate for applications that benefit from explicit, modular pipelines and configurable retrieval. As with other frameworks, production suitability depends on the application's evaluation, security, and deployment practices.

Future and other SDKs
---------------------

The LLM tooling ecosystem changes rapidly. New frameworks and provider integrations appear, while APIs, project names, and recommended architectures evolve. Rather than relying on older announcements or speculative roadmaps, compare projects using current documentation, release activity, compatibility, and the needs of the intended application.

Other useful tools include:

- `Hugging Face Transformers <https://huggingface.co/docs/transformers>`_ for loading, training, and running a broad range of transformer models.
- `Haystack <https://haystack.deepset.ai/>`_ for modular search, RAG, and agent pipelines.
- `LangGraph <https://www.langchain.com/langgraph>`_ for stateful, controllable agent orchestration.
- `LiteLLM <https://www.litellm.ai/>`_ for unified provider access and gateway operations.
- `LlamaIndex <https://www.llamaindex.ai/>`_ for data ingestion, retrieval, and context-focused application development.

### Choosing a toolkit

There is no universally best LLM SDK. A practical starting point is:

- Choose **LangChain** for fast development of LLM applications and tool-using agents with broad integrations.
- Choose **LangGraph** when you need explicit control over complex, stateful agent workflows.
- Choose **LlamaIndex** when ingesting and retrieving from domain-specific data is a central requirement.
- Choose **LiteLLM** when you need a common interface, routing, or a gateway across model providers.
- Choose **Haystack** when you want modular, explicit pipelines for search, RAG, and agentic applications.
- Choose **Transformers** when your work centers on model loading, inference, fine-tuning, or the Hugging Face model ecosystem.

These tools can be combined. For example, an application might use LlamaIndex or Haystack for retrieval, LangGraph for workflow control, and LiteLLM for model-provider routing. Prefer the smallest set of abstractions that meets the requirements, and validate the combination with tests and representative workloads.

.. comments