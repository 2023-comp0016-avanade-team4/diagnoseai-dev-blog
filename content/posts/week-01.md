---
title: Week 1 - Architecture Design
date: 2023-11-13T23:38:32+00:00
author: James
---

This week (Monday was 13 November 2023) was reading week, which
consisted of the deadline for our Human Centered Interface (HCI)
design slides.

# HCI Deliverable

After iterating our sketches and prototype with our client, they were
fairly happy with the final interactive prototype, which we
incorporated into our slides.

The aforementioned slides can be found via [this link](/diagnoseai-dev-blog/Report.pdf). Here is
a small screenshot of it:

![image of first slide of diagnoseai](images/2023-11-27_23-51.png)

# Proof of Concept code

To demonstrate the feasibility of this project, we whipped up a script
that does the following things:

1. Gets triggered by an upload to a Blob storage, via Azure Function Apps.
2. Parses a document, and extracts the text (using [Document Intelligence](https://azure.microsoft.com/en-gb/products/ai-services/ai-document-intelligence));
3. Sends the text to a text embedding model, such as
   `text-embedding-ada-002` to get embeddings;
4. Store the embeddings into a vector store, such as [Azure AI Search](https://azure.microsoft.com/en-gb/products/ai-services/ai-search).

Then, we configured Azure AI Search as a data source to an Azure
OpenAI deployment, which, for the purposes of demonstration, was
`gpt-35-turbo`.

The results were as expected; the model was able to support its
answers with phrases quoted from the text, and even provided
references based on the uploaded documents:

![image of demo response](images/demo-poc-1.jpg)

The code for this is available in [commit 7910e76 (requires
authorization)](https://github.com/2023-comp0016-avanade-team4/diagnoseai-core/blob/7910e761ddbae4e76b7115ed9ce519730f93787a/function_app.py).

# GitHub

We set up a GitHub organisation account containing our code (and this
blog), and shared it with our IXN mentor.

The GitHub organisation can be found via [this
link](https://github.com/orgs/2023-comp0016-avanade-team4).

We also share a Kanban board, which can be found
[here](https://github.com/orgs/2023-comp0016-avanade-team4/projects/2).

# Next week

We decided to start drawing up some requirements, and create an
overall systems architecture diagram to better communicate what we
want to achieve by the end of the project.
