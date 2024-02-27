
---
title: Week 11 - Continuing Work Orders
date: 2024-02-19
author: Aadhik
---

This week (Monday was 19 November 2024) was spent continuing the Work we started last week on Work Orders. Further work was needed in developing the Work Orders in the Uploader and Web App pages. 

# Work Orders in Uploader 

During the previous week, the front-end UI for the uploader page was successfully created. However, for Work Orders to be fully implemented onto the Uploader page a few key tasks were stil required: 

- API endpoints to create and delete Work Orders and to fetch the available Machines
- Global State management system to store selected Machine from upload to validation page

Here are a few code snippets from the aforementioned tasks: 

Code snippet of API endpoint to fetch machines:

![Image of the Fetch Machines API code](images/fetch-machines-api.png)

Code snippet of Global Storage with redux: 
![Image of the Global state management code snippet](images/redux-store.png)


# Work Orders in WebApp

Similar to the Uploader page, the front-end for the Work orders was created in the Web App last week. However, more work was needed to fetch and provide the Work Order details to the front end. 

- Creation of a context to provide the chat with work order selection details 
- Creation of an API endpoint to fetch the Work orders

Here are a few code snippets from the aforementioned tasks: 

Code snippet of the API endpoint to fetch Work Orders:
![Image of the WorkOrders endpoint](images/work-order-api.png)

Code snippet of the Work Order context: 
![Image of the Work Order context](images/work-order-context.png)



