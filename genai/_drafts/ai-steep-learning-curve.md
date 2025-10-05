---
title: AI steep learning curve
layout: post
categories: genai
description: Reflections on the journey of learning and adapting to AI technologies.
pic: /genai/img/arnold-commando-ayn-rand-fasces.jpg
---

The generative AI bubble is a real mess, with all those bold statements flying around about 'breakthroughs', 'game-changing' developments, people losing their jobs, rewriting Wikipedia to contain 'their' truth, and the end of humanity. It's even worse in my field of software engineering. AI-powered coding tools are simultaneously "eliminating the need for coding" and "not working at all".

I cannot say that this kind of dissonance is unprecedented, because it isn't. Scepticism and enthusiasm were present with the invention of the steam machine, the bicycle and the car. Nor can I say that I haven't witnessed this myself, because that would be untrue. There was the invention of the personal computer ('people will get stupid'), the internet ('people will get stupid' --- but wait... OK, anyway...) and the mobile phone ('we'll lose our freedom by always being "on the phone"'), and finally the smartphone ('people will get stupid').

Perhaps the only difference with AI in software engineering is that I am personally involved. It's because ["AI can take my job"][when-does-ai], and because it's expected to generate unmaintainable code (because, you know, humans only develop highly maintainable, clean code). Why are there so many mixed opinions? Let's take a brief look at this problem.

### The way we feel about AI

As a big fan of systematic, scientific analysis, I have divided all software engineers with strong opinions on AI into the following four groups:

![Types of AI Users](/genai/img/types-of-ai-users.excalidraw.png)

I will give you some time to recover from the jaw-dropping effect of this eye-opening drawing before continuing with the development of this little theory, which is mostly based on my own observations of the environment in which I work and of trends on different social media. Please note that I will only focus on people who can use AI in their work. I am aware that AI tooling is still prohibited for many projects and companies, mostly due to legal, security and data protection concerns.

Let's start with the enthusiasts. I myself am in a group called 'seasoned enthusiasts'. You could call them seniors or whatever. It's a group of people with significant experience in their profession --- software engineering, in this case. These people clearly recognise the potential of large language models. Some of them may be aware that the main driver in the development of this technology is actually their profession — big tech companies have invested billions just to address the problem of a relatively small pool of well-paid specialists. Seasoned enthusiasts have accepted this reality, either seeing it as an opportunity to quickly upskill in the AI coding area or progressing via a 'can't fight it, so join it' strategy (with exactly the same rationale). The real problem is that 'seasoned enthusiasts' are perhaps the smallest group in the population as a whole.

A much bigger group nowadays is the 'junior enthusiast', who has much less experience but is nonetheless trying to use AI in their daily work. If they are lucky, they have a good AI leader around to help them learn how to use the tools properly. If not, they have to experiment and learn through trial and error, which can be difficult (I will explain why below). However, even if they are only using the chat function (or 'ask mode' in some terminology) to ask questions related to problems with their code or technology, this may still be more efficient than searching for answers on Google or reading through Stack Overflow. Or even worse, watching tons of useless tech videos on YouTube.
Of course, there is a risk that these people will not develop the necessary skills. Even the simplest questions can be answered by AI, so there is no need to search for information, analyse it, synthesise it or apply critical thinking.

Now, let's focus on 'junior sceptics'. Most of them have tried, but after some initial success, they were set back by non-functional software or devastating code reviews performed by fellow engineers. They most likely revert to classical methods, including Stack Overflow, which will most likely become obsolete sooner or later. The existence of this group proves that a competent AI leader is essential for a project, team or company. Unless, of course, some of these people deliberately ignore AI in order to gain practical coding and engineering skills as soon as possible, thereby joining the 'seasoned' group.

Last but not least, my favourite group is 'seasoned sceptics'. I was perhaps there twice, especially when it came to coding assistants. I started using a leading tool from this group, but then quickly reverted to a plain AI chatbot and the copy-and-paste technique because I noticed the earlier one was random. I had the same experience when I first tried a multi-agent tool and then reverted to a more classical approach. In fact, there are two kinds of people in this group: those with very little knowledge of AI, particularly Generative AI, and those with a good awareness of AI who nevertheless try to turn back the clock by neglecting to use any AI in their work, fearing (and with good reason) that they will be eliminated from the market sooner or later. The people in this group can play two roles. One positive role is that of critic and realist. But there is a negative role as well — that of the defeatist, who can spoil the atmosphere and break up your teams.

Now that we have sketched out the different actors in this drama, let's elaborate more on the reasons.

### Overselling is the reason number 1

According to [Wikipedia][gh-wiki], the first version of GitHub Copilot was released as a preview in the form of a Visual Studio Code plugin in late June 2021. Powered by the [OpenAI Codex][openai-codex] model --- a fine-tuned version of GPT-3 --- GitHub Copilot was capable of completing a programmer's thoughts as they typed code into the IDE. I am personally a big fan of AI-assisted code completion. The only problem with this mode of working is the very limited set of capabilities you can use to build context, and it is not as straightforward to phrase your intent --- you either have to name your identifiers very precisely or add additional 'prompts' as code comments.

Early versions of GitHub Copilot went largely unnoticed until [ChatGPT][chatgpt] was released at the end of 2022, using much the same underlying technology as Copilot. With GPT-3.5 and the subsequent GPT-4 capable of generating substantial and intricate pieces of code based on simple English (or even Polish) prompts, the potential of AI-assisted coding has become widely recognised. Coding assistants like GitHub Copilot were immediately revealed and remarketed. The only problem is that, once users of ChatGPT started using Copilot, they were totally puzzled because the only way to use it is still to prompt via 'source code' in the editor window of your IDE. Users noticed that the code completions were more or less randomly accurate and sometimes completely inaccurate. Nobody read the manual to learn that you need to build context. How? By opening the editor's tabs! How obvious...

The problem arose because some people were able to deliver impressive work, such as implementing a video game from scratch. This was then applied to all software engineering tasks, including development on existing code bases, some of which were legacy systems with significant technical debt. 

Tool providers started to claim that their tools could significantly speed up development, and the numbers were astonishing. In early 2023, a [paper][Arxiv-GH-CoP] was published claiming evidence that using an AI coding assistant speeds up task completion by an incredible 55.8%. The evaluators received a basic introduction to how to properly use the tool (GitHub Copilot). They were tasked with developing a HTTP server in JavaScript. From scratch, of course.

![You can do big things with little effort](/genai/img/arnold-commando-ayn-rand-fasces.jpg)
*'You can do big things with little effort' --- they said (the log is fake).*

Here's why the marketing behaviour of the leading AI tool providers has become problematic. It's because the 'evidence' includes the most typical and common programming tasks, which are represented in open-source repositories with tons of examples. It doesn't matter if it's a snake game in Python, a Tetris game in JavaScript, or an HTTP server implemented in any mainstream programming language. The aforementioned study describes the problem of writing an HTTP server from scratch, which is the easiest case for AI because you don't have to deal with an existing codebase to establish the context for a conversation. We usually work with an existing codebase rather than a new one, in about 95% of cases, if not more.

A simple search on GitHub revealed 643 public JavaScript repositories implementing an HTTP server (tagged with http-server) and 467 public Python repositories implementing a snake game. This is perhaps enough for Large Language Models to acquire complete knowledge during their training processes, which is why they can implement such typical cases flawlessly. However, this does not work for your project because, sadly, you are not tasked with implementing yet another snake game. The study contains a serious methodological error.

### Complexity of the technology is the reason number 2

<!-- Context building, prompt engineering -->
<!-- not corrected -->

![You'are using that wrong](/genai/img/commando-rocket-launcher-instruction-manual.jpg)
*You're clearly using that wrong!*

### Poor usability of the tooling is the reason number 3

<!-- Messing up with tabs, fake, untransparent access to output and recently closed tabs -->

### But tools are evolving in right direction

<!-- Agentic AI -->

### Summary

<!-- Recommendations per group -->

### Disclaimer

All photos used in this article come from the brilliant 'Kommando', a VHS era film which celebrates its 40th anniversary today.

[when-does-ai]: when-does-ai-take-my-job
[ai-is-not-faster]: https://blog.miguelgrinberg.com/post/why-generative-ai-coding-tools-and-agents-do-not-work-for-me
[metr]: https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/
[gh-wiki]: https://en.wikipedia.org/wiki/GitHub_Copilot
[openai-codex]: https://en.wikipedia.org/wiki/OpenAI_Codex
[chatgpt]: https://en.wikipedia.org/wiki/ChatGPT
[arXiv-GH-CoP]: https://arxiv.org/pdf/2302.06590
[gh-http-cases]: https://github.com/topics/http-server?l=javascript
[gh-snake-cases]: https://github.com/topics/snake?l=python
