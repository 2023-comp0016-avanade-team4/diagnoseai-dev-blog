---
title: Christmas Blog - Conversation history and validation chat 
date: 2023-12-20
author: Aadhik
---

This Christmas break (Starting from 21st December 2023 to 9th January 2024) the team had a much lighter workload during the holidays. However, time was still spent working on these core functionalities including development of a validation chat user interface, enabling conversation history for client chat, and , creation of an Image vectorization endpoint. 

# Validation Chat User Interface 

Having developed the document upload and vectorization features the focus was now on developing the validation chat user interface. The validation chat is a part of Uploader web app and is used to validate the language model's understanding of the manuals uploaded.

The interface consists of validation chat message interface and two display components rendering the extracted images and text from the document uploaded. Here's a screenshot of the validation chat: 

![Image of the validation chat user interface](images/validation-chat.jpeg)

# Conversation History

In an on field situation, there are multiple factors that a language model will need to keep in mind when suggesting solutions to the problems at hand. Consequently, it is important that the language model is capable of accessing the conversation history. 

After a discussion with the IXN project mentor to better understand the requirements, the team settled on using a conversation history window of ten messages as this was suited to the client requirements. 

Here's a screenshot of the client chat accessing the conversation history. 

![Image of the client chat accessing conversation history](images/client-conv-history.jpeg)

# Image Vectorization

Another important aspect of the validation chat is the ability to vectorize the extracted images. Hence, a core API feature had to be developed to accept images and upload them into a specified vector index. 

To achieve this the image was sent to a GPT4-V vision model which generated a text summary of the image. The model was prompted to create a summary that could later be used for image based content retrieval. Here's a screenshot of the standard request contents to the vision model. 

![Image of the request body being sent to the language model](images/vision-model-prompt.png)

Further, the text generated was vectorized and its embedding was uploaded to the Azure cognitive search index specified in the request body.

