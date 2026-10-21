<!-- Source file: langchain_basics.pdf -->

## Page 1

LANGCHAIN BASICS • BEGINNER GUIDE
Page 1 of 6
LANGCHAIN BASICS
A practical beginner’s guide to building LLM-powered applications
What you’ll learn
• What LangChain does and how it fits into an AI application
• Models, prompts, messages, and structured outputs
• Tools and agents using the current create_agent pattern
• Embeddings, vector search, and retrieval-augmented generation (RAG)
• Testing, safety, and a roadmap for practice
Who this is for: Python learners who know basic functions, imports, and dictionaries. The examples focus on LangChain’s
current Python API; package and provider options may evolve.
How to use this guide: read the concept, run the code in a fresh environment, then adapt it to a small project.
Start simple • Verify outputs • Add complexity gradually

## Page 2

LANGCHAIN BASICS • BEGINNER GUIDE
Page 2 of 6
1. What Is LangChain?
LangChain is an open-source framework for building applications powered by large language models (LLMs). It provides
common interfaces and integrations for models, messages, tools, retrieval, and agent workflows, reducing the amount of
provider-specific glue code you write.
Typical application flow
User input → Prompt/context → Chat model → Response
                         I tools or retrieval I
Key terms
LLM/chat model: generates text or structured responses. Prompt: instructions and context sent to a model. Tool: a
function or external capability the model can request. Agent: a loop that lets a model decide whether to call tools and use
their results. Retriever: finds relevant documents for a query.
LangChain vs. related projects
LangChain offers higher-level components and a prebuilt agent pattern. LangGraph provides lower-level orchestration for
stateful workflows and tighter control. LangSmith supports tracing, debugging, evaluation, and monitoring. They are related
but serve different roles.
Install the Python package
python -m venv .venv
# Activate the environment for your operating system
python -m pip install -U langchain
You also need a model provider integration and valid credentials for the provider you choose. Keep API keys in environment
variables or a secret manager; never commit them to source control.
Official docs: https://docs.langchain.com/oss/python/langchain/overview

## Page 3

LANGCHAIN BASICS • BEGINNER GUIDE
Page 3 of 6
2. Models, Prompts, and Responses
A chat model accepts messages and returns a response. LangChain standardizes many provider interfaces, but you still
need the matching integration package and provider credentials. The following example uses the current provider-prefixed
model naming style shown in official documentation.
A minimal model call
from langchain.chat_models import init_chat_model
# Install/configure the provider integration first.
# Example model identifier; choose one available to you.
model = init_chat_model("openai:gpt-4.1-mini")
response = model.invoke("Explain APIs in one sentence.")
print(response.content)
Model names and integration packages change over time. Check the provider’s current LangChain integration guide and
use a model you have access to. A model call may incur cost and send input to the provider.
Use message roles
from langchain.messages import SystemMessage, HumanMessage
messages = [
    SystemMessage("You explain technical ideas simply."),
    HumanMessage("What is a vector database?"),
]
response = model.invoke(messages)
print(response.content)
A system message sets behavior, while a human message represents user input. Model responses are message objects;
their text is commonly available through .content.
Prompt templates
from langchain_core.prompts import ChatPromptTemplate
prompt = ChatPromptTemplate.from_messages([
    ("system", "Explain concepts for a beginner."),
    ("human", "Explain {topic} with one example."),
])
messages = prompt.invoke({"topic": "embeddings"})
response = model.invoke(messages)
print(response.content)
Templates separate reusable instructions from runtime values. Treat user-provided text as data, not trusted instructions, and
validate the output before using it in another system.

## Page 4

LANGCHAIN BASICS • BEGINNER GUIDE
Page 4 of 6
3. Tools and Agents
A tool is a function an agent can call to perform a task, such as looking up an order or doing a calculation. An agent
combines a model, tools, and instructions in a loop: the model may request a tool, the framework executes it, and the result
is returned to the model.
Define a tool and create an agent
from langchain.agents import create_agent
from langchain.tools import tool
@tool
def multiply(a: int, b: int) -> int:
    """Multiply two integers."""
    return a * b
