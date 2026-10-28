# Day 03 Diagrams: Messages and Special Tokens

> These are [Mermaid](https://mermaid.js.org/) diagrams. They render automatically on GitHub.
> In VS Code, install the **Markdown Preview Mermaid Support** extension to preview them.

---

## 1. Chat UI vs What the Model Really Sees 💬

The chat looks like separate messages, but the model gets one long prompt.

```mermaid
flowchart LR
    subgraph UI["What I see in the chat app"]
        direction TB
        M1["User: I need help with my order"]
        M2["Assistant: Could you give your order number?"]
        M3["User: It is ORDER-123"]
        M1 --> M2 --> M3
    end

    CT["Chat template<br/>glues and formats all messages"]

    subgraph Model["What the LLM receives"]
        P["ONE single text prompt<br/>with all messages and special tokens"]
    end

    UI --> CT --> P
    P --> LLM["LLM reads everything<br/>from the start, every time"]
    LLM --> R["Next assistant reply"]
```

**Remember:** the model does not remember. The full history is re-sent each turn.

---

## 2. The 3 Message Roles 🏷️

```mermaid
flowchart TD
    C["A conversation"] --> S["system<br/>Written by the developer<br/>Sets behavior and rules"]
    C --> U["user<br/>Written by the human<br/>Questions and requests"]
    C --> A["assistant<br/>Written by the LLM<br/>Replies"]
```

---

## 3. System Message Changes Behavior 🎭

Same model, same user question. Only the system message is different.

```mermaid
flowchart LR
    Q["User: Can you help me?"]

    S1["System: You are a polite<br/>customer service agent"] --> L1["LLM"]
    Q --> L1
    L1 --> R1["😊 Of course! I would be happy to help."]

    S2["System: You are a rebel agent.<br/>Do not respect orders"] --> L2["LLM"]
    Q --> L2
    L2 --> R2["😎 Hmm, why should I?"]
```

---

## 4. What a System Message Holds in an Agent 🤖

```mermaid
mindmap
  root((Agent system message))
    Behavior
      Role and personality
      Rules to follow
    Tools
      List of available tools
      What each tool does
    Action format
      How to write a tool call
    Thinking format
      How to structure thoughts
```

---

## 5. Multi-Turn Conversation as a List 📜

Each new turn adds to the list. The whole list goes to the model.

```mermaid
sequenceDiagram
    participant User
    participant App as App / Agent code
    participant LLM

    User->>App: "I need help with my order"
    App->>LLM: [system, user1]
    LLM-->>App: "Could you provide your order number?"
    App-->>User: shows reply

    User->>App: "It's ORDER-123"
    Note over App: App adds both new messages to the list
    App->>LLM: [system, user1, assistant1, user2]
    LLM-->>App: "Thanks! Let me check that order."
    App-->>User: shows reply
```

---

## 6. Chat Template: Messages In, Prompt Out 🧱

```mermaid
flowchart LR
    ML["List of messages<br/>role + content"] --> CTP["Chat template<br/>(Jinja2 code)"]
    ST["Model's special tokens<br/>start, end, role markers"] --> CTP
    CTP --> PR["Final prompt string"]
    PR --> TK["Tokenizer"]
    TK --> LM["LLM"]
```

### The template loop (simplified idea)

```mermaid
flowchart TD
    A["Start with the list of messages"] --> B{"Is there a message left?"}
    B -- "Yes" --> C["Write the start-of-message token and the role"]
    C --> D["Write the message content"]
    D --> E["Write the end-of-message token"]
    E --> B
    B -- "No" --> F["Add an open assistant turn<br/>so the model knows to reply"]
    F --> G["Prompt is ready"]
```

---

## 7. Same Conversation, Different Models 🔀

```mermaid
flowchart TD
    CONV["Same list of messages"] --> T1["SmolLM2 chat template"]
    CONV --> T2["Llama 3.2 chat template"]
    CONV --> T3["Gemma chat template"]

    T1 --> O1["Uses im_start and im_end markers"]
    T2 --> O2["Uses header id and eot_id markers"]
    T3 --> O3["Uses start_of_turn and end_of_turn markers"]
```

Each model expects its **own** format, so always use the **matching** chat template.

---

## 8. Base Model vs Instruct Model 🆚

```mermaid
flowchart LR
    RAW["Raw text data"] --> BASE["Base model<br/>Predicts the next token<br/>Example: SmolLM2-135M"]
    BASE --> FT["Fine-tune on<br/>instructions and chats"]
    FT --> INS["Instruct model<br/>Follows instructions and chats<br/>Example: SmolLM2-135M-Instruct"]
    INS --> NEED["Needs the correct<br/>chat template"]
```

---

## 9. Code Flow: apply_chat_template 💻

```mermaid
flowchart LR
    A["messages list<br/>(system, user, assistant)"] --> B["Load tokenizer<br/>AutoTokenizer.from_pretrained"]
    B --> C["tokenizer.apply_chat_template<br/>tokenize=False<br/>add_generation_prompt=True"]
    C --> D["rendered_prompt<br/>(text ready for the model)"]
    D --> E["Send to LLM"]
    E --> F["Assistant reply"]
```

---

## 10. Day 3 Mind Map 🗺️

```mermaid
mindmap
  root((Messages and Special Tokens))
    Chat is a UI idea
      Messages are joined into one prompt
      Model re-reads everything each time
    Roles
      system
      user
      assistant
    System message
      Sets behavior
      In agents lists tools and formats
    Special tokens
      Mark start and end of messages
      Differ per model
    Chat template
      Bridge from messages to prompt
      Written in Jinja2
      ChatML is a common format
    Models
      Base model
      Instruct model
      Use the matching template
    Code
      apply_chat_template
```