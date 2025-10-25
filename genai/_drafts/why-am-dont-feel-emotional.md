---
title: "Why don't I feel emotional about the code I used to write?"
layout: post
categories: genai architecture
description: With Agentic AI, we probably have a ready-to-use and intuitive product. This is a subjective story about why early AI coding tools failed to be adopted, and how we can recover.
pic: /genai/img/floppy.jpg
---

A few days ago my colleague Adam Witkowski published an [interesting article][adam] titled 'Digital orphans'. Adam compares two approaches of authoring code: 'classic' one, where code is crafted more or less by hand (human hand, to be precise), and 'vibe coding', where human just instructs AI Agent to generate code via natural language prompts and usually does not even look 'inside'. Adam used beautiful metaphor of parent-child relationship applicable to the relation between programmer and his code. A relation that certainly is broken if we 'instruct' AI to write and deploy something, that works. AI generated applications became 'digital orphans' meaning nobody feels really responsible. This leads to a lot of nasty things such us security issues or legacy code being created at 'day one'.

When I thought about it, I said to myself 'Well, that's certainly true!'. But then, almost instantly, I though 'Wait a minute! This is nowhere close to my experiences and the way I feel about AI!'.

### The Vibe Coding myth

In one of my previous, Vibe coding related article, [The Rise of Vibe Coding][the-rise], I have prototyped an experiment to show whether such technique can effectively replace classical coding. At the same time, I have expressed fear, that the term, Vibe coding, will end up there, where all earlier misunderstood or misused IT concepts ended, like EJB, SOA, Agile, to mention just a few.

Let me just cite Andrej Karpathy:

> There's a new kind of coding I call "vibe coding", where you fully give in to the vibes, embrace exponentials, and **forget that the code even exists**. It's possible because the LLMs (e.g. Cursor Composer w Sonnet) are getting too good. Also I just talk to Composer with SuperWhisper so I barely even touch the keyboard. I ask for the dumbest things like 'decrease the padding on the sidebar by half' because I'm too lazy to find it. **I "Accept All" always**, **I don't read the diffs anymore**. When I get error messages I just copy paste them in with no comment, usually that fixes it. The code grows **beyond my usual comprehension**, I'd have to really read through it for a while. Sometimes the LLMs can't fix a bug so I just work around it or ask for random changes until it goes away. It's not too bad for throwaway weekend projects, but still quite amusing. I'm building a project or webapp, but it's not really coding - I just see stuff, say stuff, run stuff, and copy paste stuff, and it mostly works.
>
> *Source: [Andrej Karpathy on X][karpathy]*

I must admit, I do exactly the same, since I started using AI Agent for coding. I also accept all changes always, I blindly click 'run' on tools (because GitHub Copilot always asks for confirmation, nobody knows why), i type 'continue' when Agent reaches internal limits. So I didn't know that Andrej wrote exactly this and I just started to use this approach anyway, because, honestly, there is no other choice. The reason why is very simple: if you do otherwise, meaning, review every single line of generated code, check everything with libraries API documentation and so on, you will end up on being as slow as before, except paying extra fee for a tool and tokens. It is an either-or kind of game.

The problem is that you can still generate a junk that pretends to work and cannot be relied on and this is definitely not something, that client pays for. So for mid to large size custom applications, there must be a way to use advantages of vibe and still sail your boat in the right direction.

I am more than certain, that the enterprise story of vibe coding will be following: first there will be an era of naive vibe coding that will generate a lot of legacy (a new term: green field legacy, remember that!). Then there will be an era of "controlled vibe coding", that is, for each `n` of young vibe coders there will be a `m` of senior code reviewers, checking twice each line of code being commited. That will be the end of the story - let's try to avoid that. 

(But seriously, once I have worked in one company, which rolled out a 'controlled agile', I am not kidding!)

There is one problem with this term: 'vibe coding'. It has been most likely misunderstood. Andrej didn't write 'you don't need to be a software engineer to vibe code'. Or 'anybody can now write code'. It was just concluded from the words 'forget that the code even exists'. The reality is however, that I assume Andrej Karpatky is well aware that the code exists, anyway. He always imagines how it may look like, because he is a very experienced programmers. 

I suspect this, because this is how I feel about my experience with very technique. For me it is enough to see the names (yes, only the names) of the files that has been generated or modified. It's just single look and I know that most likely this is what I was expecting therefore I decide not to look at it. And I accept. I have understood, that this is not 'blind' action. I know what I am doing. And even if there will be a problem, I will be able to spot it and fix it (most likely prompting AI again, as I am also getting lazy). This is an intuition derived from my skills and experience. Nobody with literaly no knowledge about programming would be able to do that.

