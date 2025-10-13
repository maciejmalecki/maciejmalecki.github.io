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

**Technical limitations** of early coding assistants were significant. Early Large Language Models were very limited when it comes to their context window size. This of course made working with them challenging: even a medium sized project means a lot of context data: domain description, auxiliary documentation and most prominently the code base. 

Makers of such tool didn't even attempted to fool us, but just declared: we cannot consume all your context, so we give you a freedom (and necessity) to build one each time you need to perform a task. 

Building up a context with opened tabs, and later -- once Chat function was introduced to the coding assistants -- as an attachement to the conversation was a tedious and error prone work. And even this wasn't really used properly, if at all. Shall we blame developers? I don't think it is fair -- it is an unusual and surprising way to use their favourite IDE.

Before a thinking models were populatized, there was another limitation, perhaps even more annoying. Tools had very limited planning capabilities, if any. In reality, all planning and more complex design work was intended to be -- again -- done by humans. This means that with exception of very special cases, such as implementing a simple but common video game, it wasn't really possible to prompt coding assistant with high-level requirement description and expecting a working result (even if context were properly built).

Of course, for typical, professional work, a planning and design can easily be the most difficult, and sometimes the most time consuming part of the developer's job. While coding assistants can somehow assist here, they were unable to join this kind of work with exact implementation tasks -- surely not on the single run.

**Expectation misalignment** was hugely introduced by enthusiasts, usually when somebody was lucky in implementing tasks that were simple enough for AI at the time.


> **Snake Game phenonemon**
>
> There is a plenty of working examples that people were able to implement a snake game from scratch, just by using ChatGPT. This made an impression, that we're very close to the moment, where there is literally no need to have any programming skills and still be able to generate working software. A few examples can be watched on YouTube.
>
> Driven by this example many tried to replicate the same approach on their project and failed. A little light on that can be shed with quick peek into the one of the biggest open-source repository collection: GitHub. A simple search reveals that there is over [450 public Python source code repositories][gh-snake-cases] available with some form of snake game. So maybe this (plus many more in other places) was a portion of huge training data set used to train language models used by chatbots and coding assistants?


**Complexity without transparency:** Artificial intelligence is indeed complex. It's a subject that most of us don't even attempt to familiarise ourselves with. The problem is that, from the end user's point of view, the technology seems trivial. Most interaction is with a chat interface, or perhaps an AI coding assistant that also offers a chat interface. Code completion is a well-known feature of IDEs, so nobody feels the need to understand what's going on behind the scenes. However, this time, code completion is no longer deterministic, and an unaware software developer will quickly become confused.

Given the simplicity of the user interface, it's not surprising that people tend to use it in a naive way. They simply enter a short, general statement as a prompt and expect instant results. But there are no miracles. 

In order for an LLM to work reliably, you must provide context, i.e. all the information necessary to accomplish the task. In the case of coding assistants, this means providing all the source files necessary to generate new code or modify existing code. This includes all declarations, custom data types, classes, functions, etc. If you ignore this, don't be surprised if you get a random or erratic answer. At the beginning of the GenAI revolution, this context could only be built manually. If you used an IDE, you just had to open additional files as tabs in the editor. This approach was never intuitive; it was based on statistical analysis and the assumption that most of the required files had been opened recently and were still open as tabs. 

But why can't all source files be provided as context automatically? It's simple: initially, it was simply too much because each LLM is characterised by something like a context window size. This applies to everything: context data, conversation history (when using the chat function), and of course the prompt itself. Even with LLMs accompanied by large context window sizes, processing everything does not work because recent models suffer from the [lost in the middle][lost-in-the-middle] phenomenon.

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
