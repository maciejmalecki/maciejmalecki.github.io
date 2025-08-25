---
title: Design by Agent
layout: post
categories: genai arch
pic: /genai/img/koszmary-architektury.jpg
---
In my previous, Vibe coding related article, [The Rise of Vibe Coding][the-rise], I have prototyped an experiment to show whether such technique can effectively replace classical coding. At the same time, I have expressed fear, that the term, Vibe coding, will end up there, where all earlier misunderstood or misused IT concepts ended, like EJB, SOA, Agile, to mention just a few.

Let me just cite Andrey Karpathy:

> There's a new kind of coding I call "vibe coding", where you fully give in to the vibes, embrace exponentials, and **forget that the code even exists**. It's possible because the LLMs (e.g. Cursor Composer w Sonnet) are getting too good. Also I just talk to Composer with SuperWhisper so I barely even touch the keyboard. I ask for the dumbest things like "decrease the padding on the sidebar by half" because I'm too lazy to find it. **I "Accept All" always**, **I don't read the diffs anymore**. When I get error messages I just copy paste them in with no comment, usually that fixes it. The code grows **beyond my usual comprehension**, I'd have to really read through it for a while. Sometimes the LLMs can't fix a bug so I just work around it or ask for random changes until it goes away. It's not too bad for throwaway weekend projects, but still quite amusing. I'm building a project or webapp, but it's not really coding - I just see stuff, say stuff, run stuff, and copy paste stuff, and it mostly works.
> *Source: [Andrei Karpathy on X][karpathy]*

It is too good, I've found this particular X entry now, while writing this article, because I must admit, I do exactly the same, since I started using AI Agent for coding. I also accept all changes always, I blindly click "run" on tools (because GitHub Copilot always asks for confirmation, nobody knows why), i type "contninue" when Agent reaches internal limits. So I didn't know that Andrei wrote exactly this and I just started to use this approach anyway, because, honestly, there is no other choice. The reason why is very simple: if you do otherwise, meaning, review every single line of generated code, check everything with libraries API documentation and so on, you will end up on being as slow as before, except paying extra fee for a tool and tokens. It is an either-or kind of game.

The problem is that you can still generate a junk that pretends to work and cannot be relied on and this is definitely not something, that client pays for. So for mid to large size custom applications, there must be a way to use advantages of vibe and still sail your boat in the right direction.

I am more than certain, that the enterprise story of vibe coding will be following: first there will be an era of naive vibe coding that will generate a lot of legacy (a new term: green field legacy, remember that!). Then there will be an era of "controlled vibe coding", that is, for each `n` of young vibe coders there will be a `m` of senior code reviewers, checking twice each line of code being commited. That will be the end of the story - let's try to avoid that.

(But seriously, once I have worked in one company, which rolled out a "controlled agile", I am not kidding!)

Let's summarize this short introductionary rant with a cite from Martin Fowler:

> I'm convinced that this is another fundamental change: talking to the machine in prompts is as different to Ruby as Fortran to assembler. But this is more than a huge jump in abstraction. When I wrote a Fortran function, I could compile it a hundred times, and the result still manifested the exact same bugs. Large Language Models introduce a non-deterministic abstraction, so I can't just store my prompts in git and know that I'll get the same behavior each time. As my colleague Birgitta put it, we're not just moving up the abstraction levels, we're moving sideways into non-determinism at the same time.
> *Source: [Martin Fowler][fowler]*

Yes, don't even dare to store your prompts in GIT instead of the code. The code is there and wishful thinking will not change it.

## Resurrection of Design

There was a time when we had a designers in IT project. That is, design closed the gap between architecture and software developers. Solution was passed over by analysts into the architects to check, that it fits into the boundaries of the architecture and then designers took the job to model data structures and compose the code into the business components, classes, perhaps methods even. Developers, mostly junior employees got a design checklist and just filled in source files with content. That was before Agile came. And no, that wasn't just bound to the "waterfall" project. Trust me, I work for more than 20 years and *never* saw a true waterfall project in my life. Before agile we usually did an iterative-incremental development. And yes, iterative is not a synonym of Agile!

Software designer as a separate role has slowly died and its remnant can be found partially in software architect role and partially in development. I am not missing that role if architect and developers do their job properly. If so, then just preparing a separate design document is a waste of time, especially that it is a volatile document that can be used only during given iteration and gets outdated quickly afterwards (nobody is willing to update it in subsequent iterations and I fully understand this). So, usually architect is the one in charge of writing "technical consideration" or "solution details" in story description or just writes a solution design for bigger features.

I can imagine doing a formal design if only it is cheap to prepare. And guess what? Now we have an AI agent that will do it for me.

## A good design also requires a plan

[Last time][the-rise] I have presented a prompting strategy called "plan and execute". Apparently this strategy, and underlying system prompt in particular, is a general purpose tool. It can be used for implementing new features, changing existing functionality, troubleshooting, bugfixing and, in particular, planning.

I will continue with my already presented hobby project, [RBT][rbt], that is a Gradle plugin that can build assembly projects for Commodore 64 and other 8-bit targets.

[the-rise]: the-rise-of-vibe-coding
[fowler]: https://martinfowler.com/articles/2025-nature-abstraction.html
[karpathy]: https://x.com/karpathy/status/1886192184808149383?t=7EBkBxJuW-5De2hqxso_jA&s=03
[rbt]: https://c64lib.github.io/gradle-retro-assembler-plugin/

