---
title: "Why Smart Developers Rejected AI Coding Tools (And They Were Right)"
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

<!--
Technical limitations (context, planning capabilities)
Expectation misalignment (top-down vs. bottom-up)
Knowledge gaps (how to use effectively)
-->

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

**The knowledge gap** was then the missing link; the technology itself was complex, only masked by the apparent simplicity of the AI tooling. The following simple diagram usually immediately raises questions about how AI is fed all this missing data from your project artefacts.

![LLM in a nutshell](/genai/img/coding-assistant.excalidraw.png)

It turned out that the way you [prompt][prompt-engineering] is even relevant. There are plenty of techniques supported by most models, such as in-context learning and chain-of-thought, that can be used to improve model efficiency. However, to master these techniques — or even be aware of their existence — you need proper training. Because AI technology evolves very quickly, a single training session may not be sufficient.

<!-- to be spell checked -->

<!-- to be revised -->
**Complexity without transparency:** Artificial intelligence is indeed complex. It's a subject that most of us don't even attempt to familiarise ourselves with. The problem is that, from the end user's point of view, the technology seems trivial. Most interaction is with a chat interface, or perhaps an AI coding assistant that also offers a chat interface. Code completion is a well-known feature of IDEs, so nobody feels the need to understand what's going on behind the scenes. However, this time, code completion is no longer deterministic, and an unaware software developer will quickly become confused.

Given the simplicity of the user interface, it's not surprising that people tend to use it in a naive way. They simply enter a short, general statement as a prompt and expect instant results. But there are no miracles. 

In order for an LLM to work reliably, you must provide context, i.e. all the information necessary to accomplish the task. In the case of coding assistants, this means providing all the source files necessary to generate new code or modify existing code. This includes all declarations, custom data types, classes, functions, etc. If you ignore this, don't be surprised if you get a random or erratic answer. At the beginning of the GenAI revolution, this context could only be built manually. If you used an IDE, you just had to open additional files as tabs in the editor. This approach was never intuitive; it was based on statistical analysis and the assumption that most of the required files had been opened recently and were still open as tabs. 

It turned out that the way you [prompt][prompt-engineering] is even relevant. There are plenty of techniques supported by most models, such as in-context learning and chain-of-thought, that can be used to improve model efficiency. However, there is always a trade-off between prompt specificity and length.

This is the minimum information you need to know when using this technology. If you don't understand how AI works, you will misuse it, produce poor results, and ultimately stop using it completely.


**UX that promised more than it delivered:** The worst thing a provider of AI coding tools can do is mimic functionality that isn't there. As we know, coding assistants were initially unable to build context automatically. Pushing all sources into the context window would overload it. Automatic context building was initially too difficult to accomplish, especially since coding assistants support a wide range of mainstream programming languages — parsing all 'imports' was not a viable option since the logic of 'importing' varies depending on the language. It would simply be too complex and costly to implement such a feature. The tool would lose its 'language agnostic' feature, and being 'language agnostic' is one of the selling points.

One might argue that, if the tool is limited in terms of context building and size, it would be better to make context management more explicit, offering the option to mark and unmark files as being included or excluded. The only way to manage context for code completion was to keep relevant tabs open. However, one of the market-leading tools retains recently closed files in the context. The only way to remove a file was to restart the tool. The worst thing about this is that inexperienced users might conclude that managing tabs — thus building the context — is no longer necessary, so they will skip doing this. I have witnessed this happening.

Another limitation was that early coding assistants were unable to perform complex tasks when using chat functionality, except perhaps for implementing a snake game. Unfortunately, from a user experience point of view, a chat window essentially invited a top-down approach, meaning 'specify the high-level requirements and the tool will change the necessary files'. However, coding assistants without agent mode were unable to do this!


[lost-in-the-middle]: https://cs.stanford.edu/~nfliu/papers/
[prompt-engineering]: https://en.wikipedia.org/wiki/Prompt_engineering
[ai-is-not-faster]: https://blog.miguelgrinberg.com/post/why-generative-ai-coding-tools-and-agents-do-not-work-for-me
[metr]: https://metr.org/blog/
[gh-snake-cases]: https://github.com/topics/snake?l=python
