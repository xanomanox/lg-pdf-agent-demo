# A Simple LangGraph PDF Agent
## Purpose

This is a simple demonstration of a LangGraph agent to scan local PDF files (such as arXiv publications), extract metadata using a small local LLM via an OpenAI-compatible API, and generate a markdown summary of the papers.

The extracted metadata includes:

- Paper title
- First author
- A one-sentence distillation of the abstract
- Generated tags categorizing the paper

## Dependencies
This project requires the following Python packages:

This project uses the following open-source libraries:
- [LangGraph](https://github.com/langchain-ai/langgraph) (MIT License)
- [LangChain Core](https://github.com/langchain-ai/langchain) (MIT License)
- [OpenAI Python Client](https://github.com/openai/openai-python) (MIT License)
- [PyPDF](https://github.com/py-pdf/pypdf) (BSD-2-Clause License)

The LangGraph agent expects a locally hosted model that serves an OpenAI-compatible API endpoint (e.g., text-generation-webui, Ollama, LM Studio).

## Environment
I built and ran this langgraph demo using VSCode on WSL running Ubuntu 22.04. The model server ran on the same machine.
Hardware: WSL in Windows 11 with two ancient RTX 2080 GPUs (8GB). 
Model: https://huggingface.co/Qwen/Qwen2.5-3B-Instruct-GGUF Q8_0
Model Server: https://github.com/oobabooga/text-generation-webui v3.1
- manual install with conda
- llama.cpp defaults`

## Installation
```
pip install -r requirements.txt
```

## Usage
Ensure your model API server is running and listening for OpenAPI calls on default ports.
- set openai.base to the local server address
- in __main__ set pdf_dir to the starting folder. modify path and related code if you are running any other OS than WSL Ubuntu
```
python lg-pdf-agent.py
```

### Monitor extraction progress
The script prints file processing progress and any warnings about parsing failures.
Watch your model server for langgraph queries.

### Outputs
A papers_summary.md file is created in the working directory.

Each entry in the Markdown file includes:

- Title
- First author
- File shortcut (link to the PDF file)
- A brief summary of the abstract
- Relevant tags

### Example entry

## What In-Context Learning 'Learns' In-Context — Jane Pan
**File:** [2403.12345.pdf](/mnt/nas/arxiv/2403.12345.pdf)  
**Summary:** This paper characterizes two ways through which in-context learning leverages demonstrations...  
**Tags:** in-context learning, task recognition, task learning, large language models, controlled experiments

## Notes
The PDF reader (pypdf) only grabs the first three pages of each PDF where we expect useful metadata to be. This speeds up processing and reduces pressure on context windows, especially for small models running in constrained hardware.

The agent script expects the model to generate strict JSON format, but it doesn't always.  Minor cleaning is applied to strip Markdown code fences if present. Stronger prompting may address this minor problem.

Some PDFs may fail to parse if they are corrupt or improperly formatted. These are safely skipped with a warning.

Folder exclusions can be configured by editing the exclude_paths list inside read_pdfs_node().

This project is licensed under the MIT License.