agent = create_agent(
    model="openai:gpt-4.1-mini",
    tools=[multiply],
    system_prompt="You are a concise math assistant.",
)
result = agent.invoke({
    "messages": [{"role": "user",
                  "content": "What is 17 multiplied by 23?"}]
})
print(result["messages"][-1].content)
Install the matching provider integration and configure its API key before running this example. The model identifier is
illustrative; select a currently supported model. Tool use depends on the chosen model’s capabilities.
Design tools carefully
Use clear names, type hints, and descriptive docstrings. Validate arguments and handle expected failures. A tool should do
a well-defined task and return a useful result. Do not expose unrestricted file, shell, database, or network access to an
agent.
Control side effects
For actions that send messages, change records, spend money, or delete data, add authorization and human approval
where appropriate. The model’s decision is not a security boundary. Check permissions in the tool implementation itself.
Official quickstart: https://docs.langchain.com/oss/python/langchain/quickstart

## Page 5

LANGCHAIN BASICS • BEGINNER GUIDE
Page 5 of 6
4. Retrieval-Augmented Generation (RAG)
RAG gives a model relevant external information at answer time. It is useful when answers must reflect private documents,
a changing knowledge base, or material that was not included in the model’s training data. Retrieval can improve grounding,
but does not guarantee correctness.
Typical RAG pipeline
Documents → Split into chunks → Create embeddings
         → Store vectors → Retrieve relevant chunks
Question + retrieved context → Chat model → Answer
Important components
Document loader: reads PDFs, web pages, or files. Text splitter: breaks long content into chunks. Embedding model:
maps text to vectors that capture semantic similarity. Vector store: stores vectors and metadata. Retriever: selects
relevant chunks for a query.
Conceptual retrieval example
# Pseudocode: exact APIs depend on chosen integrations
pages = load_documents("handbook.pdf")
chunks = split_text(pages, chunk_size=800)
vectors = embed(chunks)
vector_store.add(chunks, vectors)
question = "What is the leave policy?"
context = vector_store.search(question, top_k=4)
answer = model.invoke(make_prompt(question, context))
This illustrates the sequence, not a copy-paste API. In a real project, use documented LangChain document loaders, text
splitters, embedding integrations, and vector-store integrations.
RAG quality tips
Choose chunk sizes based on document structure; preserve source metadata; test retrieval using representative questions;
cite the retrieved sources in answers; and tell the model to say when context is insufficient. Watch for prompt injection inside
retrieved documents.
Start with the official tutorials: https://docs.langchain.com/oss/python/learn

## Page 6

LANGCHAIN BASICS • BEGINNER GUIDE
Page 6 of 6
5. Testing, Safety, and Next Steps
Debug systematically
Inspect the exact prompt and retrieved context, check model/provider errors, and test tools independently before connecting
them to an agent. For complex applications, tracing and evaluation tools such as LangSmith can help reveal tool calls,
latency, and failure cases.
A practical evaluation checklist
I Test normal, ambiguous, and adversarial user inputs.
I Check factual accuracy and whether claims are supported by retrieved sources.
I Measure tool-call correctness, failure handling, latency, and cost.
I Test what happens when the model or a tool times out or returns invalid data.
I Protect API keys, personal data, and privileged operations.
Common beginner mistakes
• Installing LangChain without the required provider integration. • Copying examples that use older APIs without checking
the current docs. • Assuming an agent will always choose the right tool or produce valid output. • Sending huge documents
without chunking or retrieval. • Giving tools more permissions than they need.
Mini-project roadmap
1. Build a prompt-based FAQ helper. 2. Add a custom read-only tool. 3. Index a small set of notes and answer questions
with RAG. 4. Add a test set, source citations, and failure handling. 5. Add approval before any tool performs a consequential
action.
Official learning links
Overview: https://docs.langchain.com/oss/python/langchain/overview
Quickstart: https://docs.langchain.com/oss/python/langchain/quickstart
Learning guides: https://docs.langchain.com/oss/python/learn
API reference: https://reference.langchain.com/python/
Remember: start with one model call, then add tools or retrieval only when they solve a real need. Verify current APIs before
using examples in production.