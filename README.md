## n8n-Human-in-the-Loop-Automation

#### Open nodes panel e click koro--->search & click: webhook--->HTTP Method: POST--->Path: draft--->copy koro: Test URL--->paste koro: index.html--->index.html file browser e open koro--->Webhook er Listen for Test Even click koro--->index.html file browser open hole data diye Generate Draft button click koro.

#### Webhook er +sign click koro--->search & click: open ai--->click: Message a model--->Model daw--->Role daw: System--->Prompt daw: prompt.txt theke paste kore daw.

#### Message a model er +sign click koro--->search & click: wait--->Resume daw: On Form Submitted--->Form Title e daw: [for ex:Request for meeting]--->Form Description e daw: [for ex: Please review this draft.]--->click Add Form Element--->Field Name: webhook theke topic drag & drop koro--->Element Type: Redio buttons--->Redio buttons Approve ar reject set koro.

#### Wait node er +sign click koro--->search & click: if---> Conditions e daw: wait node er CS Hackathon k drag & drop koro--->arekta daw: Approve--->

#### if er true er +sign click koro--->search & click: gmail--->click: send a message--->value set koro.--->send a message er name chenge kore Approve daw
#### if er false er +sign click koro--->search & click: gmail--->click: send a message--->value set koro.--->send a message er name chenge kore Reject daw
