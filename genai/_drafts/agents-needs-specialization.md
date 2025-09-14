---
title: Agents needs the specialization
layout: post
categories: genai
description: AI Agents can be used for different purposes, do we really need to have multiple agents then?
pic: /genai/img/YoungFrankenstein.jpg
---

[Last time][the-rise-of-vibe-coding] I have demonstrated how do I use AI Agent for not so simple coding tasks (frankly speaking, they were all very complex). My choice was to use GitHub Copilot Agent Mode, but I could also use Windsurf's Cascade, Claude Code and perhaps any reasonable mature Agentic Coding tool that exists. I tried to follow the path sketched out by Andrej Karpathy that he called himself a [Vibe Coding][karpathy]. 

Being sceptical at the beginning, I have employeed a special technique of prompting called "Plan and Execute", that I have learned elsewhere to gain some level of control over what AI Agent is doing. In result, I have vastly improved the Agent's effictency and accuracy over the way it works, just by creating a detailed plan and then letting the Agent precisely execute it. This led me to the question, maybe there are other prompts and other techniques that can be applied to the very same AI Agent letting him doing sligltly or maybe even very different things with equally good quality. If this is true, then perhaps we don't need other agents than the one that we already have? And, in particular, maybe we don't need to *code* our own Agents at all?

### Be careful with instructions file

The Plan and Execute technique requires a rather complex planning prompt to be used in order to analyze the task properly and create a detailed action plan file with exact content. The plan document must be self contained, because we use it then as an attachement to subsequent prompts, asking for execution. Sometimes, we have to restart a new agent conversation, as token window tends to be exchaused, sonner or later.

I tried to be clever, because instead keeping this complex prompt in a notepad just to manually paste and parametrize it when needed, I have crafted my `copilot-instructions.md` file and ordered my AI Agent to apply this way of working each time an user wants to create a new plan. I have even placed a section how to execute plan (and update it accordingly), when user asks for execution. That actually works, at least for my IntelliJ, where, at the time, the instructions file was the only way to customize Copilot.

The only problem with this my particular solution is, that Copilot reads `copilot-instructions.md` file always, the whole content of it. No matter, whether we use "Ask", "Edit" or "Agent" mode, and no matter for which purpose. So, if I want to execute the plan, eventhough the already generated, self contained plan document is attached to the conversation, the plan creation instructions are there. Not only they may confuse the agent, but they are consuming precious token, each time we prompt.

Perhaps instructions file(s) should be used for specific purposes and should provide only the information, that is relevant to the Agent each time it is used? So, let's do what we never do, precisely [RTFM][manual], and here's what we find (as by September, 2025):

> The instructions you add to your custom instruction file(s) should be short, self-contained statements that provide Copilot with relevant information to help it work in this repository. Because the instructions are sent with every chat message, **they should be broadly applicable to most requests you will make in the context of the repository**.

Yes, indeed, they wrote it.

Next, we also find a good recommendation on how `copilot-instructions.md` file should be structured:

> The exact structure you utilize for your instructions file(s) will vary by project and need, but the following guidelines provide a good starting point:
>
> * Provide an overview of the project you're working on, including its purpose, goals, and any relevant background information.
> * Include the folder structure of the repository, including any important directories or files that are relevant to the project.
> * Specify the coding standards and conventions that should be followed, such as naming conventions, formatting rules, and best practices.
> * Include any specific tools, libraries, or frameworks that are used in the project, along with any relevant version numbers or configurations.

There's nothing about prompts, nothing on how to react on a certain asks from an user. It's certainly a wrong place.

### Prompts are different kind of beasts

When prompting, we ask AI Agent to do a certain thing, it is an order, not a suggestion or neutral information. If by any reason you already made any prompt engineering training (if not, I strongly recommend to do it now), you already know, that usually there are some specifics that shall be used when prompting. Well crafted prompt should be properly structured, contain enough context information, precise task definition and usually utilizes a few prompting techniques such as role-playing, few-shots learnings and structured output formatting. This is a lot of information that must be send to the agent and we would not like to do it all the time, but only when needed. We certainy require a sort of information structure and Copilot (and other tools like Windsurf) provide it.

![Copilot architecture](/genai/img/copilot-prompt-architecture.excalidraw.png)
*How you should use GitHub Copilot to build the prompt for the AI Agent.*

Indeed, each time you would like to use an AI Agent to perform a complex task, you should choose a prompt template from your scratch pad / notepad / whatever you use to store texts, fill it with your wish (an user prompt) and then paste it into the agent mode chat window, remembering to select an appropriate language model (see "Rant on Models" section in forementioned [article][the-rise-of-vibe-coding]).

<!-- ### Show that we can keep instructions and prompts separate -->
### A prompt library is an answer

### Conclude on that resulting setup is more scalable and also more accurate when using




[the-rise-of-vibe-coding]: the-rise-of-vibe-coding
[karpathy]: https://x.com/karpathy/status/1886192184808149383?t=7EBkBxJuW-5De2hqxso_jA&s=03
[awesome]: https://github.com/github/awesome-copilot/
[manual]: https://docs.github.com/en/copilot/how-tos/configure-custom-instructions/add-repository-instructions#writing-effective-repository-custom-instructions