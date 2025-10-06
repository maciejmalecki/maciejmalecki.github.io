---
title: Are you still waiting for the AI bubble to burst? The train has already departed.
layout: post
categories: genai
description: Using AI in software engineering is harder than you think, and this is what marketing material are not tellying you.
pic: /genai/img/arnold-commando-ayn-rand-fasces.jpg
---

Nowadays, it is difficult to avoid being a sceptical AI user, especially if you are a software engineer. If you read the news carefully and follow tech-oriented social media accounts, you may notice a big discrepancy between the enthusiastic opinions about how game-changing the new technology is and how much code can now be generated with very little effort, and the pessimistic messages about working with AI being no faster, or even slower, than working without it.

This kind of dissonance is not unprecedented. Scepticism and enthusiasm were also evident at the time of the invention of the steam engine, the bicycle and the car. Nor can I say that I haven't witnessed this myself, because that would be untrue. Consider the invention of the personal computer, the internet and the mobile phone, and finally the smartphone.

The only difference with AI in software engineering is perhaps that I am personally involved. This is because ["AI can take my job"][when-does-ai] and because it is expected to generate unmaintainable code (because humans only develop highly maintainable, clean code, after all). Why are there so many mixed opinions? Let's take a brief look at this problem.

### The way we feel about AI

The Generative AI era in software engineering highlights that the difference between junior and senior developers is significant. This difference is not only related to skills and experience, but is also often connected to generational gaps, cultural differences and the ability to apply formal or engineering methods. AI-driven tools are certainly impacting the way we deal with knowledge, how we learn and even whether we learn. Therefore, there is no single answer to the question of AI-related sentiments; we need to consider at least four groups of people.

![Types of AI Users](/genai/img/types-of-ai-users.excalidraw.png)

Let's start with the enthusiasts. I myself am in a group called 'seasoned enthusiasts'. You could call them seniors or whatever. It's a group of people with significant experience in their profession --- software engineering, in this case. These people clearly recognise the potential of large language models. Some of them may be aware that the main driver in the development of this technology is actually their profession — big tech companies have invested billions just to address the problem of a relatively small pool of well-paid specialists. Seasoned enthusiasts have accepted this reality, either seeing it as an opportunity to quickly upskill in the AI coding area or progressing via a 'can't fight it, so join it' strategy (with exactly the same rationale). The real problem is that 'seasoned enthusiasts' are perhaps the smallest group in the population as a whole.

A much bigger group nowadays is the 'junior enthusiast', who has much less experience but is nonetheless trying to use AI in their daily work. If they are lucky, they have a good AI leader around to help them learn how to use the tools properly. If not, they have to experiment and learn through trial and error, which can be difficult (I will explain why below). However, even if they are only using the chat function (or 'ask mode' in some terminology) to ask questions related to problems with their code or technology, this may still be more efficient than searching for answers on Google or reading through Stack Overflow. Or even worse, watching tons of useless tech videos on YouTube.
Of course, there is a risk that these people will not develop the necessary skills. Even the simplest questions can be answered by AI, so there is no need to search for information, analyse it, synthesise it or apply critical thinking.

Now, let's focus on 'junior sceptics'. Most of them have tried, but after some initial success, they were set back by non-functional software or devastating code reviews performed by fellow engineers. They most likely revert to classical methods, including Stack Overflow, which will most likely become obsolete sooner or later. The existence of this group proves that a competent AI leader is essential for a project, team or company. Unless, of course, some of these people deliberately ignore AI in order to gain practical coding and engineering skills as soon as possible, thereby joining the 'seasoned' group.

Last but not least, my favourite group is 'seasoned sceptics'. I was perhaps there twice, especially when it came to coding assistants. I started using a leading tool from this group, but then quickly reverted to a plain AI chatbot and the copy-and-paste technique because I noticed the earlier one was random. I had the same experience when I first tried a multi-agent tool and then reverted to a more classical approach. In fact, there are two kinds of people in this group: those with very little knowledge of AI, particularly Generative AI, and those with a good awareness of AI who nevertheless try to turn back the clock by neglecting to use any AI in their work, fearing (and with good reason) that they will be eliminated from the market sooner or later. 

As I said, this scepticism is quite justified, especially when you read the following METR report stating that a select group of experienced developers actually reported a performance degradation when using AI coding assistants. Unfortunately, we don't know if these people knew how to use these tools properly. 

An interesting aspect is covered in [this article][ai-is-not-faster] by Miguel Grinberg. Miguel apparently has little trust in the code being generated, which is fully understandable. By reviewing each part of the generated code, he claims that he is not faster at all. This is a well-known problem with agentic AI, which I will discuss at the end of this article. It can perhaps be mitigated with an appropriate prompting technique, as I recently described in [The Rise of Vibe Coding][the-rise-of-vibe-coding].

