# Day 03: Messages and Special Tokens

**📚 Source:** [Hugging Face Agents Course, Unit 1: Messages and Special Tokens](https://huggingface.co/learn/agents-course/unit1/messages-and-special-tokens)
**🗺️ Roadmap:** Week 1, Foundations

---

## 💡 Today I Learned

> A chat is just a **UI illusion**. Behind the scenes, all the messages are **glued into one long text prompt** using a **chat template** with **special tokens**, and the LLM **re-reads the whole thing every single time**.

---

## 1. Chat Messages vs Prompts 💬

When I use ChatGPT, it feels like a back-and-forth conversation with separate messages. But the LLM doesn't see it that way.

| What I see (UI) | What the model gets |
|-----------------|---------------------|
| Separate messages: user, assistant, user... | **One single long text prompt** with all messages joined |

**Big idea:** The model does **not remember** the conversation. Every time, it **reads the full conversation again** from the start. That is why the whole history must be sent each time.

The tool that turns messages into the right prompt is the **chat template**.

---

## 2. The 3 Kinds of Messages 🏷️

Every message has a **role** and some **content**.

| Role | Who writes it | Purpose |
|------|--------------|---------|
| **system** | The developer | Sets **how the model should behave** (persistent instructions) |
| **user** | The human | The questions and requests |
| **assistant** | The LLM | The model's replies |

### System message: the model's "personality and rules"

```python
system_message = {
    "role": "system",
    "content": "You are a professional customer service agent. Always be polite, clear, and helpful."
}
```

Change the system message, and the **same model behaves very differently**. Alfred the polite helper can become Alfred the rebel agent just by editing this one message.

### In agents, the system message does even more

For an agent, the system message also contains:
- **Which tools** are available
- **How to format an action** when the agent wants to use a tool
- **How to structure its thinking** (the thought process)

### User and assistant messages: the conversation

A conversation is a list of alternating user and assistant messages:

```python
conversation = [
    {"role": "user", "content": "I need help with my order"},
    {"role": "assistant", "content": "I'd be happy to help. Could you provide your order number?"},
    {"role": "user", "content": "It's ORDER-123"},
]
```

Keeping the previous messages in the list is what gives the model **context** for multi-turn chats.

---

## 3. Special Tokens Again 🔖

From Day 2: every model has its own special tokens, such as **EOS** (End Of Sequence).

Today's new idea: special tokens are also used to **mark where each message starts and ends**, so the model can tell who is speaking.

Each model uses **different** markers. Here is the same conversation formatted two ways.

**SmolLM2 style (ChatML):**

```
<|im_start|>system
You are a helpful AI assistant named SmolLM, trained by Hugging Face<|im_end|>
<|im_start|>user
I need help with my order<|im_end|>
<|im_start|>assistant
I'd be happy to help. Could you provide your order number?<|im_end|>
<|im_start|>user
It's ORDER-123<|im_end|>
<|im_start|>assistant
```

**Llama 3.2 style:**

```
<|begin_of_text|><|start_header_id|>system<|end_header_id|>

...system text...<|eot_id|><|start_header_id|>user<|end_header_id|>

I need help with my order<|eot_id|><|start_header_id|>assistant<|end_header_id|>
...
```

Same conversation, different formatting. That is why we need chat templates.

Notice that the prompt **ends with an open assistant turn**. That is the model's cue: *"now it's your turn to write."*

---

## 4. Chat Templates 🧱

A **chat template** is the **bridge** between:
- the list of messages (system, user, assistant), and
- the exact text format a particular model expects.

It makes sure every model gets a correctly formatted prompt, even though each one uses different special tokens.

**Under the hood:** In the `transformers` library, a chat template is written in **Jinja2** code. It loops over the messages and wraps each one with the right special tokens. A simplified SmolLM2 idea:

```
for each message:
    write  <|im_start|> + role
    write  the message content + <|im_end|>
```

**ChatML** is a common standard: a clean list of `{"role": ..., "content": ...}` messages. Most AI APIs use this style.

---

## 5. Base Models vs Instruct Models 🆚

| | Base model | Instruct model |
|---|-----------|----------------|
| **Trained on** | Raw text, to predict the next token | Fine-tuned to **follow instructions and chat** |
| **Example** | `SmolLM2-135M` | `SmolLM2-135M-Instruct` |

**Important:** An instruct model must be given its **own correct chat template**. A model may be fine-tuned on a specific template, so using the wrong one can give bad results.

---

## 6. Code: Turning Messages into a Prompt 💻

The easy way is to let the **tokenizer** apply the model's own chat template:

```python
from transformers import AutoTokenizer

messages = [
    {"role": "system", "content": "You are an AI assistant with access to various tools."},
    {"role": "user", "content": "Hi !"},
    {"role": "assistant", "content": "Hi human, what can help you with ?"},
]

tokenizer = AutoTokenizer.from_pretrained("HuggingFaceTB/SmolLM2-1.7B-Instruct")
rendered_prompt = tokenizer.apply_chat_template(
    messages,
    tokenize=False,              # give me text, not token IDs
    add_generation_prompt=True,  # add the open assistant turn at the end
)
print(rendered_prompt)
```

- `tokenize=False` returns the **text prompt** so I can read it.
- `add_generation_prompt=True` adds the open **assistant** turn so the model knows to reply.

**My job:** structure the messages correctly. **The tokenizer's job:** handle the special tokens.

This function runs in the backend of an API when I send messages in ChatML format.

---

## 7. Why This Matters for Agents 🤖

- The system message is where the agent learns **its tools, action format, and thinking steps**.
- Conversation history and tool results are all stored as **messages** and re-sent to the LLM each turn.
- Getting the template right is key to an agent that behaves correctly.

---

## 🧠 Key Terms

| Term | Meaning |
|------|---------|
| **Message** | One turn in a chat, with a role and content |
| **Role** | Who is speaking: system, user, or assistant |
| **System message / prompt** | Persistent instructions that shape model behavior |
| **Prompt** | The final single text the LLM actually receives |
| **Chat template** | Rules that convert a list of messages into the model's prompt format |
| **ChatML** | A common message format with clear role labels |
| **Special tokens** | Markers for start and end of messages and sequences |
| **EOS** | End Of Sequence token that tells the model to stop |
| **Base model** | Trained only to predict the next token |
| **Instruct model** | Fine-tuned to follow instructions and chat |
| **Jinja2** | The templating language used to write chat templates |
| **apply_chat_template()** | Function that renders messages into the prompt |

---

## ❓ Questions I Still Have

- [ ] If the full history is re-sent every time, what happens when it gets longer than the context length?
- [ ] How do tool results get added to the message list?
- [ ] What does a real agent system prompt look like in full?
- [ ] What happens if I use the wrong chat template on purpose? _(Fun experiment to try!)_

---

## 🧪 Try It Yourself

1. Run the code above and **print `rendered_prompt`**.
2. Swap the model for a different one and compare the special tokens.
3. Change the system message to be **polite**, then **rude**, and see how it changes the prompt and replies.

---

## 🔗 Files for Today

- 📊 [Diagrams](./diagram.md)