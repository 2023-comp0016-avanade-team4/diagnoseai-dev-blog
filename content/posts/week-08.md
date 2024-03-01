---
title: Week 8 - Chat feature and security improvements, Optimising Cognitive search indices
date: 2024-01-23
author: Aadhik
---

This week (Monday was 22 January 2024) was focused on three key aspects of project development. Improving security of API keys in the webapp, enabling image support for the chat API, and migrating documents from validation vector indices into production.

# API key security 
In the existing webapp, calls to the OpenAI endpoint were being sent directly from the frontend. This meant that they could be accessed easily from the client side. Till now this caused no issues as the app was being developed locally but it had to be improved to allow for the project to become internet deployable. 

Hence, a back-end API endpoint was created using Next.js server side runtime which stores the API keys using a .env file. This is effective at protecting the API keys and enables the web app to be deployed on the internet

Below is an code snippet from the backend sourcecode: 

![Image of the chat connection api code](images/chat-connection-api-code.png)

# Image support for the Chat API 

Although an image summary and vectorisation function had already been developed, it was yet to be integrated into the websocket chat endpoint. This step is necessary to add image support into the webapp chat function.
 
This involves getting an image, saving it into blob storage, converting it to base64 to be processed by the vision model, and then feeding it into the conversation history to be used by the large language model.

Below is an image of the image functionality working in the webapp chat: 
![Image being uploaded to the webapp chat](images/web-app-chat-image.jpeg)

# Migrating validated documents to production index

In the process of uploading documents, each document is assigned its own vector index in the Azure cognitive search. This allows the end-user to validate that the document has been vectorised properly and is fit to be used in the field. 

However, it is space-inefficient to store every document in its own vector index. Hence, once documents are validated they should be migrated into a production index and the validation index needs to be deleted. 

During this week, a HTTP endpoint was developed that can migrate documents from a specified index into the production index and to delete the aforementioned index after. 

Below is a code snippet from the endpoint's source code: 

![Image of code snippet from validation to production endpoint](images/validation-to-production-code.png)