The people in this group can play two roles. One positive role is that of critic and realist. But there is a negative role as well — that of the defeatist, who can spoil the atmosphere and break up your teams.

Now that we have sketched out the different actors in this drama, let's elaborate more on the reasons.

### #1 Overselling

According to [Wikipedia][gh-wiki], the first version of GitHub Copilot was released as a preview in the form of a Visual Studio Code plugin in late June 2021. Powered by the [OpenAI Codex][openai-codex] model --- a fine-tuned version of GPT-3 --- GitHub Copilot was capable of completing a programmer's thoughts as they typed code into the IDE. I am personally a big fan of AI-assisted code completion. The only problem with this mode of working is the very limited set of capabilities you can use to build context, and it is not as straightforward to phrase your intent --- you either have to name your identifiers very precisely or add additional 'prompts' as code comments.

Early versions of GitHub Copilot went largely unnoticed until [ChatGPT][chatgpt] was released at the end of 2022, using much the same underlying technology as Copilot. With GPT-3.5 and the subsequent GPT-4 capable of generating substantial and intricate pieces of code based on simple English (or even Polish) prompts, the potential of AI-assisted coding has become widely recognised. Coding assistants like GitHub Copilot were immediately revealed and remarketed. The only problem is that, once users of ChatGPT started using Copilot, they were totally puzzled because the only way to use it is still to prompt via 'source code' in the editor window of your IDE. Users noticed that the code completions were more or less randomly accurate and sometimes completely inaccurate. Nobody read the manual to learn that you need to build context. How? By opening the editor's tabs! How obvious...

The problem arose because some people were able to deliver impressive work, such as implementing a video game from scratch. This was then applied to all software engineering tasks, including development on existing code bases, some of which were legacy systems with significant technical debt. 

Tool providers started to claim that their tools could significantly speed up development, and the numbers were astonishing. In early 2023, a [paper][Arxiv-GH-CoP] was published claiming evidence that using an AI coding assistant speeds up task completion by an incredible 55.8%. The evaluators received a basic introduction to how to properly use the tool (GitHub Copilot). They were tasked with developing a HTTP server in JavaScript. From scratch, of course.

![You can do big things with little effort](/genai/img/arnold-commando-ayn-rand-fasces.jpg)
*'You can do big things with little effort' --- they said (the log is fake).*

Here's why the marketing behaviour of the leading AI tool providers has become problematic. It's because the 'evidence' includes the most typical and common programming tasks, which are represented in open-source repositories with tons of examples. It doesn't matter if it's a snake game in Python, a Tetris game in JavaScript, or an HTTP server implemented in any mainstream programming language. The aforementioned study describes the problem of writing an HTTP server from scratch, which is the easiest case for AI because you don't have to deal with an existing codebase to establish the context for a conversation. We usually work with an existing codebase rather than a new one, in about 95% of cases, if not more.

A simple search on GitHub revealed 643 public JavaScript repositories implementing an HTTP server (tagged with http-server) and 467 public Python repositories implementing a snake game. This is perhaps enough for Large Language Models to acquire complete knowledge during their training processes, which is why they can implement such typical cases flawlessly. However, this does not work for your project because, sadly, you are not tasked with implementing yet another snake game. The study contains a serious methodological error.

### #2 Complexity of the technology

Artificial Neural Networks (ANNs) as a scientific discipline are nearly as old as digital computers, originating in the 1940s. The theory slowly evolved without many real-life applications until the beginning of the 21st century. A few things happened at this time: the internet revolution provided relatively cheap access to large quantities of data and cloud computing started to offer flexible and scalable computing power at a reasonable price. Then, driven by the gaming industry's demand for more relevant technology, highly performant graphics processing units (GPUs) drove computation costs even lower (because vector graphics and neural networks essentially run on the same maths, so these dedicated integrated circuits could easily be reused). Then, with deep learning, machine learning led us to generative models and, finally, [the Transformer][attention]. 

Artifical Intelligence is indeed complex. It's a subject that most of us don't even try to familiarise ourselves with. One might say, 'We don't need to', and this is exactly *the second problem*. Ask yourself: **when filling up your car with fuel, doesn't it really matter whether you pour diesel or petrol?**

The problem is serious because, from the end user's point of view, the technology seems trivial. You mostly interact with a chat interface, or perhaps an AI coding assistant which also offers a chat interface. Code completion is also a well-known feature of IDEs. However, this time, code completion is no longer deterministic.

Given such a simple user interface, it's not surprising that people tend to use it in a naive way. They just type in a short, general statement as a prompt and expect miracles. No miracle occurred? The code doesn't work? Or even worse — MY GOD! IT HALLUCINATES!!!

