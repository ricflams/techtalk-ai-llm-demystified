---
marp: true
html: true
transition: reveal
theme: demystified
paginate: true
---
<style>
h4, h4 ~ * { display: none; }
</style>

# AI Demystified

## or  "A eye opening insight"

Richard Flamsholt · 2026

#### 
What do I want to tell:
Token, Embedding, LLM, Context, AI-model, Agent, (Prompt), Modes, Thinking, Agent-files, Skills, Tools, MCP - and more.
So you become more confident with the terms, know why AI-models are different, and what the Agents can do and how to drive it.

I hope you'll latch onto something and refer back to the slides and me if you've got questions.

---
## 10,000,001 AI videos... +1
<div class="cols">
<img src="images/intro/youtube-intro-1.png">
<img src="images/intro/youtube-intro-2.png">
<img src="images/intro/youtube-intro-3.png">
<img src="images/intro/youtube-intro-4.png">
</div>

#### Why this presentation?

What can I possibly say that hasn't already been said in the 10,000,000 existing AI-related videos?
Why is one more presentation about LLMs useful?

I guess for the same reason that you ask an AI about any topic instead of reading some of the 10,000,000 webpages about that topic: you get a personal, curated presentation that tells the story in a way I find insightful - hopefully presenting **just the good bits** from those 10,000,000 videos. Also, you can ask any questions you have.

---
## All the parts
<img class="full" src="images/overview/full.png" />

The **human** use an **AI Agent** to communicate with an **AI Service**, that turn your message into **tokens** and feed it through an **LLM**. The service and agent can use **tools** and the agent also has **memory**.

---
# The AI Service

---
## AI Data center

<img src="images/intro/datacenter.jpg">

####
The commercial **AI Services** run in massive data centers.

---
### Frontier AI Foundation Models (FM)

<div class="cols">
<div>

<img class="logo" src="images/intro/logo-claude.svg"> **Claude** by Anthropic

<img class="logo" src="images/intro/logo-openai.svg"> **ChatGPT** by OpenAI

<img class="logo" src="images/intro/logo-gemini.svg"> **Gemini** by Google

<img class="logo" src="images/intro/logo-grok.png"> **Grok** by xAI *(Elon Musk)*

<img class="logo" src="images/intro/logo-meta.svg"> **Llama** by Meta *(Facebook)*

</div>
<div>

<img class="logo" src="images/intro/logo-deepseek.svg"> **DeepSeek** by DeepSeek *(Chinese)*

<img class="logo" src="images/intro/logo-mistral.svg"> **Le Chat** by Mistral AI *(French)*

<br/>

<img class="logo" src="images/intro/logo-copilot.png"> Copilot by Microsoft is *not a model*

</div>
</div>

---
### Let's ask the LLM
<img class="full" src="images/ai-service/what-is-an-llm-in-claude.png">

---
### Let's ask the LLM
<img class="full" src="images/ai-service/what-is-an-llm-in-ai-service.png">

---
## Tokens
<img class="full" src="images/overview/tokens.png">

---
### A token is

* Practically, for you: it's **a word**
<!-- -->
* More precisely: it is the **chunks** the AI break your text into
<!-- -->
* What you ultimately **pay for** - it's the **unit of work** for the LLM

---
### "hello" is token 24912 (in gpt-4o)
<img class="full" src="images/token/tokenize-detokenize-hello.png">

---
### ChatGPT 3.5's token vocabulary
<img class="full" src="images/token/vocabulary-full.png">

