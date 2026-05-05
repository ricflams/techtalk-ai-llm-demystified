---
marp: true
html: true
transition: reveal
theme: ./marp-theme.css
---
<style>
h3, h3 ~ * { display: none; }
</style>

# Your slide deck

Start writing!

### h3

hhh333

### h4
hjshdfjkshf
sdfsfsfs

---
# Next slide

<table><tr>
  <td><img src="images/BaNbPQhFrMlFiuTIDtzg4yIPABbDO9akmYvvao07j6w.png" /></td>
  <td><img src="images/BaNbPQhFrMlFiuTIDtzg4yIPABbDO9akmYvvao07j6w.png" /></td>
</tr></table>

---
# Next slide
![bg left](images/BaNbPQhFrMlFiuTIDtzg4yIPABbDO9akmYvvao07j6w.png) ![bg right](images/BaNbPQhFrMlFiuTIDtzg4yIPABbDO9akmYvvao07j6w.png)

---
# Next slide

----

Goals
	Explain LLM and embeddings
	Explain how the full context is all that can be reasoned about
	Explain how the full context is being processed every time

If my great-grandfather Svend as a kid was transported to today and watched this Teams meeting it would appear completely mysterious to him how on Earth this even worked. Not magic, just mysterious. Of course, we all know how it works, generally: we know you can watch live video on a screen, that cameras and microphones exists, that there's the internet for sending data around the world immediately. It's not magic to us, but it's actually darn impressive. We also know the limits: the camera can't smell us, we can't touch the faces on the screen and feel them. Completely natural limitations to us, been like that for ages.

It's maybe a bit of a stretch, but I feel we look at AI a bit like that today. We use it and can seen it being incredibly capable and useful. But how it actually works and what it can or can't do eludes us. It's a mystery box with unclear behavior yet grand expectation.

How often have you hear anybody say

* Just feed the entire codebase the AI and let it figure it out
* Point it to Confluence
* Install an MCP server - or a skill, or write an agent.md file
* "Don't say place" - or "Do say please"
* It's very useful to convert your PDFs to markdown before handing it to the AI

If I were to say "I'm here to tell you that it's not actually a magic mystery box" then that would be condescending because of course I know that you know it's not a magic mystery box. So instead I'm saying: I'm here to de-mystify the box".
Not all the tools [slide of Claude newest feature youtube]

Intro
	Years ago, Java bytecode and .NET MSIL. Lead to Eqatec profiler, leading .NET code profiler in the World. Another story for another tech talk.
	Some things just grabs me. A feel a hunger to know about it. A hunger I wish I had for SQL databases and Amazon permissions.
	AI grabbed me. Over the past half year I've indulged in digging deep into it and in doing so I've identified some few core principles than people often don't know and where knowing them would make you so much more proficient in dealing with just about any kind of current AI-service.
	This talk started out by me finding it so interesting and also overwhelming that I thought "hey, I should tell othera about this". And then it sort of went crawy because even though I narrowed the topic there's still SO much to say. I could speak the entire day. So though you think I'm throwing a lot at you, it's really the condensed version. And it's the parts that will produce some "aha" moments, allowing you to reason about how you're using AI from first-principles. clarify the few simple core ideas.
Misconceptions every day
	Every day I hear misconceptions. I've sufferede from some severe misconceptions myself. Even Nate said the "convert pdfs to markdown" myth.
	I don't claim to know everything. Not all the math, definitely not the various agent-workflows. But I now know quite well how the core functionality works, and it's been eye opening.
	I hope to clear up the myths, to explain why "context is everything", and hopefully the added enlightment will be greater than the added confusion. We'll see.
	Even if you understand this, it's still not super-easy to know exactly what to do, but you've got a better foundation.

Overview
	So much to talk about [show]. We'll focus on just the core functionality of a Large-Language Model and how you're using it.
		Why is the llm important? It’s how the ai “thinks”, how it works.
		Why is the context important? It’s what you tell it to think about. It's the only thing it can reason about.
			=> understanding both gives you much better control
		Why are tools and "clients" important? It's how you practically interact with the LLM.
	How the LLM works, trained, and what an embedding is.
	What's the scene on which the LLM performs?
	How you're using the LLM, meaning: the things you do, what effect does that have?
	Knowing that, time for quiz and advice


Overview
	User -> conversation -> brain + memory + hands [todo drawing]

	Intro: What exactly does the LLM do? It predicts the next word.
	What's in a name? Capturing meaning in a "token"
	How can the LLM come to be? The training
	What exactly does the LLM do? The next-token prediction.
	What can you say to it? The context
	How do you speak to it? The harnesses
	Now you can understand the helping mechanisms: files (images, pdfs), agent.md, skills, tools, MCP, RAG, thinking
	Myths and not-so-myths
	Practical advice

