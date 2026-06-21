
# Customer_Service_Chatbot

An AI-Powered Customer Service Automation System built on self-hosted infrastructure. This system automatically processes inbound customer inquiries, generates context-aware responses using LLMs, and handles data routing efficiently 24/7.

## 🎥 Video Demo
<!-- TIPS: Rekam workflow n8n & UI WhatsApp/Web pakai Loom atau jadikan GIF, lalu masukkan link/file-nya di bawah ini -->
<img width="394" height="848" alt="WhatsAppVideo2026-06-21at08 24 34-ezgif com-video-to-gif-converter" src="https://github.com/user-attachments/assets/890bf363-3b16-4a31-9fbd-214db3d6be06" />

---

## 📝 Project Overview

### ⚠️ The Problem
In traditional customer support, businesses often face bottlenecks due to:
* **Delayed Response Times:** Customers waiting hours for basic inquiries during peak times or outside business hours.
* **High Operational Costs:** Needing multiple human agents just to answer repetitive, frequently asked questions (FAQs).
* **Scattered Data:** Customer interactions and leads are not synchronized instantly into internal databases, leading to missed opportunities.

### 💡 The Solution
This project automates the entire first-line customer support pipeline. By deploying a self-hosted automation engine connected to advanced AI models, the system can:
* Provide **instant, context-aware responses** to customer queries within seconds, 24/7.
* Intelligently route complex issues or specific keywords to distinct business logic pathways.
* Automatically sync structured customer data and interaction logs into backend systems without human intervention.

---

## 🛠️ Technical Specifications

### Tech Stack
* **Automation Engine:** n8n (Self-Hosted via Docker Compose)
* **AI & LLM Integration:** Gemini API (Advanced AI / AI Agent nodes)
* **Gateway/API:** WAHA (WhatsApp HTTP API) / Custom Webhook Gateway
* **Database/Logs:** Vector Data Base Supabase

### System Architecture
Below is the data flow and logic routing implemented within the n8n canvas:

```mermaid
graph TD
    %% Nodes Definition
    A[📱 WhatsApp Message Inbound] -->|Webhook Event| B(WAHA Trigger Node)
    
    %% Filter your chat
    B --> C{Filter Node}
    C -->|Private Message| D(Edit Fields Node)
    C -->|Group Message| Drop[🛑 Drop Message]

    %% This Help Bot More Natural
    D --> E(Send Seen Node)
    E --> F(Wait Node <br> Dynamic Delay)
    F --> G(Start Typing Node)

    %% AI & Database
    G --> H(AI Agent Node)
    I[🧠 Google Gemini Chat Model] <-->|Language Model| H
    J[🗂️ Simple Memory] <-->|Session ID: from| H
    K[📦 Data Product <br> Supabase Vector Store] <-->|Tool: Retrieve Product Specs| H
    L[🧬 Embeddings Google Gemini] -->|Vector Search| K

    %% Output & Error Handling
    H -->|Success| M(Stop Typing Node)
    M --> N(Send Text Message via WAHA)

    H -->|Error| O(Stop Typing 1 Node)
    O --> P(Send Text Message 1: 'Server is unvailable')

    %% Styling
    style A fill:#25D366,stroke:#128C7E,stroke-width:2px,color:#fff
    style H fill:#ff9900,stroke:#cc7a00,stroke-width:2px,color:#000
    style I fill:#4285F4,stroke:#357AE8,stroke-width:2px,color:#fff
    style K fill:#3ecf8e,stroke:#2b9a66,stroke-width:2px,color:#fff
    style P fill:#ea4335,stroke:#c5221f,stroke-width:2px,color:#fff
    style Drop fill:#777,stroke:#555,stroke-width:1px,color:#fff