####
* [ChatGPT’s entire vocabulary](https://emaggiori.com/chatgpt-all-tokens/)


---
### "hello" is token 24912
<img class="full" src="images/token/hello.png">

####
Tokenizers:

* [Tiktokenizer](https://tiktokenizer.vercel.app/)
* [OpenAI's tokenizer](https://platform.openai.com/tokenizer)


---
### "hello world"
<img class="full" src="images/token/hello-world.png">


---
### "hello" in the vocabulary
<img class="full" src="images/token/vocabulary-hello.png">

####
Notice that "hello" in the gpt-4o tokenizer is #24912 and in the ChatGPT vocabulary it's 15339. Vocabularies change from model to model. The concrete numbers doesn't matter outside of the AI service so you shouldn't rely on them.


---
### "h e l l o   w o r l d"
<img class="full" src="images/token/h-e-l-l-o-w-o-r-l-d.png">

####
This shows one advantage of the tokenization: simply fewer tokens than if everything was spelled out.

---
### "hello world from me"
<img class="full" src="images/token/hello-world-from-richard.png">

####
The first four words are common and have each their own token, but "Flamsholt" isn't worthy of getting its own token so it's made up of 3 parts.

---
### Token for "Flam"
<img class="full" src="images/token/vocabulary-flam.png">

####
Look' there's "Flam", in dubious company of inflammatory tokens.

---
### Tokenizing identifies "traits"
<img class="full" src="images/token/wonderful-tokenization.png">

####
Another advantage of tokenization is that it allow identifying common linguistic "traits", such as "ization" which is about "doing/producing something". Works for other languages too.


---
### A danish elevator in motion
<img class="full" src="images/token/elevator-i-fart.jpg">

####
Another advantage of tokenization is that it allow identifying common linguistic "traits", such as "ization" which is about "doing/producing something". Works for other languages too.


---
### I fart poetry
<img class="full" src="images/token/i-fart-marked.png">

####
Another advantage of tokenization is that it allow identifying common linguistic "traits", such as "ization" which is about "doing/producing something". Works for other languages too.


---
### The English advantage
<img class="full" src="images/token/five-sentences.png">

####
Examples in English, Danish, Korean, Classical Chinese, and C#.

---
### Revisit the example
<img class="full" src="images/token/what-is-an-llm-tokens.png">


---
### Tokens, summarized

* A **token** is the **chunk of text** the AI service can reason about
* Models typically have a **vocabulary** of 200,000 tokens
* For English, 1 token is roughly 1 word

And last, but not least:

* AI services **charges per token**, since tokens are the unit the LLM works on
* A ballpark figure: you pay **$1 for 100,000 tokens**
* 100,000 tokens amounts to _"Harry Potter and the Philosopher's Stone"_

####
For English, one token generally corresponds to about 4 characters. For a text the number of tokens will typically be 30% higher than the number of words, ie 1000 words means 1300 tokens - broadly speaking.


---
## Embeddings
<img class="full" src="images/overview/embedding.png">

---
### One more basic part: embeddings
<img class="full" src="images/embedding/what-is-an-llm-embeddings.png">


---
### ⚠️ Warning ⚠️ 

This will sound confusing

Stay with me.


Computers don't understand the word "cat." They understand numbers. So we need a way to turn "cat" into numbers in a way that preserves its meaning. That's what an embedding does.

<img class="half" src="images/embedding/confused.png">

---
### An "embedding" is a thing, not an action
<img class="full" src="images/embedding/bicycle-tree.jpg" />

#### Notes
In everyday English, "embedding" sounds like something you do: the act of placing something into something else.

In AI, it means the concrete vector of numbers that _somehow_ represent the characteristics of something. It's a noun, not a verb. It's a "thing", not something that "happens".


---
### The Big Five test (OCEAN)
<img class="full" src="images/embedding/analogy_bigfive.svg" />


---
### Specialty Coffee Association (SCA)
<img class="full" src="images/embedding/analogy_coffee.svg" />


---
### Spotify's music characterization
<img class="full" src="images/embedding/analogy_spotify.svg" />


---
### Could we measure ... "everything"?
<img class="full" src="images/embedding/20d-blank.svg" />


---
### Measuring "everything"
<img class="full" src="images/embedding/20d-with-tokens.svg" />


---
### That's an embedding

<img class="full" src="images/embedding/20d-with-tokens.svg" />

Every **column** here is an embedding:

An embedding is **list of numbers** (a *vector*) that _somehow_ characterise **everything imaginable!** I say somehow because we don't know what each part of the dimensions does - it's just math that works our.

For example: Any **word** you know. Any **sentence** there exist. Any **feeling** you can have. The concept of **keeping an audience in suspense, eager for the next slide**, too.

####

Also called _tensor_ in python, or _hidden state_ when speaking about the LLM.

---
### More of "everything imaginable"

<div class="cols">
<div>

* The concept of the number 7
* Loneliness in a crowded place
* The entire first _Harry Potter_ book
* A single chess position mid-game
* The smell of rain on hot asphalt
* A user's purchases on a website
* A protein's amino acid sequence

</div>
<div>

* The grammatical role "indirect object"
* The concept of sarcasm _(yeah, right)_
* What "London-ness" feels like
* A function's behavior in a codebase
* A legal precedent and what it stands for
* The notion of "almost, but not quite"
* What 3 a.m. feels like

</div>
</div>

---
### The embeddings in ChatGPT
12288 dimensions


---
<img class="full" src="images/embedding/2d-90deg.png" />


---
<img class="full" src="images/embedding/2d-88deg.png" />


---
<img class="full" src="images/embedding/2d-60deg.png" />


---
<img class="full" src="images/embedding/2d-3d.png" />


---
<img class="full" src="images/embedding/superposition-explosion.png" />


---
### The values of an embedding
<img class="full" src="images/embedding/embedding_matrix.svg" />

####
Also sometimes called as "features", as each value in the vector encodes some learned semantic trait of the token. Like eg "catness" or "largeness".

In practice though we simply don't know what those dimensions mean. They don't map crisply to existing human concepts. Dimension 847 might contribute a little to formality, a little to temporal reference, a little to something related to food, and a lot to some abstract statistical regularity that doesn't map to any word in English.

There's a research field called mechanistic interpretability that tries to decompose these representations into interpretable directions. They can extract interpretable features, but understanding how features compose to produce behavior is still largely unsolved.

* [Scaling Monosemanticity and Feature Steering](https://learnmechinterp.com/topics/scaling-monosemanticity/)
* [Emotion concepts and their function in a large language model](https://www.anthropic.com/research/emotion-concepts-function)
* [How might LLMs store facts | Deep Learning Chapter 7 (3Blue1Brown)](https://www.youtube.com/watch?v=9-Jl0dxWQs8)


---
<img class="full" src="images/embedding/what-is-an-llm-embeddings-matrix.png" />


---
## LLM
<img class="full" src="images/overview/llm.png">


---
### The LLM is

a humongous calculator that can do just one thing: predict the next word.

_or more precisely_

**Run math on embeddings and produce probabilities for the next token**


---
### Our example
<img class="full" src="images/llm/what-is-an-llm-example.png">

####

Two things to note here:

Why only `An`? Why not the full answer? Yeah, hold your horses just a bit longer, because: the LLM only deals with producing one next token. That's all it is concerned about: figuring out with which *probablility* any of the tokens is the vocabulary has for being the next token.

That's the second thing to note: the LLM itself actually just produce this set of probabilities. The mechanism that actually *picks that next token* is strictly speaking outside the LLM. Let's include it here to convey that the outcome eventually is a token, namely `An` in this case.


---
### Attention is all you need
<img class="full" src="images/llm/attention-is-all-you-need.png">

####
The 2017 paper "Attention Is All You Need" by Vaswani et al. is arguably the most consequential piece of computer science research published in the 21st century.

Today, the paper sits at over 200,000 citations, making it an absolute statistical anomaly in scientific literature.

It is the Genesis block of modern AI. Without it, there is no GPT-4, no Gemini, no Claude, no Stable Diffusion, and no AlphaFold. It transformed AI from an academic field of hyper-specialized, rigid pipelines into a unified era of generalized foundation models.

---
### The Transformer
<img class="full" src="images/llm/attention-transformer.png">

####

---
### Let's walk through a fresh example
<img class="full" src="images/llm/what-we-want.png">

####

---
### The objective: find the next token
<img class="full" src="images/llm/find-the-next-token.png">

####

---
### Likely next tokens after "you"
<img class="full" src="images/llm/next-token-after-you.svg">

####

---
### Embeddings pay attention to each other
<img class="full" src="images/llm/attention-head.png">

####

---
### Neural network let training kick in
<img class="full" src="images/llm/multiplexer-perceptron.png">

####

---
### Focus on the strong signals
<img class="full" src="images/llm/relu.png">

####

---
### Now do this again...
<img class="full" src="images/llm/attention-head-2.png">

####

---
### In search for next token for "you"
<img class="full" src="images/llm/attention-space-seek.png">

####

---
### In fact, let's do it 96 times
<img class="full" src="images/llm/attention-96-layers.png">

####

---
### 
<img class="full" src="images/llm/final-embedding-all-absorbed.png">

####

---
### Find next token by similarity
<img class="full" src="images/llm/next-token-prediction.svg">

####

The cosine similarity of 0.97 is computed in isolation — it's purely a geometric measurement between two vectors, with no knowledge of the other 127,999 tokens. The 91% is a different kind of number entirely: it's the result of a competition. Softmax takes all 128,000 similarity scores simultaneously, exponentiates each one, and divides by their sum. Every token competes against every other token at once. "Stronger" claimed 91% of the total probability mass — not because 0.97 is intrinsically high, but because it pulled far enough ahead of the field. A high cosine similarity is the evidence. The probability is the verdict.

---
### The final output token
<img class="full" src="images/llm/final-output-token.png">

####

---
### Input tokens vs output tokens
<img class="full" src="images/llm/final-output-token.png">

####

---
### Nerd movie night: Karpathy builds a GPT

Only 600 lines of Python code: 300 for `train.py`, 300 for `model.py`

<img class="full" src="images/llm/karpathy-nanogpt.png">

####
2 hours of Andrej Karpathy building a small GPT model, fully.

[Let's build GPT from scratch (2 hours)](https://www.youtube.com/watch?v=kCc8FmEb1nY)
[Let's reproduce GPT-2 (4 hours)](https://www.youtube.com/watch?v=l8pRSuU81PU)
[Github repo for nanoGPT](https://github.com/karpathy/nanoGPT)

---
### 170 billion weights
<img class="full" src="images/llm/170-billion-weights.png">

---
## The AI Service
<img class="full" src="images/overview/ai-service.png">

---
### What can it do?

_Surprisingly, only a few things:_

1. Reason about the chat to produce a response
2. Understand documents and images
3. Call internal tools; python, images
4. Ask to use tools or MCP servers
5. Use MCP servers itself
6. Think harder

Plus a few things on its own; caching, safeguards


---
### But but but - what about...?

* all those agents.md files you hear about?
* skills
* setting the language
* modes - plan, autopilot, and so on?
* conversation tone?



---
### Overview
<img class="full" src="images/ai-service/top-level.png">


---
### The full monty


####
Your diagram is the undisputed Common Core of the modern LLM runtime. For standard open-weight architectures (like Meta's Llama series or Mistral) and classic chat endpoints, this blueprint captures the system flawlessly.

Your diagram captures the Canonical Engine Blueprint. It cleanly outlines the immutable data flow, boundary gates, and structural contracts of modern AI. Leaving the vendor-specific bells and whistles off the page isn't an omission—it's good engineering discipline.


---
### Token spree


hotdog-zoom-in analogy

---
# The AI Agent

What is an AI Agent?

---
## Choosing of AI Service and LLM-model

---
## Choosing an AI Agent

---
# How to drive your AI Agent

---
## Level 0: Use the basic controls: modes (plan, agent), thinking, individual chats

---
## Level 1: give it specific general instructions

---
## Level 2: give it task-specific instructions: in web, use projects or gems etc; on CLI use agent.md files

---
## Level 3a: Skills

Skills look like a smart routing system — describe what a skill does, and Claude figures out when to use it. But the routing isn't magic, and it isn't symmetric.
There's an instruction baked into Claude Code's system prompt that says roughly: before writing code or creating files, check whether any skills are relevant. That instruction is what makes skills feel reliable for those task types. It's a forcing function — the model is explicitly told to look before it acts.
For everything else — answering questions, explaining concepts, responding to anything conversational — there's no forcing function. The model might still invoke a skill if your description matches strongly enough, but it's relying on the description alone to catch its attention during the forward pass. That's a much weaker signal.
The practical consequence: if you write a skill for a task type that isn't file creation or code writing — say, a skill for how your team handles incident postmortems, or tone guidelines for executive communication — and you wonder why it's not triggering reliably, this is why.
The fix is simple: add an explicit instruction to your CLAUDE.md that names the trigger condition and the file path. "Before responding to any question about incidents, first read this skill." That gives you the same forcing function the built-in instruction provides, but for your task type.
Skills aren't self-activating. The description is a hint, not a contract. If you need guaranteed activation, you need an explicit instruction.
---
## Level 3b: Tools

---
## Level 3c: MCP

my mcp poc example

---
## Level 4: Go crazy with subagents, agent-specific commands, openclaw, etc

---
## Level 5: Go beyond the Agent: speak directly AI API, setup RAG vector database, control temperature and sys prompt, etc

---
# How to brake the AI Agent
....

---
# Fact or Fiction?

"Lost in the middle was real in 2023, is largely solved for simple retrieval in 2026, persists for complex tasks — and the reason it ever existed is still being worked out. so my suggestion is: stop worrying about the middle, and start worrying about the load. "Keep your facts close and your context lean."

---
# Embeddings, revisited

---
# How I use AI now

---
# Summary


