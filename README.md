### VAA_Explain

VAA_Explain is an tool that helps VAA(Voting Advice Applicaiton) Creators to explain VAA statements in more simple and easy to understand language. It uses RAG to find relevant information from the provided documents , then an LLM drafts what each statement means and what agree or disagreeing would mean. The creators can review and edit the explanations before using them.

## Features

- RAG Retrieval: Find relevant passages from the provided sources.

- LLM-Generated Explainations: Draft three sections for each statment:
1. What the statement means
2. What agreeing supports
3. What disagreeing rejects

- Source References - include passages ID so VAA users can verify the supporting text.

## Example Results

```text
STATEMENT: Britain should become a member of the European Union customs union again.
MEANING: The statement proposes that Britain should rejoin the European Union customs union, which would involve sharing a common external tariff on imports from outside the union. A customs union, as defined by the sources, is an arrangement where member countries agree on trade concessions and apply a unified external tariff on goods imported from non-member countries.
AGREE: Agreeing to Britain rejoining the EU customs union would involve adopting a shared external tariff on imports from outside the union. One possible benefit is that trade among members could be easier due to reduced complexity in origin rules for goods from third countries [C2].
DISAGREE: The proposal would require Britain to adopt a common external tariff on imports, which could limit its ability to set its own trade policies [C1]. A customs union does not include the deeper integration of standards and movement of people and services found in the single market [C4].
```

## Architecture

1. Input: Read the corpus, references and VAA statments from text files
2. Chunking: Split the corpus into smaller passages while keeping source IDs.
3. Embeddings: Use BAAI/bge-small-en-v1.5 to convert the passages and each statements to vectors.
4. Retrieval: Compare the vectors and select the three most relevant passages for each statements. Embeddings are stored in  memory, so no vector database is needed. (Recommend to use vector database for bigger datasets)
5. Generation: Give the statement and retrieved passages to Qwen3-8b, loaded in 4-bit mode to reduce GPU usages, to draft the meaning, agree and disagree sections.
6. Output: Display the drafts and save them with their source passages as JSON for reviews.

## How to Use

1. Open `VAA_Creator_Demo.ipynb` in Jupyter Notebook, VS Code, or Google Colab. The current generation setup requires a compatible NVIDIA GPU.
2. Replace the placeholders in the example files with your source text, references, and VAA statements.
3. Keep the three text files in the same working folder.
4. If you are using Google Colab, run the first code cell to install dependencies. Otherwise, install them using `python -m pip install -r requirements.txt` and skip the first cell. Then run the remaining cells in order.
5. Review the drafts and run the final cell to export them as `vaa_drafts.json`.