### AI generated code does not need to be 'random'

If you use tools such us Cursor, Windsurf, or GitHub Copilot you have a plenty of options to use to customize the way AI Agent behaves and generates code. Honestly, this is not an option, this is a must. In Copilot, you have to provide at least `copilot-instructions.md` file to contain necessary information about the project, it's architecture, it's composition, what are the tools available, how and when to use them. You can specify all of it, in natural language. It's even in [manual][instructions].

While am I currently pitching Agentic SE topics quite frequently, the question that always is asked is 'can we trust the quality of generated code'. My answer is always 'you don't let Agent do whatever it wants to do, you have to narrow the corridor' and then I show this picture:

![Narrow the corridor](/genai/img/narrow-the-corridor.excalidraw.png)
*Narrow the corridor*

It speaks by itself. Always.

So, you still need to know how to shape and integrate these tools. You have to maintain this orchestation. You have to ensure that your CI is strict. You have to ensure, that everything is automated. Even testing.

There is no way somebody without programming and engineering background can do this.

### I am not father of my code

My last point in this rant is actually quite sad. I have discovered, that I don't feel anything about the code I write for living. Well, maybe 2 or 3 times I was exceptionally proud of something I have hand-coded at the very low level. In my whole career. This is not what I thought in 1990, when I decided to become a programmer. Because at that time, for the next 10 years I wrote code for fun. That was mostly a video games or graphical programs (yes, I loved to create UIs).

But then I decided to become a web developer, that is, fullstack guy, originally coding in Java, then also included JavaScript frameworks to liberate myself from JSP/PHP/JSF hell. The code I write for so many years are business apps, usually a mixture of CRUDs and some more deeper modules, and UI is usually tables and forms. Perhaps charts, that's it. Writing this kind of code is not fun at all. I feel no emotion about it. The only linkage is visible via `git blame`.

The only parts that I love are on these disks:

![Floppies](/genai/img/floppy.jpg)

The first one contains a [BASIC][basic] program that I wrote in 1993 (the only that survived). It does nothing serious, but I invented it and typed it myself that year. I would show it, but I don't have a time to set up my [1541][1541] to read this disk (I know it works). The second one is a first physical release of my video game ever, a single filer on the Side B of this disk, named [TRex64][trex]. This is what I write for fun, taking that much time I need. Coding this in bare assembly. I am father of this code.

I don't know anybody who loves coding endless CRUDs, tables and forms. I don't think that anybody cares about such a code (even hand written) more, than it is enforced by CI/CD pipelines. The tools. Exactly, the same tools, that AI must use, obey and tackle feedback, correcting itself.

AI Agent can generate insecure code? The same for humans...

AI Agent can generate legacy from scratch? O boy... Existing legacy has been created by 100% humans! And in my whole career I've met at least a few programmers, that were writting pure legacy from the day one. Nobody's perfect.

### I am still father of my architecture

I said I have no fun from coding any longer. That's not 100% true. I don't feel low level coding makes sense. But at least 10 years ago I have discovered something more exciting. Application have architectures. Applications can be modularized. We can manage this, dependencies between modules, granularities of the modules. Architecture existing on micro and on macro levels. We have big interconnected systems of applications. We have microservices. We can shape the architecture so that the software stays maintainable long term. You know what? Architecture is [still relevant](significance-of-architecture), even more when using Agentic AI for coding. 

Can Agents create and manage architectures? Sure they can. But perhaps specifying the prompt to do that will reveal the architecture itself, so perhaps AI can just confirm our choices. We will still do most of the job alone. I am bit more optimistic on this.

[adam]: https://www.linkedin.com/pulse/digital-orphans-adam-witkowski-r17ef/?trackingId=4OfhmobARbyeKlfo9Y6qQQ%3D%3D
[the-rise]: the-rise-of-vibe-coding
[karpathy]: https://x.com/karpathy/status/1886192184808149383?t=7EBkBxJuW-5De2hqxso_jA&s=03
[instructions]: https://docs.github.com/en/enterprise-cloud@latest/copilot/concepts/prompting/response-customization
[basic]: https://en.wikipedia.org/wiki/Microsoft_BASIC
[1541]: https://en.wikipedia.org/wiki/Commodore_1541
[trex]: https://maciejmalecki.itch.io/trex64
