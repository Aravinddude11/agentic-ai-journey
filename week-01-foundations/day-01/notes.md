# Day 01: What is an Agent?

**📚 Source:** [Hugging Face Agents Course, Unit 1: What is an Agent?](https://huggingface.co/learn/agents-course/unit1/what-are-agents)
**🗺️ Roadmap:** Week 1, Foundations

---

## 💡 Today I Learned

> An **agent** is an AI system that doesn't just talk: it **thinks, plans, and takes actions using tools** to finish a task for you.

---

## 1. The Simple Story: Aarvi the Coffee Agent ☕

Imagine you tell Aarvi: *"I'd like a coffee, please."*

1. **Understand:** Aarvi understands your request in normal language.
2. **Plan:** He figures out the steps: go to the kitchen → use the coffee machine → brew → bring it back.
3. **Act:** He uses a tool (the coffee machine) to actually make the coffee.
4. **Deliver:** He brings you the coffee.

That's an agent: something that can **reason, plan, and interact with the world around it** to get a job done.

The word "agent" comes from **agency**, which means having the ability to act on the environment instead of just answering.

---

## 2. Definition in Plain Words

An agent is a system that:

- uses an **AI model** to think,
- **interacts with its environment** (apps, files, the web, other systems),
- to **achieve a goal the user gave it**.

It combines **reasoning + planning + taking actions** (usually through tools).

---

## 3. The Two Parts of an Agent: Brain and Body 🧠🦾

| Part | What it is | What it does |
|------|-----------|--------------|
| **Brain** | The AI model (usually an LLM) | Thinks, plans, and decides what to do next |
| **Body** | The tools and abilities it has | Actually performs actions |

**Key idea:** An agent can only do what its body allows.
Humans can walk and grab things, but we can't fly, because we have no wings. In the same way, an agent that has no "send email" tool can't send emails, however smart the brain is.

---

## 4. Which AI Model Do Agents Use?

- Most agents use an **LLM (Large Language Model)**. It takes **text in** and gives **text out**.
- Examples: GPT-4 (OpenAI), Llama (Meta), Gemini (Google).
- Some agents use a **VLM (Vision Language Model)**, which is like an LLM but can also understand **images**.
- For now, I'm focusing on LLMs.

---

## 5. How Does an LLM Take Actions? 🛠️

**Problem:** An LLM can only **generate text**. It cannot send an email, browse the web, or draw a picture by itself.

**Solution: Tools.** Developers connect extra functions (tools) to the LLM. The LLM decides when to use one, and the code runs it.

Example: when ChatGPT creates an image, the LLM isn't drawing it. It's calling an **image generation tool**.

### Tool example in Python

A tool is just a normal function with a clear description:

```python
def send_message_to(recipient, message):
    """Useful to send an e-mail message to a recipient"""
    ...
```

When the user says *"Email my manager to delay today's meeting"*, the LLM produces a call like this:

```python
send_message_to("Manager", "Can we postpone today's meeting?")
```

The tool then runs and the email is sent.

### Two things to remember

- **Tool design matters a lot.** Good tools make a good agent. Some jobs need custom tools, while others work with general ones like `web_search`.
- **Action ≠ Tool.** An *action* is the step the agent takes (e.g., "book a trip"), and it may need **several tools** to complete.

---

## 6. Real-World Examples 🌍

| Example | How it works as an agent |
|---------|-------------------------|
| **Virtual assistants** (Siri, Alexa, Google Assistant) | Understand your request, look up info, then set reminders, send messages, or control smart devices |
| **Customer service chatbots** | Answer questions, guide troubleshooting, open tickets in databases, even complete purchases |
| **Game NPCs** (non-playable characters) | Respond to the player naturally instead of following fixed scripts, so characters feel alive |

---

## 7. Quick Summary ✅

An agent uses an AI model (usually an LLM) as its brain to:

1. **Understand natural language**, meaning human instructions
2. **Reason and plan**: analyze, decide, make strategies
3. **Interact with its environment**: gather info, take actions, check results

---

## 🧠 Key Terms

| Term | Meaning |
|------|---------|
| **Agent** | An AI system that reasons, plans, and acts to reach a goal |
| **Agency** | The ability to act on the environment |
| **Environment** | Everything the agent can interact with (apps, web, files, users) |
| **LLM** | A model that takes text and produces text |
| **Tool** | A function the agent can call to do something real |
| **Action** | A step taken by the agent, possibly using one or more tools |

---

## ❓ Questions I Still Have

- [ ] How exactly does the LLM decide *which* tool to call?
- [ ] What stops an agent from looping forever?
- [ ] How does the agent see the result of a tool? _(Next topic: Thought → Action → Observation)_

---

## 🔗 Files for Today

- 📊 [Diagrams](./diagram.md)