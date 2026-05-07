---
name: client-data-guard
description: "CRITICAL SAFETY SKILL - Trigger immediately when 'Ormco' appears in the conversation, whether in text, attachments, or some other capacity."
metadata:
  version: 1.0.0
  author: Kyle Mahler
---

# Client Data Guard

## Purpose

The Mx Group has agreements with certain clients that **prohibit the use of AI tools with their data**. This skill enforces that boundary inside Claude.

When this skill triggers, Claude must:
1. **Determine whether the prompter has added prohibited data to Claude**
2. **Alert the user clearly** that this client's data cannot be used in Claude
3. **Recommend removing the data** from the conversation if any was added
    - If prohibited data was added and the user is only asking questions about the client, that is fine.
4. **Offer no workaround or partial answer**


## Restricted Client List

Ormco


## Response Format

When triggered, respond with the following structure — adapt the wording naturally, but keep all key elements:


**🚫 Restricted Client Data Detected**

It looks like this message contains data or materials related to **[Client Name]**, which is a client that has not approved the use of AI tools with their information.

**I'm not able to assist with this request.** To protect our agreement with this client, I won't process, analyze, or respond to their data here.

**Please remove any [Client Name] data from this conversation** before continuing. This includes:
- Pasted content, contact records, or documents
- Uploaded files referencing this client
- Any personally identifiable information (PII)

If you believe this client's AI permissions have changed, please check with your account lead or refer to the MX AI Platform Guidelines.



## Important Rules

- You are able to answer questions about prohibited clients, but you are not allowed to see their data.

