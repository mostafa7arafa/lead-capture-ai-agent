# Lead Capture AI Agent

This project is built on the Flowise platform, which provides a drag-and-drop interface to create AI agents using LangChain. The AI agent, named **Sammy**, is a helpful assistant for a company called **CloudSync**. The bot is designed to capture leads and integrate the information with Zoho CRM through a webhook.

## Overview

The AI chatbot has access to a knowledge base (company documents) and can answer questions about **CloudSync**'s services and products in a professional and concise manner. After engaging with the user, it collects lead information such as:

- First name
- Last name
- Email
- Business name
- Phone number

Once the lead is captured, the assistant uses a custom tool that triggers a webhook to send the lead data to Zoho CRM.

## Features

- **OpenAI Assistant:** AI-powered chatbot that can answer customer queries about CloudSync.
- **Lead Capture:** Collects leads during conversations and sends them to Zoho CRM.
- **Zoho CRM Integration:** Uses a webhook (via Make platform) to integrate with Zoho CRM and upsert leads.
- **Custom Webhook Tool:** The assistant calls a custom tool `add_lead` to send the data via a webhook.

## How It Works

1. **Flowise Platform:** The AI agent is created using Flowise's visual drag-and-drop interface. 
2. **Assistant Instruction:** 
    - The AI assistant, Sammy, follows a script where it answers customer queries and tries to capture leads.
3. **Zoho CRM Integration:** 
    - The captured lead data is passed to a webhook, which then updates Zoho CRM using the following custom tool:
    ```javascript
    const fetch = require('node-fetch');
    const url = 'https://hook.eu2.make.com/l2wjrf7b5negdcc6u87ufghww92uebgl';
    const options = {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          'firstname': `${$firstname}`,
          'lastname': `${$lastname}`,
          'email': `${$email}`,
          'phone': `${$phone_number}`,
          'business_name': `${$business_name}`
        })
    };
    try {
        const response = await fetch(url, options);
        const text = await response.text();
        return text;
    } catch (error) {
        console.error(error);
        return '';
    }
    ```
4. **Webhook Integration:**
    - The webhook is configured in [Make platform](https://www.make.com/) to handle the information and forward it to Zoho CRM.

## Setup

Since this project uses Flowise, no actual code needs to be stored. However, here’s how you can set it up:

1. **Flowise Setup:**
   - Use Flowise to drag and drop the components for creating the chatbot and connecting it to OpenAI.
   - Define the assistant's behavior and connect it to the company knowledge base (documents).
   
2. **Custom Tool (Add Lead):**
   - Add the provided custom tool code in Flowise to pass the lead data to the webhook.

3. **Zoho CRM Integration via Webhook:**
   - Set up a scenario in the Make platform that listens to the webhook and upserts the lead data into Zoho CRM.

## Technologies Used

- **Flowise:** Drag-and-drop platform for building AI agents using LangChain.
- **OpenAI:** For creating an assistant that handles customer queries.
- **Make.com (Webhook):** To connect the captured lead data to Zoho CRM.
- **Zoho CRM:** CRM used to store captured leads.
- **Node.js (Custom Tool):** Used in Flowise to send data to the webhook.

## How to Use

- Once set up, the AI chatbot can be embedded into a company website as a lead generation tool.
- Customers interacting with the bot will be asked for relevant information, which will be forwarded to Zoho CRM.
