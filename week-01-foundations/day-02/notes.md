# Day 02: What are LLMs?

**📚 Source:** [Hugging Face Agents Course, Unit 1: What are LLMs?](https://huggingface.co/learn/agents-course/unit1/what-are-llms)
**🗺️ Roadmap:** Week 1, Foundations

---

## 💡 Today I Learned

> An **LLM** is an AI model that reads text and **predicts the next token, one token at a time**. It does this so well that it can chat, plan, and decide which tools to use. That is why it is the **brain of an agent**.

---

## 1. What is a Large Language Model? 🧠

- An LLM is an AI model that is great at **understanding and generating human language**.
- It is trained on a **huge amount of text**, so it learns the patterns, structure, and even the tone of language.
- Most modern LLMs are built on the **Transformer** architecture, a deep learning design that is based on **Attention** (see section 3).
- LLMs have **millions to billions of parameters**. Parameters are the numbers the model learned during training.

### The 3 types of Transformers

| Type | What it does | Example | Good for | Typical size |
|------|-------------|---------|----------|--------------|
| **Encoder** | Reads text and turns it into a numeric summary (an *embedding*) | BERT (Google) | Classification, semantic search, finding names in text | Millions of parameters |
| **Decoder** | **Generates** new text, one token at a time | Llama (Meta) | Chatbots, text generation, code generation | **Billions** of parameters |
| **Seq2Seq** (encoder + decoder) | Reads an input, then generates a new output | T5, BART | Translation, summarizing, paraphrasing | Millions of parameters |

**Remember:** When people say "LLM" today, they almost always mean a **decoder-based** model with billions of parameters.

### Well-known LLMs

| Model | Provider |
|-------|----------|
| GPT-4 | OpenAI |
| Llama 3 | Meta |
| DeepSeek-R1 | DeepSeek |
| Gemma | Google |
| Mistral | Mistral |
| SmolLM2 | Hugging Face |

---

## 2. Understanding Next Token Prediction 🔮

### What is a token?

A **token** is the small unit of text an LLM works with. It is **like a word, but usually smaller**.

- English has roughly 600,000 words, but an LLM may only use about **32,000 tokens** (Llama 2 is an example).
- Tokens are often **sub-word pieces** that can be combined:
  - `interest` + `ing` → **interesting**
  - `interest` + `ed` → **interested**

This keeps the vocabulary small and efficient.

### The one job of an LLM

> Given the tokens so far, **predict the next token**.

That's it. Everything else (chatting, coding, planning) comes from doing this one job very well.

### Autoregressive: the output becomes the next input 🔁

LLMs are **autoregressive**. This means:

1. The model predicts one token.
2. That token is **added to the input**.
3. The model predicts the next token using the longer input.
4. Repeat, until the model predicts a special **EOS** token, which means "stop".

### What happens in one prediction step?

1. The text is **tokenized** (split into tokens).
2. The model builds a representation of the tokens, capturing their **meaning and position**.
3. The model gives a **score to every token** in its vocabulary for being the next one.
4. A **decoding strategy** picks the token:
   - **Greedy:** always pick the highest-scoring token (simplest).
   - **Beam search:** explore several possible sequences and choose the one with the best **total** score, even if some single tokens scored lower.

### Special tokens

Every model has **special tokens** that mark the structure of text, such as the start or end of a message. The most important one is **EOS (End Of Sequence)**.

| Model | EOS token |
|-------|-----------|
| GPT-4 | `<\|endoftext\|>` |
| Llama 3 | `<\|eot_id\|>` |
| DeepSeek-R1 | `<\|end_of_sentence\|>` |
| SmolLM2 | `<\|im_end\|>` |
| Gemma | `<end_of_turn>` |

No need to memorize them. Just remember that **each model uses its own special tokens**.

---

## 3. Attention Is All You Need 👀

- When predicting the next word, **not every word matters equally**.
- Example: in *"The capital of **France** is ..."*, the words **"capital"** and **"France"** matter most.
- **Attention** is the mechanism that lets the model **focus on the most important tokens** in the input.
- The core idea of LLMs (predict the next token) has stayed the same since GPT-2. The big progress has come from **scaling up** and making attention work over **longer and longer text**.

### Context length

**Context length** is the **maximum number of tokens** an LLM can handle at once. You can think of it as the model's **attention span** or working memory.

---

## 4. Prompting the LLM Is Important ✍️

- The LLM only predicts the next token based on the input tokens it sees.
- So **how you word your input really matters**.
- The input you give an LLM is called a **prompt**.
- A carefully designed prompt **guides the model toward the answer you want**.

**Simple tip:** be clear, give context, and say exactly what you want.

---

## 5. How Are LLMs Trained? 🏋️

**Step 1: Pre-training**
- The model reads **huge amounts of text** and learns to **predict the next word**.
- It needs no human labels (it's *self-supervised*).
- Result: it learns the structure of language and patterns it can **apply to new text**.

**Step 2: Fine-tuning**
- The pre-trained model is trained further on a **specific task** with labeled examples (*supervised learning*).
- Examples: chat conversations, **tool usage**, classification, code generation.

```
Raw text  →  Pre-training  →  General LLM  →  Fine-tuning  →  Specialized LLM
```

---

## 6. How Can I Use LLMs? 💻

| Option | What it means | When to pick it |
|--------|--------------|-----------------|
| **Run locally** | Run the model on your own computer | You have strong enough hardware |
| **Use a cloud / API** | Send requests to a hosted model | Easiest way to start, and what this course mainly uses (Hugging Face Inference API) |

---

## 7. How Are LLMs Used in AI Agents? 🤖

The LLM is **the brain of the agent**. It can:

- **Understand** what the user wants
- **Keep context** during a conversation
- **Make a plan**
- **Decide which tool to use**

Link to Day 1: *Brain (LLM) + Body (tools) = Agent.*

---

## 🧠 Key Terms

| Term | Meaning |
|------|---------|
| **LLM** | Large Language Model, trained to predict the next token |
| **Transformer** | The neural network design behind modern LLMs |
| **Parameters** | The numbers the model learned during training |
| **Token** | A small piece of text (often part of a word) |
| **Vocabulary** | The full list of tokens a model knows |
| **Autoregressive** | Each output token is fed back in to make the next one |
| **EOS token** | Special token that tells the model to stop |
| **Decoding strategy** | The method for choosing the next token (greedy, beam search) |
| **Attention** | Lets the model focus on the most relevant tokens |
| **Context length** | Maximum tokens the model can handle at once |
| **Prompt** | The input text you give an LLM |
| **Pre-training** | Learning general language from huge text data |
| **Fine-tuning** | Extra training for a specific task |

---

## ❓ Questions I Still Have

- [ ] How exactly does the Attention mechanism calculate what is "important"?
- [ ] What happens when my input is longer than the context length?

---

## 🔗 Files for Today

- 📊 [Diagrams](./diagram.md)