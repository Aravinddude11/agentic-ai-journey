# Day 02 Diagrams: What are LLMs?

> These are [Mermaid](https://mermaid.js.org/) diagrams. They render automatically on GitHub.
> In VS Code, install the **Markdown Preview Mermaid Support** extension to preview them.

---

## 1. The 3 Types of Transformers 🧩

```mermaid
flowchart TD
    T["Transformer architecture"] --> E["Encoder<br/>Text in, embedding out"]
    T --> D["Decoder<br/>Generates text token by token"]
    T --> S["Seq2Seq<br/>Encoder + Decoder"]

    E --> E1["Example: BERT"]
    E --> E2["Use: classification, search"]

    D --> D1["Examples: Llama, GPT-4, Gemma"]
    D --> D2["Use: chatbots, code, text generation"]

    S --> S1["Examples: T5, BART"]
    S --> S2["Use: translation, summarizing"]

    D -.-> LLM["Most LLMs are decoders<br/>with billions of parameters"]
```

---

## 2. Tokenization: Words to Tokens ✂️

```mermaid
flowchart LR
    W["Sentence:<br/>'I am interested'"] --> TK["Tokenizer"]
    TK --> T1["I"]
    TK --> T2["am"]
    TK --> T3["interest"]
    TK --> T4["ed"]
    T1 --> ID["Token IDs<br/>(numbers the model reads)"]
    T2 --> ID
    T3 --> ID
    T4 --> ID
```

Small vocabulary of about 32,000 tokens can build all the words we need, because tokens combine: `interest` + `ed` = interested.

---

## 3. Next Token Prediction (One Step) 🔮

What happens each time the model produces one token.

```mermaid
flowchart LR
    A["Input text:<br/>'The capital of France is'"] --> B["Tokenize"]
    B --> C["Model builds a representation<br/>(meaning + position)"]
    C --> D["Score every token<br/>in the vocabulary"]
    D --> E["Decoding strategy<br/>picks one token"]
    E --> F["Next token:<br/>'Paris'"]
```

Example scores for the next token:

```mermaid
xychart-beta
    title "Example: scores for the next token"
    x-axis ["Paris", "Lyon", "London", "Pizza"]
    y-axis "Likelihood (example)" 0 --> 100
    bar [85, 8, 5, 2]
```

---

## 4. The Autoregressive Loop 🔁

The output of each step becomes part of the input for the next step, until the model predicts EOS.

```mermaid
flowchart TD
    S["Start: prompt tokens"] --> P["Model predicts next token"]
    P --> Q{"Is it the EOS token?"}
    Q -- "No" --> R["Add token to the input"]
    R --> P
    Q -- "Yes" --> Z["Stop. Output is complete."]
```

Step-by-step view:

```mermaid
sequenceDiagram
    participant In as Input
    participant M as LLM

    In->>M: The capital of France is
    M-->>In: Paris
    In->>M: The capital of France is Paris
    M-->>In: .
    In->>M: The capital of France is Paris.
    M-->>In: EOS (stop)
```

---

## 5. Decoding Strategies: Greedy vs Beam Search 🎯

```mermaid
flowchart TD
    Start["Scores for all possible next tokens"] --> G["Greedy<br/>Always take the highest score"]
    Start --> B["Beam search<br/>Keep several candidate sequences"]
    G --> G1["Fast and simple"]
    B --> B1["Picks the sequence with the best TOTAL score"]
    B1 --> B2["Even if one token had a lower score"]
```

---

## 6. Attention: Focus on What Matters 👀

For the sentence "The capital of France is ...", some words matter more than others.

```mermaid
flowchart LR
    subgraph Input["Input tokens"]
        W1["The"]
        W2["capital"]
        W3["of"]
        W4["France"]
        W5["is"]
    end

    W2 ==>|"high attention"| P["Predict next token"]
    W4 ==>|"high attention"| P
    W1 -.->|"low"| P
    W3 -.->|"low"| P
    W5 -.->|"low"| P

    P --> R["Paris"]
```

Context length is the maximum number of tokens the model can pay attention to at once:

```mermaid
flowchart LR
    CL["Context window<br/>(max tokens)"] --> I["Your prompt"]
    CL --> H["Conversation history"]
    CL --> O["Model's reply"]
```

---

## 7. Why Prompting Matters ✍️

```mermaid
flowchart LR
    V["Vague prompt:<br/>'Tell me about dogs'"] --> L1["LLM"] --> R1["Random, general answer"]
    C["Clear prompt:<br/>'List 3 dog breeds good for<br/>small apartments, one line each'"] --> L2["LLM"] --> R2["Focused, useful answer"]
```

---

## 8. How LLMs Are Trained 🏋️

```mermaid
flowchart LR
    D1["Huge text data<br/>(books, websites, code)"] --> PT["Pre-training<br/>Learn to predict the next token"]
    PT --> GM["General LLM<br/>Knows language patterns"]
    D2["Task-specific labeled data<br/>(chat, tool use, code)"] --> FT["Fine-tuning<br/>Supervised learning"]
    GM --> FT
    FT --> SM["Specialized LLM<br/>(chat model, tool-calling model)"]
```

---

## 9. How Can I Use LLMs? 💻

```mermaid
flowchart TD
    Q["I want to use an LLM"] --> H{"Do I have strong enough hardware?"}
    H -- "Yes" --> L["Run locally<br/>on my own machine"]
    H -- "No / want it easy" --> A["Use a Cloud / API<br/>(e.g. Hugging Face Inference API)"]
    A --> C["This course mainly uses this option"]
```

---

## 10. LLM as the Brain of an Agent 🤖

```mermaid
flowchart LR
    U["🧑 User request"] --> L["🧠 LLM (the brain)"]
    L --> U1["Understand the request"]
    L --> U2["Keep conversation context"]
    L --> U3["Make a plan"]
    L --> U4["Choose a tool"]
    U4 --> TL["🛠️ Tools (the body)"]
    TL --> ENV["🌍 Environment"]
    ENV --> L
```

---

## 11. Day 2 Mind Map 🗺️

```mermaid
mindmap
  root((LLMs))
    What is it
      Understands and generates language
      Transformer based
      Billions of parameters
      Mostly decoders
    Next token prediction
      Tokens are sub-words
      Autoregressive loop
      EOS token stops it
      Greedy or beam search
    Attention
      Focus on key words
      Context length
    Prompting
      Wording guides output
    Training
      Pre-training
      Fine-tuning
    Usage
      Run locally
      Cloud API
    In agents
      The brain
      Plans and picks tools
```