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


Intro
	Years ago, Java bytecode and .NET MSIL. Lead to Eqatec profiler, leading .NET code profiler in the World. Another story for another tech talk.
	Some things just grabs me. A feel a hunger to know about it. A hunger I wish I had for SQL databases and Amazon permissions.
	AI grabbed me. Over the past half year I've indulged in digging deep into it and in doing so I've identified some few core principles than people often don't know and where knowing them would make you so much more proficient in dealing with just about any kind of current AI-service.
	Making this talk has been problematic because I have sso much to say I could speak the entire day. So though you think I'm throwing a lot at you, it's really the condensed version. And it's the parts that will produce some "aha" moments, allowing you to reason about how you're using AI from first-principles. clarify the few simple core ideas.
Misconceptions every day
	Every day I hear misconceptions. I've sufferede from some severe misconceptions myself. Even Nate said the "convert pdfs to markdown" myth.
	I don't claim to know everything. Not all the math, definitely not the various agent-workflows. But I now know quite well how the core functionality works, and it's been eye opening.
	I hope to clear up the myths, to explain why "context is everything", and hopefully the added enlightment will be greater than the added confusion. We'll see.

Overview
	How the LLM works, trained, and what an embedding is.
	What's the scene on which the LLM performs?
	How you're using the LLM, meaning: the things you do, what effect does that have?
	Knowing that, let's walk through myths and misconceptions
	Finally, give practical advice


Overview
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
	An embedding is a high-dimensional set of characteristics, the very essense of a word. Super-important concept!
	12288 dimensions. More dimensions, more room to encode nuances
	the attention layers, all tokens attend to previous tokens => n^2
	the final token has absorbed, soaked up, the full meaning of all the tokens before it, ready to find the likely next token
	The special tokens, can't be in the text: eg stop, turn-taking, etc.
	Takeaway: the model only understands and can process tokens.


The LLM
	"It's just a fancy auto-complete" - yes, more accurately than you'd think
	Brief example of auto-regressive generated text from an LLM: https://claude.ai/chat/c0fddd6f-9dab-4bb6-af04-9850e54d7601
	The core part of the LLM, the Transformer, can do one thing: take a text and produce the most likely next word
	Already I'm lying: take sequence of tokens and produce likelihoods for the next token
	Go to tokenizer: here's how the tokens for the sentence looks like
		The list of tokens to auto-complete (auto-regression) is the context, and it can up to 1M large now; 200K for Anthropic [table]
		Meaning that it can autocomplete 900,000 word long text. Mind blown!
	What do I mean: most likely next word?
		Inside the LLM there are 96 attention-layers with billions of weights; GPT 3.5: Vocabulary ~100K (cl100k_base tokenizer), 96 layers, 96 attention heads, 12,288 embedding dimension, ~175B parameters, 4K/16K context window.
		For a completely fresh model, the weights starts out random and are then trained in two steps
			Pre-training: fed billions of text examples through, adjust weights. Cost XXX$, takes xxx petaflops. Essentially statistical autocomplete. All models are roughly the same. Training data — the quiet giant.
				GPT-3 was trained on ~300B tokens. Llama 3.1 used 15.6 trillion. DeepSeek-V3 used 14.8 trillion
			RLHF, RLAIF: Now trained to produce the desired outcome. This is where the models differ!
				Anthropic's constitution
				Grok's "truth-seeking", un-woke
				This is where “be helpful, harmless, and honest” gets baked in, where Claude gets its collaborative personality, where ChatGPT gets its particular hedging style.
			Artificial tokens: https://claude.ai/chat/19ca2bd6-33b2-4c5f-814d-402c8ca67ce7 
		After all this the weights are settled in to produce the likely next token. Some takeaways:
			Yes, you really do feed it text (chunked up into tokens)
			It's all statistics plus adjustments for desired outcomes
			Nobody has ever manually said "Denmark is a kingdom". No built-in rules or dictionaries.
			It's one big, read-only, stateless, mechanical, pure-function, transformation-box of, say, 700 billion factors: put tokens in, get token-likelihoods out.
			You can download these models [show list]
	I want to add one more part to make it a bit more functional: Keep on producing tokens until the model says stop. Now it can generate a sentence.
	Takeaway:
		the LLM model embodies a vast set of "knowledge" with behavior highly shaped by the RL-training. "I drink and I know things": https://tenor.com/view/buttmarnn-gif-26886582
		The LLM can only reason about the context. Absolutely nothing more. No links, no nothing. Text in, next text out.

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
		claude.md / gemini.md / agents.md etc
		personalization, memories, ...
		[current file, selected text, terminal output, etc]
		Also, the difference between plan and agent mode
		Prompt injection examples: I am groot.
		How big is it? Show some examples.
	Tools
		LLM has built-in narrow set of tools: web_search, web_fetch, code_execution; OpenAI has a debian instance with Python 3.11, Node.js, Java, Go, and Ruby pre-loaded
		tool-use, tool-result
		"show me your tools"
	MCP servers
		Just a place where the client can fetch a list of tools, nothing more
		But can also be given to the LLM for it to fetch the tools
	Messages - finally
		user - assistant, taking turns
		Show real plain text example
			Show Human AKA Richard example
		Plain text - just goes in
		Images - not likely OCR, split up into patches, each turned into an embedding, then re-projected into text space
			Size difference in output
		Documents - turned into text or images - Gemini is superb at multi-modal. Extract PDF text beforehand? No.
		Lots of documents - gets RAGged and relevant bits are uploaded
			"Browse my entire repo and ..." ### this one is important!
		Thinking
	Temperature
		temperature - sampling
	KV-cache
		Little caching points
	Safety classifier
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
	It all goes through the transformer. All of it, as tokens. No fixed rules. Only trained behavior. Hence, prompt injection.
	Thinking doesn't make the model "think harder". It just produce thoughful pondering course-correctioning outputs before the final answer, ie not one-shotting it
	The only parameter you can really control is the temperature

Managing the context
	Why even reduce context? What’s the downside? Even for 1m window.
	Cost. Token cost for eg claude vs github copilot.
	All this context-management likely gets optimized in the (near) future. But the llm behavior stay the same, generally.

Anti-advice
	Show example of search for some code that eat up the full context.

Myth or fact?
	Place facts about x before x - very little if any effect
	Saying Please
	U-shaped forgetful middle
	Thinking mode
		 const thinkingBudget = prompt.includes("ultrathink") ? 31999 : 0

Beliefs that holds up:
	Being specific and concrete saves context and gives better more precise result
		Example of refering to specific file
		Repeating instructions make them stick better
	An overloaded context is both expensive and produce worse results
		Mit aspose/pdfpig eksempel
	

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
