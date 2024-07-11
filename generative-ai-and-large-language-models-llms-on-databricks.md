# Generative AI and large language models (LLMs) on Databricks

This article provides an overview of generative AI on Databricks and includes links to example notebooks and demos.

### What is generative AI?

Generative AI is a type of artificial intelligence focused on the ability of computers to use models to create content like images, text, code, and synthetic data.

Generative AI applications are built on top of large language models (LLMs) and foundation models.

* **LLMs** are deep learning models that consume and train on massive datasets to excel in language processing tasks. They create new combinations of text that mimic natural language based on its training data.
* **Foundation models** are large ML models pre-trained with the intention that they are to be fine-tuned for more specific language understanding and generation tasks. These models are utilized to discern patterns within the input data.

After these models have completed their learning processes, together they generate statistically probable outputs when prompted and they can be employed to accomplish various tasks, including:

* Image generation based on existing ones or utilizing the style of one image to modify or create a new one.
* Speech tasks such as transcription, translation, question/answer generation, and interpretation of the intent or meaning of text.

Important

While many LLMs or other generative AI models have safeguards, they can still generate harmful or inaccurate information.

Generative AI has the following design patterns:

* Prompt Engineering: Crafting specialized prompts to guide LLM behavior
* Retrieval Augmented Generation (RAG): Combining an LLM with external knowledge retrieval
* Fine-tuning: Adapting a pre-trained LLM to specific data sets of domains
* Pre-training: Training an LLM from scratch

### Develop generative AI and LLMs on Databricks

Databricks unifies the AI lifecycle from data collection and preparation, to model development and LLMOps, to serving and monitoring. The following features are specifically optimized to facilitate the development of generative AI applications:

* Unity Catalog for governance, discovery, versioning, and access control for data, features, models, and functions.
* MLflow for model development tracking and LLM evaluation.
* Feature engineering and serving.
* Mosaic AI Model Serving for deploying LLMs. You can configure a model serving endpoint specifically for accessing foundation models:
  * State-of-the-art open LLMs using Foundation Model APIs.
  * Third-party models hosted outside of Databricks. See External models in Mosaic AI Model Serving.
* Mosaic AI Vector Search provides a queryable vector database that stores embedding vectors and can be configured to automatically sync to your knowledge base.
* [Lakehouse Monitoring](broken-reference) for data monitoring and tracking model prediction quality and drift using automatic payload logging with inference tables.
* AI Playground for testing foundation models from your Databricks workspace. You can prompt, compare and adjust settings such as system prompt and inference parameters.
* Mosaic AI Model Training (formerly Foundation Model Training) for customizing a foundation model using your own data to optimize its performance for your specific application.

### Additional resources

* See What is Mosaic AI Agent Framework?.
  * See [Build a Q\&A chatbot with LLama2 and Databricks](https://www.databricks.com/resources/demos/tutorials/data-science-and-ai/lakehouse-ai-deploy-your-llm-chatbot).
* For information about using Hugging Face models on Databricks, see Hugging Face Transformers.
* The [databricks-ml-examples](https://github.com/databricks/databricks-ml-examples) repo in Github contains example implementations of state-of-the-art (SOTA) LLMs.

***
