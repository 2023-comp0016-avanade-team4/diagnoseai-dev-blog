---
title: Week 4 - Video
date: 2023-12-12
author: James
---

# Core API

The proof-of-concept code mentioned in [the first
week]({{< ref "week-01" >}}) was used as a reference to write the chat
management component of CoreAPI.

In particular, we implemented the following flow:

1. A WebSocket proxy to the model via Azure WebPubSub Service, which
   triggers a function running on Azure Function App.
2. The Azure Function App connects to the Azure OpenAI model, much
   like what was implemented in Proof of Concept. This means that the
   model has access to Azure Cognitive Search, which is currently
   acting as our vector database.
3. This WebSocket proxy is connected to the front-end UI to provide an
   instant-messaging like interface.

# UIs

While Figma generates good-looking UI, the generated code is
unserviceable. Hence, both the Uploader interfaces and Chat interfaces
were reworked to include original components.

The instant-messaging and uploading functionality were made
functional. The following flow was implemented:

1. Uploading - Files are now uploaded to a container on a Azure
   Storage Account, which is then processed by a Azure Function App.
2. The Azure Function App vectorizes the blob with the
   `text-embedding-ada-002` model, and stores the vectors within Azure
   Cognitive Search.
3. Chatting - When the model is invoked via CoreAPI, the model
   connects to the same Azure Cognitive Search service to obtain
   relevant documentation whenever required.


# Video

A video was created to demo the progress made thus far:

<iframe width="560" height="315" src="https://www.youtube.com/embed/4eF8jrSBjlU?si=Y9TLpTn2iyxHKrkX" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


# Christmas

Over the Christmas break, we will focus on the following components:

1. Validation Chat User Interface
2. User Conversation History (at the moment, it is not stored)
3. Image Vectorization
