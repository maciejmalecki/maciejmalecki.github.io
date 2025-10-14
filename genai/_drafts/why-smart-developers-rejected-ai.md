---
title: "Why Smart Developers Rejected AI Coding Tools?"
layout: post
categories: genai
description:
pic:
---
I have been an AI enthusiast for some time and started out by using ChatGPT for simple code generation tasks. I then made several attempts to use coding assistants, initially finding them useful, but always reaching a point where I could not understand them. However, as I am an enthusiast, I worked out the problems and started to develop usage methods. But this is my experience. Generally speaking, I see a rather different attitude: software developers, even the more experienced ones, quickly become frustrated after a few initial attempts and abandon the tool (and sometimes the whole Generative AI technology) as useless.

Initially, I was puzzled by this attitude, but after some time, I realised that they were right. There must be a general problem with early AI coding assistants — not only with their limited and rudimentary functionality, but also with their poor user experience, lack of expectation management and frequently changing implementations. We were all dealing with software in 'experimental' stage of development.

## The four tribes of AI adoption

The Generative AI era in software engineering highlights that the difference between junior and senior developers is significant. This difference is not only related to skills and experience, but is also often connected to generational gaps, cultural differences and the ability to apply formal or engineering methods. AI-driven tools are certainly impacting the way we deal with knowledge, how we learn and even whether we learn. Therefore, there is no single answer to the question of AI-related sentiments; we need to consider at least four groups of people.

![Types of AI Users](/genai/img/types-of-ai-users.excalidraw.png)

Let's start with the enthusiasts. I am in a group called 'seasoned enthusiasts', for example. You could call them seniors, or something similar. It's a group of people with significant experience in software engineering. These people clearly recognise the potential of large language models. Some of them may be aware that their profession is actually the main driver in the development of this technology — big tech companies have invested billions just to address the problem of a relatively small pool of well-paid specialists. Seasoned enthusiasts have accepted this reality; they are aware that there is a risk of being replaced by machines. Some see this as an opportunity to quickly upskill in AI coding, while others are adopting a 'if you can't beat them, join them' strategy (with exactly the same rationale).

Switching to the 'junior enthusiast', who has much less experience but is nonetheless trying to use AI in their daily work. If they are lucky, they have a good AI leader around to help them learn how to use the tools properly. If not, they have to experiment and learn through trial and error, which can be difficult (I will explain why below). However, even if they are only using the chat function (or 'ask mode' in some terminology) to ask questions related to problems with their code or technology, this may still be more efficient than searching for answers on Google or reading through Stack Overflow.

Now, let's focus on 'junior sceptics'. Most of them have tried, but after some initial success, they were set back by non-functional software or devastating code reviews performed by fellow engineers. They most likely revert to classical methods, including Stack Overflow, which will most likely become obsolete sooner or later. 

Last but not least, my favourite group is 'seasoned sceptics'. I was perhaps there twice, especially when it came to coding assistants. I started using a leading tool from this group, but then quickly reverted to a plain AI chatbot and the copy-and-paste technique because I noticed the earlier one was random. I had the same experience when I first tried a multi-agent tool and then reverted to a more classical approach. In fact, there are two kinds of people in this group: those with very little knowledge of AI, particularly Generative AI, and those with a good awareness of AI who nevertheless try to turn back the clock by neglecting to use any AI in their work, fearing (and with good reason) that they will be eliminated from the market sooner or later. 

## Main reasons of rejection

**The technical limitations** of the early coding assistants were significant. Early large language models had a very limited context window size. This, of course, made working with them challenging, as even a medium-sized project requires a lot of context data, such as the domain description, auxiliary documentation, and, most prominently, the code base. 

The creators of these tools didn't attempt to deceive us; they simply declared that they could not consume all our context, so they gave us the freedom (and necessity) to build one each time we needed to perform a task. Context building is still a live issue. Even with current LLMs accompanied by large context window sizes, processing everything does not work because recent models suffer from the [lost in the middle][lost-in-the-middle] phenomenon.

Building up a context with open tabs and, later, with the introduction of the chat function to coding assistants, attaching it to the conversation was tedious and error-prone. Even then, it wasn't really used properly, if at all. Should we blame the developers? I don't think so — it's an unusual and surprising way to use their favourite IDE.

Before thinking models were popularised, there was another limitation that was perhaps even more annoying. Tools had very limited planning capabilities, if any. In reality, all planning and more complex design work was intended to be done by humans. This meant that, except in very special cases such as implementing a simple video game, it wasn't really possible to prompt a coding assistant with a high-level requirement description and expect a working result (even if the context was properly established).

Of course, for typical professional work, planning and design can easily be the most difficult and time-consuming part of a developer's job. While coding assistants can assist to some extent, they are unable to combine this kind of work with exact implementation tasks — certainly not in one go.

**Expectation misalignment** was largely introduced by enthusiasts when someone was fortunate enough to implement simple tasks for AI at the time. Together with bold claims regarding the 'end of the coding era', this popularised the idea that 'the new programming language is English'.

