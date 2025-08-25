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

I will continue with my already presented hobby project, [RBT][rbt], that is a Gradle plugin that can build assembly projects for Commodore 64 and other 8-bit targets. The project is useful by itself, but I also use it for experiments. One is to use Kotlin as a main programming language, second is to use Gradle as build system (instead of Maven, that I never liked), third is to use so-called Hexagonal Architecture. I use `copilot-instructions.md` file to put some essential information about the project and it's architecture, remembering to keep these informations short and concise. Let me reveal these chapters, as I kept them empty [last time][the-rise].

> ### Coding guidelines
> 1. This application is a Gradle plugin for building Assembly projects for MOS 65xx family of microprocessors, using Kick Assembler as the only supported ASM dialect.
> 2. This application is implemented in Kotlin and uses Gradle as the build tool.
> 3. This application uses Hexagonal Architecture, with the domain layer containing the business logic and the adapters layer containing the glue code between the domain layer and specific technology, such as Gradle.
> 4. Top level directories denote bounded context, each having internal hexagonal structure.
> 5. There are dedicated at-hoc gradle plugins declared for each kind of module, all being located in the `buildSrc` folder.
> 6. Each module should have its own `build.gradle.kts` file, with the root `build.gradle.kts` file aggregating all modules and applying the necessary plugins.
> 7. There is an end user documentation stored in `doc` folder that is implemented in AsciiDoctor, keep it up to date with the code changes.
> 8. There is `CHANGES.adoc` file in the root of the project that contains the change log for the project, keep it up to date with the code changes.
>
> ### Testing guidelines
> 1. This application uses Kotest as testing library for unit and integration tests.
> 2. This application prefers using BDD style of testing, using Given/When/Then DSL of Kotest.
>
> *(for more check my GIT [repository][instructions])*

So, as you can see, I put all essential information related to the project, including the purpose, main technology stack, file layout, where to find documentation and so on. I used some important key words such as `bounded context`, `business logic`, `adapters`. I also use a very specific library to write unit tests (Kotest), so I named it here as well as which style of testing do I prefer (BDD).

Having all this in my repository, plus a "plan and execute" prompt as defined in [The Rise of Vibe Coding][the-rise], I open my Agent tab, select right model (that's one of Claude Sonnet family) and type my first prompt, ordering preparation of a plan:

> Currently, all tasks in the problem are executed sequentially, not leveraging parallelization feature of Gradle. This results in very long build times for complex projects that execute compilation, preprocessing and postprocessing of long projects. Propose a new bounded context named 'flows' that will allow to organize tasks into chains (flows) that can depend on each other (outputs of one flow can feed input of another flows). This new flow mechanism should have a separate, new DSL syntax. Create an action plan.

Honestly speaking, I was shaping this feature for a very long time. Not because it is a very difficult one, but rather that I really had no idea how to model it. I would need to do some prototyping to see what kind of DSL (Domain Specific Language, that is the main notation that Gradle uses as a "language" for its `build.gradle` files) works best for my purpose. I could sit with a pencil and sheet of paper and try to sketch it, but that is something "non coding"-related, that I don't like to spend my free time on.

I have imagined that perhaps I can arrange "steps" in something I called "pipelines", later renamed to flows. I thought that maybe flows can depend on other flows and possibly run in parallel if feasible. I had no idea, however, how to link steps together. In "classic" RBT notation such linking is very limited and built-in into the existing DSL, but this will not work here. I can have a flow that will convert graphical data into binary asset, then pack it, import and compile using assembler and then another flow that will gather these compiled programs and link it together into the floppy disk image using again the assembler. Each step can produce multiple files and consume also multiple files, this is not that easy.

So I made this prompt (above) and was curious whether AI Agent can propose something for me. After a short while an [action plan][action-plan] was generated and this is what I found there.

First, Agent has rewritten my "user story" into more elaborated form as "Issue Description".

> ## Issue Description
> Currently, all tasks in the Retro Assembler Plugin are executed sequentially, not leveraging Gradle's parallelization features. This results in very long build times for complex projects that execute compilation, preprocessing, and postprocessing. The existing `flows` bounded context exists but lacks implementation. We need to enhance it with a new DSL syntax that allows organizing tasks into chains (flows) that can depend on each other, where outputs of one flow can feed inputs of another flows, enabling parallel execution.


All that is essential is included there: what? (organize build into flows), the reason (performance), how? (run independent tasks parallely) and most importantly "that can depend on each other", meaning - not everything can run in parallel.

Then, Agent went straight into the design, first identifying relevant code parts (as instructed!):

> ## Relevant Codebase Parts
> 1. **flows/** - Empty bounded context directory that needs to be populated with domain logic and adapters
> 2. **infra/gradle/src/main/kotlin/com/github/c64lib/gradle/RetroAssemblerPlugin.kt** - Main plugin where tasks are currently registered sequentially
> 3. **shared/gradle/** - Contains existing DSL extensions and task utilities that need to be enhanced
> 4. **compilers/kickass/** - Kick Assembler integration that represents the main compilation flow
> 5. **processors/** - Various processor modules (charpad, goattracker, image, spritepad) that can be parallelized
> 6. **dependencies/** - Dependency resolution that can run in parallel with other tasks
> 7. **emulators/vice/** - Testing execution that depends on compilation outputs
> 8. **buildSrc/** - Contains Gradle plugin definitions that need enhancement for flow support

Yes, I did one thing manually before using AI Agent. I have created an empty bounded context `flows`, and forgot to remove it. So, Agent found it and enlisted as point 1. Point 2 is essential, this is the main plugin class and surely it must be modified to register new DSL constructs (flows). Then, Agent has found essential functionalities that would need to be plugged into the flows, that is 4 and 5. Very good, I am impressed.

Then, following instructions, Agent formulates a root cause hypothesis:

> ## Root Cause Hypothesis
> The current architecture treats each task as an independent Gradle task with simple dependencies, but doesn't leverage Gradle's built-in parallelization capabilities. The missing piece is:
> 1. **Flow Definition DSL** - A way to define task chains with explicit input/output relationships
> 2. **Dependency Graph Analysis** - Logic to analyze which flows can run in parallel
> 3. **Task Orchestration** - Enhanced task registration that respects parallel execution capabilities
> 4. **Resource Management** - Proper handling of shared resources and output artifacts

That was a creative part of the Agent.

> ## Investigation Questions
> 1. What are the current bottlenecks in build execution time?
> 2. Which tasks are independent and can run in parallel (e.g., different processors)?
> 3. What are the input/output dependencies between different task types?
> 4. How should the new DSL syntax look to be intuitive for users?
> 5. What Gradle features should we leverage for parallel execution?
> 6. How do we maintain backward compatibility with existing build scripts?
> 7. What validation is needed to prevent circular dependencies in flows?
> 8. How do we handle error propagation in parallel flows?
> 9. What monitoring/logging is needed for parallel execution debugging?
> 10. How do we test the parallel execution scenarios effectively?


[the-rise]: the-rise-of-vibe-coding
[fowler]: https://martinfowler.com/articles/2025-nature-abstraction.html
[karpathy]: https://x.com/karpathy/status/1886192184808149383?t=7EBkBxJuW-5De2hqxso_jA&s=03
[rbt]: https://c64lib.github.io/gradle-retro-assembler-plugin/
[instructions]: https://github.com/c64lib/gradle-retro-assembler-plugin/blob/develop/.github/copilot-instructions.md
[action-plan]: https://github.com/c64lib/gradle-retro-assembler-plugin/blob/develop/.ai/62-pipelines/feature-flows-parallelization-action-plan.md