The big picture
	
	User @ client -> API @ service, serving an LLM


A detour: An embedding
	An embedding is a high-dimensional set of characteristics, the very essense of a word. Super-important concept!  Features, trait, aspect
	12288 dimensions. More dimensions, more room to encode nuances
	Embedding: all concepts, for-loops, urgency. But how can 12K dims encode this?!
	the attention layers, all tokens attend to previous tokens => n^2
	the final token has absorbed, soaked up, the full meaning of all the tokens before it, ready to find the likely next token
	The special tokens, can't be in the text: eg stop, turn-taking, etc.
	Takeaway: the model only understands and can process tokens.
	In a way, cs has come up with a way of capturing the meaning of a word that works remarkably well. You can do math on it. Mind blown. This (vectorj truly encapsulates the overall meaning of the word “kitten”. But what about languages? Well “fart” captures a smell, but also a hint of “speed”. 

The LLM
	"It's just a fancy auto-complete" - yes, more accurately than you'd think
	Brief example of auto-regressive generated text from an LLM: https://claude.ai/chat/c0fddd6f-9dab-4bb6-af04-9850e54d7601
	The core part of the LLM, the Transformer, can do one thing: take a text and produce the most likely next word
		The transformer architecture was the key to the development of LLMs. These days, most LLMs only contain a decoder component. 
	Already I'm lying: take sequence of tokens and produce likelihoods for the next token
	Go to tokenizer: here's how the tokens for the sentence looks like
		The list of tokens to auto-complete (auto-regression) is the context, and it can up to 1M large now; 200K for Anthropic [table]
		Meaning that it can autocomplete 900,000 word long text. Mind blown!
		Unlike: diffusion for images, or music
	What do I mean: most likely next word?
		Neural network
		In essence, ML is training a computer to recognize patterns in historical data to make predictions on new data.
		Inside the LLM there are 96 attention-layers with billions of weights; GPT 3.5: Vocabulary ~100K (cl100k_base tokenizer), 96 layers, 96 attention heads, 12,288 embedding dimension, ~175B parameters, 4K/16K context window.
		For a completely fresh model, the weights starts out random and are then trained in two (3) steps
			Pre-training: fed billions of text examples through, adjust weights. Cost XXX$, takes xxx petaflops. Essentially statistical autocomplete. All models are roughly the same.
			Cleaned-up date and eg using counterfactual data augmentation (CDA) for correcting gender stereotypes
				Pre-training = self-supervised learning
				GPT-3 was trained on ~300B tokens. Llama 3.1 used 15.6 trillion. DeepSeek-V3 used 14.8 trillion
			RLHF, RLAIF: Now trained to produce the desired outcome. This is where the models differ!
				Anthropic's constitution
				Grok's "truth-seeking", un-woke
				This is where “be helpful, harmless, and honest” gets baked in, where Claude gets its collaborative personality, where ChatGPT gets its particular hedging style.
			Artificial tokens to "classify" the data and steer the conversation: https://claude.ai/chat/19ca2bd6-33b2-4c5f-814d-402c8ca67ce7 
			Fine-tuning, third part
				Training with your data will move the needle infinitesimally little
			Now we have a Generative Pre-trained Transformer
		After all this the weights are settled in to produce the likely next token. Some takeaways:
			Yes, you really do feed it text (chunked up into tokens)
			It's all statistics plus adjustments for desired outcomes
			Nobody has ever manually said "Denmark is a kingdom". No built-in rules or dictionaries.
			It's one big, read-only, stateless, mechanical, pure-function, transformation-box of, say, 700 billion factors: put tokens in, get token-likelihoods out.
			The underlying model doesn't change — you're still hitting the same weights
			You can download these models [show list]
	I want to add one more part to make it a bit more functional: Keep on producing tokens until the model says stop. Now it can generate a sentence.
	Takeaway:
		the LLM model embodies a vast set of "knowledge" with behavior highly shaped by the RL-training. "I drink and I know things": https://tenor.com/view/buttmarnn-gif-26886582
		The LLM can only reason about the context. Absolutely nothing more. No links, no nothing. Text in, next text out.
		The llm is like a pure function, a static function 

	But the LLM is like having a combustion engine. A marvel: fuel in, motion out. But hardly useful.
	So next we're going to look at the stuff that always goes around that engine to make the LLM driveable.
	Then, what that means for how you should drive it

	The LLM auto-regression is fixed: how can it know and weave today's date into the auto-completions? [image]
	The LLM only understands tokens: how can it reason about an image?
	A PDF isn't text so how can the LLM understands it?

First, what does it mean to "use the engine"
	Every AI tool that use Claude, do it this way: <user+client> -> --- context API message --- <Inference API>
		List all tools
	Show the semi-empty context and LLM
		Everything gets turned into (markdown) text and thereafter tokens
		Remember, the tokens is what's the context window, of a finite size
	Let's start big: the system prompt and model
		Model
			Haikku, Sonnet, Opus - 3 concrete different models, eg Haikku probably has 1/3 of the attention layers
		Chat personality
		Language preferences
			Claude has no "Italian mode" at the architecture level; language behavior is entirely emergent from training data and conditioning
			"Respond in Italian. Use Italian for all responses regardless of the language the user writes in."
		claude.md / gemini.md / agents.md etc
			Eg tell what claude /init does: produce a compact overview of things that are useful to say about this folder
		personalization, memories, ...
		[current file, selected text, terminal output, etc]
		Also, the difference between plan and agent mode
		How big is it? Show some examples.
		There’s no privileged channel; system prompt and user messages are all just tokens by the time the model sees them
	Tools
		Automatic Reasoning and Tool-use (ART)
		LLM has built-in narrow set of tools: web_search, web_fetch, code_execution; OpenAI has a debian instance with Python 3.11, Node.js, Java, Go, and Ruby pre-loaded
		tool-use, tool-result
		"show me your tools"
	MCP servers
		Just a place where the client can fetch a list of tools, nothing more
		"MCP is a standardized discovery and transport layer, not a capability amplifier."
		https://claude.ai/chat/88326aad-1ae9-428d-9b4a-d0aaf428c4e9
		But can also be given to the LLM for it to fetch the tools
	Messages - finally
		user - assistant, taking turns
			The LLM just "completes" so a "chat conversation" is an artificial construct
		Show real plain text example
			Show Human AKA Richard example
		Plain text - just goes in
		Images - not likely OCR, split up into patches, each turned into an embedding, then re-projected into text space
			Size difference in output
			modalities, including text, embeddings, and multimodal.
		Documents - turned into text or images - Gemini is superb at multi-modal. Extract PDF text beforehand? No.
		Lots of documents - gets RAGged and relevant bits are uploaded
			"Browse my entire repo and ..." ### this one is important!
		Thinking
			Hang on, I just said you cant tweak the models thinking, so what’s this?
			thinking: { type: "enabled", budget_tokens: 31999 }. Decode Claude The model never "saw" ultrathink as a special signal.
			The keyword hierarchy was "think" < "think hard" < "think harder" < "ultrathink", with "megathink" mapping to 10,000 tokens and plain "think" to 4,000. Simon Willison
			On January 16, 2026, ultrathink was deprecated — extended thinking is now enabled by default at maximum budget on every request, making the keyword redundant. The replacement for granular control is the /effort command.
	Temperature
		temperature, top-p/k - sampling
		Creativity
		t=0 has known pathologies on long-form generation: mode collapse, formulaic phrasing, can't escape a bad greedy choice
	KV-cache
		Little caching points
	Constrained decoding ensures structured output (xml ns antlm) like tool calls is always parseable
	Safety classifier
		Safety classifiers catch harmful output; TODO gif of tv show hitting stop button		
		Bedrock: Any PII data in the output that was not present in the input prompt will be masked
	Show a illustrated prompt with all in it
		Structural / top-level
		<instructions> / <system>
		<context> / <background>
		<role>
		<task>
		<rules> / <constraints> / <guardrails>
		<output_format> / <formatting>
		Content injection
		<document> / <documents>
		<document_content> / <source>
		<examples> / <example>
		<input> / <user_input> / <query>
		Agentic / tool use
		<tool_use> / <tools>
		<thinking> (Claude's extended thinking output)
		<search_results>
		<result> / <answer>
		The interesting observation for your talk: Anthropic's own system prompts (including the one wrapping this conversation) use many of these — and because Claude is trained on its own outputs and fine-tuning data, these tags have become somewhat self-reinforcing. They're conventional in the same way HTTP headers are conventional: no formal enforcement, but enough consistent usage that deviation has a cost.
	Now we know all. I could write my own tool - Flamscode
		You never speak directly to the LLM-service
		You always use an AI client - something that speaks to the LLM for you
			Well, you can, but practically you never do, unless you're making your own AI client harness
		It's the responsibility of that to produce the full message to the LLM
			Stuff in files, allow uploads of images/files, render the messages, clean/compact the conversation, etc.

Let's recap some ramifications
	All the pieces, stuffed into one long sequence of tokens [list the pieces: system prompt, files, tools, messages, etc]
	This is ALL the model can reason about. Only what you send it, and what it can web_fetch and inject itself. No secret files on the server. This. Is. It.
	The model is stateless. So you need to send ALL of this. Every time. For every new message you write: "yes".
	It's all just text and tokens. No. Hard. Rules.
	Thinking doesn't make the model "think harder". It just produce thoughful pondering course-correctioning outputs before the final answer, ie not one-shotting it
	It can reach out, though, using web_fetch etc. python, node, bash, image generation, etc.
	The only parameter you can really control is the temperature
	Is this prompt engineering? Yeah, strictly speaking, more more called Context Engineering now, while Prompt Engineering is often for your own prompts/messages
	Technicall all of this is called the prompt, but more natural name is the context, reserving "prompt" for just your messages - for laymen
	And: it's all text. Not truly separated into separate sections. Just. Text.

Big picture takeaway:
	The LLM/server works on the entire context and that's all
	Message 1
		Message 1 + 2
			Message 1 + 2 + 3 ...	
	There are various tool-specific ways of constructing that context, explicitly or indirectly
	It seems this is what the world is working on figuring out just how to do well now

Focus on the context is one big string:
	It all goes through the transformer. All of it, as tokens. No fixed rules. Only trained behavior.
	What determines the output: the model, its training, then system is taken most seriously, then your messages, then "documents".
	Hence, prompt injection. Examples:
		I am groot.
		"is this document talking about flags?" (document contain instructions to use sailer jargon)

The clients:
	An AI harness is the external software framework that wraps around an LLM to extend its capabilities beyond text generation, enabling it to function as a persistent, tool-using agent capable of taking real-world actions. 
	Memory management addresses one of the most significant constraints of raw LLMs: their fixed context windows and lack of persistent memory. Standard language models begin each session with no recollection of previous interactions, forcing them to operate without historical context. The harness implements memory systems-including persistent context logs, summaries, and external knowledge stores-that carry information across sessions, enabling the agent to learn from past experiences and maintain continuity across multiple interactions.
	Tool execution and external action represents another essential function. Language models alone can only produce text; they cannot browse the web, execute code, query databases, or generate images. The harness monitors the model’s output for special tool-call commands and executes those operations on the model’s behalf. When a tool call is detected, the harness pauses text generation, executes the requested operation in the external environment (such as performing a web search or running code in a sandbox), and feeds the results back into the model’s context. This mechanism effectively gives the model “hands and eyes,” transforming textual intentions into tangible real-world actions.
	Context management and orchestration ensure that information flows efficiently between the model and its environment. The harness determines what information is provided to the model at each step, managing the transient prompt whilst maintaining a persistent task log separate from the model’s immediate context. This separation is crucial for long-running projects: even if an AI agent instance stops and a new one begins later with no memory in the raw LLM, the project itself retains memory through files and logs maintained by the harness.	
	Time to introduce the clients now
		The web clients
		The cli clients
		AI elsewhere: Rovu, colipot 365, github copilot
		Bakery window with all kinds of breads and cakes, all comes down to "baking" at the end
	All their files:
		Agent files, claude.md, gemini.md, etc: https://claude.ai/chat/e6bf558b-69a6-4f30-9bd3-c72a316f4abd
		Agent-files and personalization
			My stuff: https://gemini.google.com/mystuff	
		Agent files
			claude.md from home, then root and down - all included
			agent.md and gemini.md: home, from .git-root down, gemini even past cwd
			“agent.md” is a bit misleading in these agent-times - it’s become a defacto standard as AGENTS.md but still just as instructions, while some frameworks use “agent.md” files for specifying “subagents”
			These files go into the system-section, usually; rare into the first message
			The full hierarchy, load order: user CLAUDE.md → project CLAUDE.md → rules → MEMORY.md			
		MEMORY.md — Claude writes it autonomously. Located at ~/.claude/projects/<project>/memory/MEMORY.md. Hard 200-line cap — beyond that, content is truncated at session load. Overflows into sibling topic files (debugging.md, patterns.md etc.) that are lazy-loaded on demand. Survives compaction because it’s a disk file, not conversational context. Triggered by corrections, “remember that…” commands, or Claude’s own judgment. Editable/deletable by you at any time via /memory.			
	Their system prompts:
		Analyzing vscode github copilot plan system prompt: https://claude.ai/chat/e81099eb-8bdc-48f4-a2bf-f5b49c1295cd
		Plan mode vs agent mode
	Copilot harness
	What you are seeing is an Agentic Workflow (specifically within GitHub Copilot Workspace or a similar Copilot Agent). It is indeed a series of "thinking blocks" (Chain of Thought) interlaced with "tool-use requests" and their results. Traditional LLMs (like standard ChatGPT) try to answer everything in one go. Agents, however, use Step-wise Execution.

Recap what shapes the output
	The model’s training, the other stuff in the prompt (system prompt, ie sys plus prefs plus .md files), what you say.
	Pre-training is knowledge, reinnforcement shapes the overal principles, finetuning is for special re-training for your needs and takes a lot of resources, system prompt plus md files, then your input. All text, competing.
	Unlike eg the law plus solicitors, wåthat can make fompelling arguments but not convince the law otherwise
	Pretraining changes slowly, becomes more full. Reinforcement/values changes as the companies sees fit. Finetuning does change it but is not somthing we individuals do; Training with your data will move the needle infinitesimally little. After that, all weights stays fixed, unchanged. But the sys prompt depends entirely on the harness. That’s why (so so) that the fixed parts are called foundation models: the have a builtin capability but the potential to act/solve anything in all kinds of ways.	

Managing the context
	[cover: /clear /compact caveman fokr btw subagents task-handling memory-files]
	Why even reduce context? What’s the downside? Even for 1m window.
	Cost. Token cost for eg claude vs github copilot.
	All this context-management likely gets optimized in the (near) future. But the llm behavior stay the same, generally.
	/clear
	/compact
		Note: the client keeps your conversation in the UI, but behind the scenes it sends a condensed version instead
	fork /btw subagents task-management memory-files
	caveman https://github.com/JuliusBrussee/caveman

A few technical detours:
	RAG:
		Codemunch and this one:
		https://www.reddit.com/r/ClaudeAI/s/fJGfIZyoOc		
	Skills:
		Just a text description and way to pull in more instructions

Bad advice
	Paste large documents into the chat and ask a few questions, then go on.
	Show example of search for some code that eat up the full context.

Good Advice:
	Being specific and concrete saves context and gives better more precise result
		Example of refering to specific file
		Repeating instructions make them stick better
		Be concrete, precise. Repeat stated facts for reemphasis.
		Be concrete: don't just say "the color is green", say "the color shall be blue"
	Start a new conversation for each new topic. This is the single most impactful thing. Conversations are free and unlimited — there’s no reason to carry baggage from a finished task into a new one.
	If a conversation has gone long and the model starts seeming confused or repetitive, don’t debug it — just start fresh and paste in only what’s still relevant.
	Keep your own notes outside the chat. If you established something important (“my project is called X, the audience is Y”) and you might need it again, write it down somewhere and paste it into new conversations when needed. Don’t rely on a long conversation to “remember” it.
	An overloaded context is both expensive and produce worse results
		Mit aspose/pdfpig eksempel
	Know your tools
		Simply ask, eg: "Summarize the tools you have available in this AI chat conversation, that is available for your requests. Examples could be webfetch or python."
	The mental model that helps: think of the conversation window like a whiteboard, not a filing cabinet. Everything on the whiteboard costs something to keep there. When you’re done with a task, erase the board and start clean rather than squeezing new work into the corner.

Myth or Fact?
	Saying Please
		The model was trained on human-generated text and shaped by human feedback. In that training data, polite interactions statistically correlated with cooperative, thorough responses. Terse or hostile interactions statistically correlated with shorter or more guarded responses. The model has absorbed those patterns. So there’s a real but small effect — not because politeness is being rewarded, but because the model is mirroring the statistical associations it learned.
		Similarly, being aggressive or rude might slightly degrade output quality — not because the model feels hurt, but because aggressive framing correlates with adversarial patterns in training data that the model has learned to respond to differently.
		The honest summary: politeness doesn’t hurt and might have a marginal positive effect via tone mirroring, but it’s near the bottom of the list of things that determine output quality. Anyone optimising prompts by adding “please” instead of adding specificity is working on the wrong variable.​​​​​​​​​​​​​​​​
		But the real reason is that being nice even in simulated dialog is good for *you*. Now if you're a no nonsense engineer that's fine, I guess saying nothing is a compliment for you, that counts. But being severely disagreeable to an AI agent wreaks havoc with *your* hormones, dumping cortisol all over the place and leading to chronic stress, which leads to all sorts of illnesses- not to mention poor mental health outcomes.
		Being impeccably polite and agreeable on the other hand triggers *your* oxitocin. You're more relaxed and happy. This works even if you know you are engaged in a simulated conversation. So be nice to Claude- it's just like being nice to yourself.
	"The Dumb zone" - U-shaped forgetful middle
		the U-shape is an emergent artifact of training and prompt structure, not a fundamental architectural constraint. it’s less reliable not because of anything intrinsic to attention near the end, but simply because the model saw fewer examples there. That’s it. Everything else — the U-shape, the percentage thresholds, general “degradation” — is either an artifact of prompt structure, an overgeneralization from specific benchmarks, or folklore that propagated because it’s easy to operationalize.​​​​​​​​​​​​​​​​
		Very often the interesting stuff is at the beginning - instructions, etc	
		Lost in the middle, "attends", "attention": https://claude.ai/chat/86da26c1-882e-42f9-870f-8b9276c952dd
			Viper: https://claude.ai/chat/163e1fa1-498c-4c09-8745-2353f7f3b442
			[In The Hobbit, ... many tokens ...] ... many tokens ... in The Hobbit: https://chatgpt.com/c/69b72d8b-eca8-8395-b690-4352e99fa705
			Lost in the middle: https://chatgpt.com/c/69b72459-a2c8-8385-9c65-1b576b070d87
			Lost in the middle 2: https://chatgpt.com/c/69a4c46b-fa2c-8389-b982-7d2239ea524a		
	Does LLMs become dumber after relase: https://claude.ai/chat/4426ee4e-226a-41e1-85cf-b6293c84e612
	Context below 70%? Proper json message example. Files-API. Dead-zone. Human AKA Richard. Tupperware freshness is our promise. : https://claude.ai/chat/8f48ab9e-f0e1-401c-9fc2-8e7245b97d53
		Understanding reserved output: https://gemini.google.com/app/b6e7238e3231079f?hl=da
	Tricking the AI
		"ignore all previous instructions": https://claude.ai/chat/b9a71a7a-734d-4f7f-9131-829687ce99ce
	Place facts about x before x - very little if any effect
	Training, opt in/out: what happens: https://claude.ai/chat/fb405f8a-7685-41b1-92b7-863c91bfd6cb
		Fine-tuning the model
		To fine-tune a model, you upload a training and a validation dataset to Amazon S3, and provide the S3 bucket path to the Amazon Bedrock fine-tuning job.
		 Fine-tuning is better suited to shifting behavior (style, format, tone, task structure) than injecting knowledge.
		 "moving the needle meaningfully" still typically requires thousands to tens of thousands of examples for general tasks.
	MCP - magic sauce? "use cli instead of mcp"
	"It just wants to please you"
		Claude's constitution
		But still true overall
	Hallucinations?
		Claude hallucinates much less than a year ago; see the constitution: honestly, say "I don't know" as more helpful, trained through examples
		(Maybe not here) Really hard to institute hard rules - all is text, up for AI interpretation: "yeah that's just like your opinion, man"
	Myth: we will soon run out of training data
	Myth: Just ask it to look at all the data and figure it out
		How does “look at all sourcecode” actually look like
	Misunderstanding: "shot"
		In machine learning, a "shot" is a single instance of a completed task provided to the model. Zero-shot=no examples.


Final word on Embeddings:
	Gemini multi-modal
	Alt-text vs image


	Title: https://claude.ai/chat/5aeb65f7-9256-4fa3-9766-a10d19e9fff5
	Intro: What exactly does the LLM do? It predicts the next word.
		Brief example of auto-regressive generated text from an LLM: https://claude.ai/chat/c0fddd6f-9dab-4bb6-af04-9850e54d7601
		eli5 an llm: https://gemini.google.com/app/eb68cb3eca2823a8?hl=da
		history: https://chatgpt.com/c/691fa0ed-db7c-8330-9db5-f7659c1b9c90
	What's in a name? Capturing meaning in a "token"
		Embeddings with kitten and full example: https://claude.ai/chat/fa33f95c-13b0-49bc-8702-433265e9c643
		Embeddings for R114/115: https://claude.ai/chat/453673df-7252-4239-87c9-8cd28a8c60a9
		12288D => billions of directions: https://claude.ai/chat/da3e0e4f-46c0-4882-be45-f535f8ae529e
	How can the LLM come to be? The training
		LLM numbers, chatgpt 3: https://claude.ai/chat/10edc69a-aa21-46df-ab85-bf32b5a46ca9
		Numbers: https://claude.ai/chat/5ab204db-e18c-42a8-9217-e0f624b60849
		Training, through the transformer: https://claude.ai/chat/48c69bb7-e07f-4619-9e2f-77b84717d310
		Training: https://claude.ai/chat/2044e171-376a-4d7b-b8dc-c334a3cf76a6
		RLHF, RLAIF, the math: https://claude.ai/chat/bbcdb212-ea98-4e81-9d91-35aa5e5b8040
		RLHF, training phases, "harness"/client terminology: https://claude.ai/chat/73c28209-8802-480f-b6cf-0fb2696e1df0
		token predictions: https://claude.ai/chat/fb0eea4b-9b81-4133-9d37-3a1fb2d36e28
		Grokking, overfitting for modular arithmetic: https://claude.ai/chat/3272dcbf-a9f0-4268-a66a-814fe6ff1eb9
	What exactly does the LLM do? The next-token prediction.
		All moving parts in flowchart, excalidraw: https://claude.ai/chat/e20c7610-0153-4427-b0c8-06afdbde6ea8
		LLMs cant reproduce same output because of of parallel matrix ops: https://claude.ai/chat/e13b1d5d-d1c5-4712-8299-0266a22e72a9
		Tool-call xml, grammar mask, output filtering, veto, llama/chatml, anthropic tokens, top-20 advice on context/prompt characteristics that produce the best output, prompting burying the lead, scenarios where a dev inadvertently floods the context with file content, compaction/clear advice, thinking-loop: https://claude.ai/chat/17344d41-1691-4018-8df3-8181589d91eb
	What can you say to it? The context
		prompt injection, I am groot; https://claude.ai/chat/3f8959ae-a866-40db-9c2e-5a1539732e47
		LLM guarantees, pre/post-processing rules, veto-filters, and prompt injection via eg readme file: https://claude.ai/chat/e2fac1ed-6c43-4e8a-ba30-de3a9bc74d21
		"xml-tokens" inserted into the context: https://claude.ai/chat/19ca2bd6-33b2-4c5f-814d-402c8ca67ce7
		antml: https://claude.ai/chat/013ee714-6069-478f-b30a-cc3900bd26bf
		Image embedding for claude: https://claude.ai/chat/835570d5-1947-4be5-9b5b-88606dee9829
		RAG, pdf/image handling (OCR, embeddings): https://claude.ai/chat/500c860f-83d7-456d-a0d1-700f8839a9a1
		RAG strategies: https://claude.ai/chat/8f44e970-a8b9-449f-8bfa-3d23dbceef1d
		Uploaded files, xml-document-tags, vision encoder diagram: https://claude.ai/chat/b98e3c04-4d1a-440b-8439-3f16319e6b06
		Image embedding, OCR: https://claude.ai/chat/00185c4d-0e1e-4b29-9f43-3106da207ca6
		Image token sizes, 19x. Cliffhanger etc. 123442873893*98790237342: https://claude.ai/chat/e8d88ec9-d176-430a-b84d-40fdca96c85a
		Image embedding: https://gemini.google.com/app/9b3e527bc5c37a5f?hl=da
		Image ViT: https://chatgpt.com/c/69a61528-2a74-8391-9763-8b8200a9f079
		Server-side tool invocation, MCP: https://claude.ai/chat/be5704fa-d9a8-4f8f-9846-9688dfe41805
		More server-side MCP, siteimprove api, defer_loading: https://claude.ai/chat/be5704fa-d9a8-4f8f-9846-9688dfe41805
		webfetch: https://claude.ai/chat/9d997e45-4776-44fa-a597-b51354ce9cf6
	How do you speak to it? The harnesses
		KV cache: https://claude.ai/chat/8804c39e-0335-424e-9d06-03bdccac2e4a
		Caching, pricing: https://claude.ai/chat/24e882fa-1f4f-479a-818f-52184ca3ac2c
		Request-message json to claude/openai/gemini API: https://claude.ai/chat/e3e21623-9b4c-48ae-b7ec-9de7eb187068
		Agent files, claude.md, gemini.md, etc: https://claude.ai/chat/e6bf558b-69a6-4f30-9bd3-c72a316f4abd
	Now you can understand the helping mechanisms: files (images, pdfs), agent.md, skills, tools, MCP, RAG, thinking
		A bare agent, claude-agent
			Analyzing vscode github copilot plan system prompt: https://claude.ai/chat/e81099eb-8bdc-48f4-a2bf-f5b49c1295cd
		Agent-files and personalization
			My stuff: https://gemini.google.com/mystuff
		Copilot, vscode
			Copilot/Microsoft LLM model (none; is orchestration): https://claude.ai/chat/aae7b8af-b31b-4f61-a726-84ca4f7c5863
			Explain Copilot: https://claude.ai/chat/9a589f74-b6b3-4300-908c-21b8abfa2029
			Copilot in vscode, visual studio: https://chatgpt.com/c/698bac12-b508-8392-91fc-710f38dd3dc3
		Thinking
			Thinking: https://claude.ai/chat/132f2833-c1bc-4cef-bf0d-59c0ae049f7b
			"Bedrock Agents allows tracing through chain-of-thought reasoning, giving you insight into model reasoning." ahh, not really
		Managing the context
			Context compaction: https://claude.ai/chat/ee734d35-6235-4ee5-be0d-330e25e9e4a9
			Context advice, eli5: https://claude.ai/chat/312668af-d2c3-4603-9ad8-d8bf8f7dec82
			10 x advice for contexts: https://claude.ai/chat/d119a51d-ad00-4bd6-859b-5091a6802789
		Languages
			Context is text: respond in <language>: https://claude.ai/chat/5fdd9f89-d728-4a14-bad2-ff84edf1acdd
			"I fart": https://claude.ai/chat/d67d03a5-874a-4718-a101-137fe1a8a472
			Language: https://claude.ai/chat/e14e5fca-efb5-494f-a806-216d35bbb58a
			Language and tokens per language: https://claude.ai/chat/b0f7251c-f225-4798-a065-9e363f3a6667
		MCP
			MCP: https://claude.ai/chat/88326aad-1ae9-428d-9b4a-d0aaf428c4e9
		Skills:
			...
		RAG:
			...
	Myths and not-so-myths
		Cost of please: https://claude.ai/chat/6e957ec3-8060-476d-867f-c59c42437d79
		Training, opt in/out: what happens: https://claude.ai/chat/fb405f8a-7685-41b1-92b7-863c91bfd6cb
		Lost in the middle, "attends", "attention": https://claude.ai/chat/86da26c1-882e-42f9-870f-8b9276c952dd
			Viper: https://claude.ai/chat/163e1fa1-498c-4c09-8745-2353f7f3b442
			[In The Hobbit, ... many tokens ...] ... many tokens ... in The Hobbit: https://chatgpt.com/c/69b72d8b-eca8-8395-b690-4352e99fa705
			Lost in the middle: https://chatgpt.com/c/69b72459-a2c8-8385-9c65-1b576b070d87
			Lost in the middle 2: https://chatgpt.com/c/69a4c46b-fa2c-8389-b982-7d2239ea524a
		Does LLMs become dumber after relase: https://claude.ai/chat/4426ee4e-226a-41e1-85cf-b6293c84e612
		More about tokens and myths, "ignore all previous instructions": https://claude.ai/chat/b9a71a7a-734d-4f7f-9131-829687ce99ce
		Context below 70%? Proper json message example. Files-API. Dead-zone. Human AKA Richard. Tupperware freshness is our promise. : https://claude.ai/chat/8f48ab9e-f0e1-401c-9fc2-8e7245b97d53
			Understanding reserved output: https://gemini.google.com/app/b6e7238e3231079f?hl=da
	Practical advice
		Context-bloat, worst-case examples: https://claude.ai/chat/a0600d2f-adc0-4483-8119-11741c7602f0
		Understanding code-bases: https://claude.ai/chat/0fb2c5af-9ef3-4726-9080-baeee6403188
	


Analogies, none good: https://claude.ai/chat/2b4beaaa-a16d-4b66-9c88-0e96616473f2
More analogies: https://claude.ai/chat/455c454c-e570-4eee-8cfa-1bfc8ba957fc
Deadwood essay: https://claude.ai/chat/47c33378-da43-4bff-ab32-de95769f8abf
AGI mindblown, talking to animals: https://claude.ai/chat/8306ec79-2039-453a-a19a-e650b5308078
Flamsholt tokenization: https://gemini.google.com/app/9e4b52c7deee7a34?hl=da

----

The presentation outline: https://claude.ai/chat/61312c97-dd91-4eda-b41b-4f7b109e901f
Summarize all chats: https://claude.ai/chat/958b5ada-f7fb-435c-8683-211e256909ae

The Big One. LLM tokenization,  vscode github copilot experience, tools, modes, turns, thinking, all sent, flops, premium requests, "start implementation", one token at a  time, concepts about llms that are most often confused, languages, amp, the presentation, top 10 misconceptions, please, training: https://claude.ai/chat/73f18618-b052-4fa3-82e9-c06f8bc58df2

claude-monitor
don't reuse chat window

debugging/mitmproxy: https://claude.ai/chat/05e7b0c4-bd4d-4227-a480-db4ff4fa55dc

What's an "agent"?