> **Snake Game phenonemon**
>
> There are plenty of examples of people implementing a snake game from scratch using ChatGPT alone. This suggests that we are very close to a time when it will literally be possible to generate working software without any programming skills. You can watch a few examples on YouTube.
>
> Driven by this example, many have tried to replicate the same approach in their own projects, but have failed. Some insight into this can be gained by taking a quick look at one of the largest collections of open-source repositories: GitHub. A simple search reveals that there are over [450 public Python source code repositories][gh-snake-cases] available containing some form of snake game. So perhaps this, plus many more examples from other sources, formed part of the huge training dataset used to develop the language models employed by chatbots and coding assistants?

This approach worked for simple, common coding tasks, such as implementing a basic video game. However, it is not necessarily applicable to our daily work as developers implementing complex business systems. This is because such systems are a mixture of common parts, such as never-ending screen-to-DB modules (also known as CRUDs), and unique, mission-critical parts that are usually implemented only once for a given client. Therefore, it is highly unlikely that a simple, naive prompt could easily evolve into complex, functioning software that works exceptionally well for a specific business.

The major expectation was always planning capability, which early AI tools lacked. Users expected a top-down approach, but a more rudimentary bottom-up approach was all that was available.

![Bottom up AI assisted method](/genai/img/bottom-up.excalidraw.png)
*Bottom-up, AI assisted software development method.*

When the bottom-up method is executed properly, it is found that even AI-assisted code completion is sufficient for the majority of low-level coding tasks, such as implementing functions or classes based on a contract-driven prompt or rapidly generating unit tests. However, it took a long time to reach this point, with very little AI assistance initially available.

**The knowledge gap** was then the missing link; the technology itself was complex, only masked by the apparent simplicity of the AI tooling. The following simple diagram usually immediately raises questions about how AI is fed all this missing data from your project artefacts. But without adequate training or at least reading a few articles on the subject, there is no way you could know that.

![LLM in a nutshell](/genai/img/coding-assistant.excalidraw.png)

It turned out that the way you [prompt][prompt-engineering] is even relevant. There are plenty of techniques supported by most models, such as in-context learning and chain-of-thought, that can be used to improve model efficiency. However, to master these techniques — or even be aware of their existence — you need proper training. Because AI technology evolves very quickly, a single training session may not be sufficient.

There is no excuse for not understanding how the underlying mechanism works just because you are a user. It helps to know that there is no magic involved and that models simply process natural language. You need to provide context in order to have anything to process. Once you understand this, you will know what needs to be done to avoid hallucinations. In other words, provide enough information and be precise to get meaningful results.

An interesting report was recently published by [METR][metr] stating that, contrary to popular belief, seasoned OSS developers actually performed more slowly with AI than without it. I strongly believe this will be explained sooner or later, but some suspect the cause may be a lack of experience with the selected AI tools and a lack of in-depth AI knowledge among the evaluation group. 

Although Miguel Grinberg does not lack knowledge, as he stated in his blog [post][ai-is-not-faster], it could be a lack of trust in the technology driven by previous failures and misuse. Who knows?

## You need more of Top-Down approach

The missing piece is what everyone expected from AI: the ability to understand and implement high-level business requirements. The more AI is able to work, the greater the potential speed increase. If AI could seek context itself, plan, provide a design and cut the workload into smaller pieces to be executed step by step with only limited human input, it could become truly autonomous.

![Top-down VS Bottom-up](/genai/img/bottom-top-plain.excalidraw.png)

Designing and planning work for a new feature is undoubtedly the most demanding part of a programmer's job. You either do a big up-front design or progress through trial and error, perhaps supported by experience and intuition. The formal design phase has almost become extinct because it does not fit the 'agile way of working'. This process takes time, can only be carried out by a limited number of people (designers and architects), and significantly slows down 'time to market'. If this process could be automated, the design phase could be speeded up and become a more common practice in IT.

We can see that early AI coding assistants can be very effective in performing bottom-layer tasks, such as the implementation of classes, interfaces and functions, and the mass generation of unit tests. These are all areas in which GenAI has always excelled. Without support for thinking, planning and building context, early coding tools were limited to 'bottom-up', and this was the only context in which they could be used effectively.

But something fundamental has changed: the realisation that adding a thinking process and seeking context was only a matter of time. No matter what we call it: Agentic AI, Coding Agent or Agent Mode — it's already here. Is that enough for a 'top-down' approach? We will soon find out. The capabilities are promising, as I mentioned in one of my previous [posts](the-rise-of-vibe-coding). In my next article, I will take a closer look at the agentic way of working.

[lost-in-the-middle]: https://cs.stanford.edu/~nfliu/papers/
[prompt-engineering]: https://en.wikipedia.org/wiki/Prompt_engineering
[ai-is-not-faster]: https://blog.miguelgrinberg.com/post/why-generative-ai-coding-tools-and-agents-do-not-work-for-me
[metr]: https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/
[gh-snake-cases]: https://github.com/topics/snake?l=python
