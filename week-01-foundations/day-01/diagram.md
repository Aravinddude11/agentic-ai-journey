# Day 01 Diagrams: What is an Agent?

> These are [Mermaid](https://mermaid.js.org/) diagrams. They render automatically on GitHub.
> In VS Code, install the **Markdown Preview Mermaid Support** extension to preview them.

---

## 1. Aarvi the Agent: Step by Step ☕

How an agent handles a request, using the coffee example.

```mermaid
flowchart TD
    U["🧑 User: 'I would like a coffee, please'"] --> A["🤖 Aarvi understands the request"]
    A --> B["🧠 Reason and plan the steps"]
    B --> P["📋 Plan:<br/>1. Go to kitchen<br/>2. Use coffee machine<br/>3. Brew coffee<br/>4. Bring it back"]
    P --> T["🛠️ Act: use a tool (coffee machine)"]
    T --> R["☕ Coffee is brewed"]
    R --> D["🧑 Deliver coffee to the user"]
```

---

## 2. Brain + Body of an Agent 🧠🦾

An agent is made of a thinking part and an acting part.

```mermaid
flowchart LR
    subgraph Agent
        direction LR
        Brain["🧠 BRAIN<br/>AI Model (LLM)<br/>Reasons and plans"]
        Body["🦾 BODY<br/>Tools and capabilities<br/>Performs actions"]
        Brain -- "decides which action" --> Body
    end
    Goal["🎯 User's goal"] --> Brain
    Body -- "acts on" --> Env["🌍 Environment<br/>(web, files, apps, email)"]
    Env -- "results come back" --> Brain
```

---

## 3. Chatbot vs Agent 💬 vs 🤖

The main difference: a plain LLM only produces text, while an agent can take real actions.

```mermaid
flowchart LR
    subgraph Plain_LLM["💬 Plain LLM (chatbot)"]
        direction LR
        I1["Question"] --> L1["LLM"] --> O1["Text answer only"]
    end

    subgraph Agent_Flow["🤖 Agent"]
        direction LR
        I2["Task"] --> L2["LLM (brain)"]
        L2 --> Tool["🛠️ Tools"]
        Tool --> Real["Real result<br/>(email sent, image made,<br/>web searched)"]
        Real --> L2
        L2 --> O2["Final answer or finished task"]
    end
```

---

## 4. How an LLM Uses a Tool 🔧

The LLM only writes text. The surrounding code is what actually runs the tool.

```mermaid
sequenceDiagram
    participant User
    participant LLM as 🧠 LLM
    participant Code as ⚙️ Agent Code
    participant Tool as 🛠️ send_message_to()

    User->>LLM: "Email my manager to delay today's meeting"
    LLM->>Code: Writes: send_message_to("Manager", "Can we postpone today's meeting?")
    Code->>Tool: Runs the function
    Tool-->>Code: Email sent ✅
    Code-->>LLM: Result of the tool
    LLM-->>User: "Done! I've asked your manager to postpone."
```

---

## 5. Real-World Agent Examples 🌍

```mermaid
mindmap
  root((AI Agents))
    Virtual Assistants
      Siri
      Alexa
      Google Assistant
      Set reminders
      Control smart devices
    Customer Service
      Answer questions
      Troubleshooting
      Open tickets
      Complete transactions
    Game NPCs
      Natural dialogue
      Adapt to the player
      Less scripted behavior
```