![You'are using that wrong](/genai/img/commando-rocket-launcher-instruction-manual.jpg)
*You're clearly using that wrong!*

To understand the contrast between the simplicity of the user interface and the underlying complexity, take a look at the following picture:

![LLM in a nutshell](/genai/img/coding-assistant.excalidraw.png)

This is perhaps the simplest possible AI-driven chatbot, with the ability to upload documents (contextual data) and conversation history — after all, this is a chatbot. It has no web access, thinking process or deep research function. As you can see, the Large Language Model block is labelled 'built-in knowledge', meaning that it contains knowledge that was injected during the training and fine-tuning processes. However, this knowledge may be outdated and incomplete, and, most importantly, inaccurate. This is because LLMs are not document storage systems. Therefore, you will never get an exact copy of the training sample, but rather a paraphrase of it. Sometimes, when a fact was not sufficiently represented in the training data, you may experience an effect known as 'hallucinations'.

In order for an LLM to work reliably, you must provide context, i.e. all the information necessary to accomplish the task. In the case of coding assistants, this means providing all the source files necessary to generate new code or modify existing code. This includes all declarations, custom data types, classes, functions, etc. If you ignore this, don't be surprised if you get a random or erratic answer. At the beginning of the GenAI revolution, this context could only be built manually. If you used an IDE, you just had to open additional files as tabs in the editor. This approach was never intuitive; it was based on statistical analysis and the assumption that most of the required files had been opened recently and were still open as tabs. 

But why can't all source files be provided as context automatically? It's simple: initially, it was simply too much because each LLM is characterised by something like a context window size. This applies to everything: context data, conversation history (when using the chat function), and of course the prompt itself. Even with LLMs accompanied by large context window sizes, processing everything does not work because recent models suffer from the [lost in the middle][lost-in-the-middle] phenomenon.

It turned out that the way you [prompt][prompt-engineering] is even relevant. There are plenty of techniques supported by most models, such as in-context learning and chain-of-thought, that can be used to improve model efficiency. However, there is always a trade-off between prompt specificity and length.

This is the minimum information you need to know when using this technology. Diesel cannot be used to fuel a petrol engine. If you do, you will need to clean your engine, which will be a messy process. Similarly, if you don't understand how AI works, you will misuse it, produce poor results, and ultimately stop using it completely.

### #3 Poor usability

The worst thing a provider of AI coding tools can do is mimic functionality that isn't there. As we know, coding assistants were initially unable to build context automatically. Pushing all sources into the context window would overload it. Automatic context building was initially too difficult to accomplish, especially since coding assistants support a wide range of mainstream programming languages — parsing all 'imports' was not a viable option since the logic of 'importing' varies depending on the language. It would simply be too complex and costly to implement such a feature. The tool would lose its 'language agnostic' feature, and being 'language agnostic' is one of the selling points.

One might argue that, if the tool is limited in terms of context building and size, it would be better to make context management more explicit, offering the option to mark and unmark files as being included or excluded. The only way to manage context for code completion was to keep relevant tabs open. However, one of the market-leading tools retains recently closed files in the context. The only way to remove a file was to restart the tool. The worst thing about this is that inexperienced users might conclude that managing tabs — thus building the context — is no longer necessary, so they will skip doing this. I have witnessed this happening.

Another limitation was that early coding assistants were unable to perform complex tasks when using chat functionality, except perhaps for implementing a snake game. Unfortunately, from a user experience point of view, a chat window essentially invited a top-down approach, meaning 'specify the high-level requirements and the tool will change the necessary files'. However, coding assistants without agent mode were unable to do this!

### The way you were intended to work with early AI tools

The only reliable way to use early coding assistants was what I call the 'bottom-up' approach. This means doing the first part of the work manually — selecting the context by opening the relevant files, for example — and splitting the work into smaller steps. This involves creating or editing every single file in your project that needs to be changed, and asking the AI to fulfil only the most basic tasks at the bottom, such as implementing a class or a function, or changing a tiny part of the code. You can also optionally generate a unit test, which worked exceptionally well even with code completion.

With the 'bottom-up' approach, you get down to the function level and literally generate code there, either using code completion (which works surprisingly well here), or by entering prompts into the chat window. Alternatively, you can use the inline chat function, which is also available in tools such as Codeium (now Windsurf) and GitHub Copilot.

The problem with this usage strategy is that deciding what to include and what to exclude can be a lot of work. The same applies to planning the work and splitting it into easily digestible chunks for the AI. This is why reported gains from such tools are at most 20–30%, nowhere near the proclaimed 55%.

### Coding AI agent changes everything

Recently, most AI coding assistants have been enriched with something called Agentic AI. I feel this should have been there from the beginning. This is because most software developers dealing with AI have always acted intuitively, as all these tools have always been agentic.

This is because an agent can eventually plan work and split complex, high-level tasks into smaller steps. Agents can also seek context data by traversing the existing code base. These tools do this better or worse, but the difference is huge. Forget about tabs and attachments, at least when it comes to relevant source files. Having an agent in your AI coding assistant enables a 'top-down' approach.

As an added bonus, agents can integrate with tools and offer great flexibility. For example, if you use a build system such as [Gradle][gradle], it can analyse a build file, recognise its capabilities and use them to compile code, run tests, launch various checks, and so on during its run.

![Tools are getting better](/genai/img/John-Matrix-Commando-Schwarzenegger-e.jpg)
*The AI Coding Agent is well equipped with tools.*

The picture below illustrates why Agentic AI is making our lives so much easier. With a 'bottom-up' approach, most of the work is still done manually, unlike with a 'top-down' approach, where, if you're lucky, everything can be done by the agent with just a prompt.

![bottom vs top](/genai/img/bottom-top.excalidraw.png)
*Two eras of AI assisted software engineering.*

Of course, the advent of agentic AI does not mean that you can now ignore the underlying technology's complexity. Unfortunately, it is still not acceptable. There are many things that can go wrong. For example, context resolution may not be as accurate as it should be, particularly if you do not have good, consistent naming conventions. Tool output may be ignored, or tool launch may be omitted. If your quality assurance is not fully automated, you will still need to perform a lot of manual tests because code can be generated very quickly. If your static code analysis tools are not good enough, you will have extra work on reviewing generated code (a lot of code), in addition to the fact, that you may need to review it anyway (remeber [Miguel][ai-is-not-faster]?)

### What should I do?

The answer to this question depends on which group you belong to.

If you are a **junior enthusiast**, you are on the right track to becoming a seasoned enthusiast, but remember that you are now very exposed to the 'training on the job' cut-off. In any case, you need expertise in order to work with AI-generated code efficiently. Here's how you can use AI to boost your skills:

Use the 'Ask' function to ask questions and learn from the output (you can double-check on Google if you don't trust the answer).
Use the 'Ask' function or 'Agent Mode' to troubleshoot and analyse the output. Treat this as an ad hoc generated tutorial and learn from it.
If you're unsure about the code being generated, generate unit tests for it and analyse the test methods and invariants — it's often easier to analyse the tests than the code itself.

If you are a **seasoned enthusiast**, become a leader for your colleagues. Becoming a mentor is a great responsibility. However, it is also a lot of fun, as you will be the one discovering new methods and strategies for using AI. New language models pop up every month, so the game will never end.

If you're a **junior sceptic**, perhaps you gave up too early. Start with simple steps: generate unit tests for code that you have written, for example. Alternatively, you could try a TDD approach: hand-code an interface, then generate tests for it, and then use code completion to generate production code while keeping the interface and tests in context (opened tabs!).

For **seasoned sceptics**, I would advise taking a professional course in AI-aided software engineering. It may be that your project is especially challenging — the language is not mainstream, or the code base is bloated. My experience shows that Agentic AI can work pretty well with a messy code base and that, for a non-mainstream language, fine-tuning is a good solution. Alternatively, such dedicated language models may already be available. However, if you are considering trying to reverse the clock, I recommend against it. In my humble opinion, this trend cannot be reversed.

> **Disclaimer:**
> All photos used in this article come from the brilliant 'Commando', a VHS era film which celebrates its 40th anniversary today.

![Just bodies](/genai/img/leave-anything-us-just-bodies.jpg)
*'Just bodies.', John Matrix*

[when-does-ai]: when-does-ai-take-my-job
[ai-is-not-faster]: https://blog.miguelgrinberg.com/post/why-generative-ai-coding-tools-and-agents-do-not-work-for-me
[metr]: https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/
[gh-wiki]: https://en.wikipedia.org/wiki/GitHub_Copilot
[openai-codex]: https://en.wikipedia.org/wiki/OpenAI_Codex
[chatgpt]: https://en.wikipedia.org/wiki/ChatGPT
[arXiv-GH-CoP]: https://arxiv.org/pdf/2302.06590
[gh-http-cases]: https://github.com/topics/http-server?l=javascript
[gh-snake-cases]: https://github.com/topics/snake?l=python
[attention]: https://en.wikipedia.org/wiki/Attention_Is_All_You_Need
[lost-in-the-middle]: https://cs.stanford.edu/~nfliu/papers/lost-in-the-middle.arxiv2023.pdf
[prompt-engineering]: https://en.wikipedia.org/wiki/Prompt_engineering
[gradle]: https://gradle.org/
[the-rise-of-vibe-coding]: the-rise-of-vibe-coding
