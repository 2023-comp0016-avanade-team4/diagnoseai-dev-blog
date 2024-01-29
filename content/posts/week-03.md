---
title: Week 3 - UI's and Backend
date: 2023-12-05
author: George
---

This week marks the start of development, having established the requirements with the client. We focused mainly on building the individual components - UI's and document storage and processing, but also made a start on integration.

# Interfaces
The had a big emphasis on interface implementation for week 3. The chat webapp interface was originally designed using React. We then decided that this was unsuitable because of the nature of the project (communication with the chatting backend). Hence the webapp was transitioned to Next.js and now offers server-side rendering, and more importanty - secure API calls without exposing keys. Below is the current version our uploader interface:

<!-- ![Image of the chat webapp](images/chat_webapp.png) -->

<!-- <img src="images/chat_webapp.png" alt="Image of the chat webapp"> -->

<!-- {{< figure src="/images/chat_webapp.png" title="Chat webapp" >}} -->

[Image of webapp]

The first design of the uploader was also developed this week.

# Blob Storage and Cognitive Search
We decided that 2 Blob Storage containers will be used. The first one called "verification" will only have 1 document in it at a time. The document comes from the uploader and all the text will be extracted for verification purposes. The user can view and inquire about this text, and if the document is approved it will be moved to the "production" containter, where all the approved documents are stored.

[Image of containters]

In order to access our knowledge base we deployed a Cognitive Search model to create an index of all the aproved documents and perform vector search. The embeddings model ada-002 is used to generate all the vector representations of the documents so that they can be added to the index.


# Integration
In terms of integration, this week we prioritised two key areas:

1. Chat Integration: connecting the Next.js frontend to our backend through WebSockets. This approach was favoured because of the reliablity and low latency of WebSockets

2. Uploader and Blob Storage: a crucial connection, enabling the smooth transfer of data from our uploader to blob storage. This integration is vital for data processing, forming a core part of our system's architecture.


# CI
Added on dev blog