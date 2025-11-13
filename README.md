## Design and Implementation of a Multidocument Retrieval Agent Using LlamaIndex

### AIM:
To design and implement a multidocument retrieval agent using LlamaIndex to extract and synthesize information from multiple research articles, and to evaluate its performance by testing it with diverse queries, analyzing its ability to deliver concise, relevant, and accurate responses.

### PROBLEM STATEMENT:
The challenge is to develop an agent that can efficiently retrieve and synthesize information from a large corpus of documents, ensuring that it answers queries with precision and relevance, leveraging LlamaIndex for effective retrieval and summarization.



### DESIGN STEPS:
STEP 1: Load PDFs
Load and register multiple Socail media PDF documents into the system.

STEP 2: Generate tools (Vector + Summary)
For each PDF, create a vector retriever tool which searches relevant paragraphs and a summary tool that summarizes extracted content.

STEP 3: Initialize LLM Agent
Create a function-calling agent using LlamaIndex so the model can automatically decide which PDF/tool to use based on the query.

STEP 4: User Query
Send a social media-related query (e.g., trends, threats, best practices). Agent retrieves data from the PDFs → synthesizes answer.

STEP 5: Display Output
The system prints the response generated using the research papers.



### PROGRAM:
```
from helper import get_openai_api_key
OPENAI_API_KEY = get_openai_api_key()

import nest_asyncio
nest_asyncio.apply()

urls = [
    "https://openreview.net/pdf?id=VtmBAGCN7o",
    "https://openreview.net/pdf?id=6PmJoRfdaK",
    "https://openreview.net/pdf?id=hSyW5go0v8",
]

papers = [
    "pdf0.pdf",
    "pdf1.pdf",
    "pdf2.pdf",
]

from utils import get_doc_tools
from pathlib import Path

paper_to_tools_dict = {}
for paper in papers:
    print(f"Getting tools for paper: {paper}")
    vector_tool, summary_tool = get_doc_tools(paper, Path(paper).stem)
    paper_to_tools_dict[paper] = [vector_tool, summary_tool]

initial_tools = [t for paper in papers for t in paper_to_tools_dict[paper]]

from llama_index.llms.openai import OpenAI

llm = OpenAI(model="gpt-3.5-turbo")

from llama_index.core.agent import FunctionCallingAgentWorker
from llama_index.core.agent import AgentRunner

agent_worker = FunctionCallingAgentWorker.from_tools(
    initial_tools, 
    llm=llm, 
    verbose=True
)
agent = AgentRunner(agent_worker)

response = agent.query("Benefits of Digital Marketing:")
print(str(response))

```

### OUTPUT:
<img width="1065" height="634" alt="image" src="https://github.com/user-attachments/assets/9f4c517a-f8ac-4892-b261-9a0f05d2b56b" />
<img width="1080" height="338" alt="image" src="https://github.com/user-attachments/assets/64701c00-9326-4033-bf11-4f8231182133" />



### RESULT:
Hence to design and implement a multidocument retrieval agent using LlamaIndex to extract and synthesize information from multiple research articles, and to evaluate its performance by testing it with diverse queries, analyzing its ability to deliver concise, relevant, and accurate responses is verified.
