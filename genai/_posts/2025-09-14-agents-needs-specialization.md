---
title: AI agents require specialisation
layout: post
categories: genai
description: AI agents can be used for a variety of purposes. Do we really need multiple agents?
pic: /genai/img/kramer.jpg
---

[Last time] [the-rise-of-vibe-coding], I demonstrated how to use an AI agent for complex coding tasks. I opted for GitHub Copilot Agent Mode, but I could also have used Windsurf's Cascade, Claude Code, or any other mature Agentic Coding tool. I tried to follow the approach outlined by Andrej Karpathy, who refers to it as [Vibe Coding][karpathy]. 

Initially sceptical, I employed a special prompting technique called 'Plan and Execute', which I had learned elsewhere, to gain some level of control over what the AI agent was doing. As a result, I vastly improved the agent's efficiency and accuracy by creating a detailed plan and letting the agent execute it precisely. This led me to question whether there are other prompts and techniques that can be applied to the same AI agent to make it perform slightly or even very different tasks with equally good quality. If so, perhaps we don't need any other agents besides the one we already have. In particular, perhaps we don't need to code our own agents at all.

### Be careful with instructions file

The Plan and Execute technique requires a complex planning prompt to analyse the task properly and create a detailed action plan. The plan document must be self-contained because it is used as an attachment to subsequent prompts asking for execution. Sometimes, we have to restart a new agent conversation as the token window tends to expire sooner or later.

Instead of keeping this complex prompt in a notepad and manually pasting and parametrising it when needed, I crafted my `copilot-instructions.md` file and ordered my AI agent to work this way each time a user wants to create a new plan. I have even included a section on how to execute the plan (and update it accordingly) when the user requests execution. This actually works, at least in IntelliJ, where the instructions file was the only way to customise Copilot at the time.

The only problem with my particular solution is that Copilot always reads the entire content of the `copilot-instructions.md` file. This happens no matter whether we use "Ask", "Edit" or "Agent" mode and no matter what the purpose is. So, even though the self-contained plan document is already generated and attached to the conversation, the plan creation instructions are still there. Not only may they confuse the agent, but they also consume precious tokens each time we prompt.

![Cosmo Kramer](/genai/img/kramer.jpg)
*Agents tends to panic while overloaded with instructions. Cosmo Kramer.*

Perhaps the instructions should be specific to each purpose and only provide the relevant information each time the agent is used? So, let's do what we never do and read [the manual][manual]. Here's what we find (as of September 2025):

> The instructions you add to your custom instruction file(s) should be short, self-contained statements that provide Copilot with relevant information to help it work in this repository. Because the instructions are sent with every chat message, **they should be broadly applicable to most requests you will make in the context of the repository**.

Yes, they did write it.

Next, we find a useful recommendation for the structure of the `copilot-instructions.md` file.

> The exact structure you utilize for your instructions file(s) will vary by project and need, but the following guidelines provide a good starting point:
>
> * Provide an overview of the project you're working on, including its purpose, goals, and any relevant background information.
> * Include the folder structure of the repository, including any important directories or files that are relevant to the project.
> * Specify the coding standards and conventions that should be followed, such as naming conventions, formatting rules, and best practices.
> * Include any specific tools, libraries, or frameworks that are used in the project, along with any relevant version numbers or configurations.

There's nothing about prompts or how to react to certain requests from users. This is certainly the wrong place.

### Prompts are a different kind of beast.

When prompting, we are asking the AI agent to perform a specific task; it is an instruction, not a suggestion or neutral information. If you have already undertaken any prompt engineering training, you will be aware that there are usually some specifics that should be used when prompting. A well-crafted prompt should be properly structured and contain enough contextual information and a precise task definition, as well as utilising a few prompting techniques, such as role-playing, few-shot learning and structured output formatting. This is a lot of information to send to the agent, and we would prefer not to do it all the time, only when needed. Certainly, we require a certain type of information structure, and Copilot (and other tools such as Windsurf) provide this.

![Copilot architecture](/genai/img/copilot-prompt-architecture.excalidraw.png)
*How you should use GitHub Copilot to build the prompt for the AI Agent.*

Indeed, whenever you wish to use an AI agent to perform a complex task, you should select a prompt template from your scratchpad, notepad or whatever you use to store text. Fill it with your desired prompt and then paste it into the agent mode chat window. Remember to select an appropriate language model (see the 'Rant on Models' section in the [aforementioned article][the-rise-of-vibe-coding]).

### A prompt library provides the answer.

GitHub Copilot supports so-called prompt files. This is a relatively new feature, introduced in September 2025, but it has been available in Visual Studio Code for some time. I had an epiphany when a colleague shared a [link][awesome] containing a Git repository with lots of custom instructions and prompt files. So why shouldn't I organise my repository in exactly the same way? Meanwhile, I discovered that prompt files had been added to the IntelliJ plugin without my noticing (perhaps I didn't read the release notes).

I have decided to reorganise my '.github' folder slightly. I have removed all "Plan and Execute" related items from my `copilot-instructions.md`, and created two separate prompt files:

* [plan.prompt.md][plan.prompt] contains now all information needed to create a new plan.
* [exec.prompt.md][exec.prompt] contains now all information needed to execute plan.

Now let's take a look at the second one.

```markdown
---
mode: 'agent'
description: 'Execute step or multiple steps from an attached action plan md file.'
tools: ['read_file', 'edit_file', 'codebase', 'search', 'searchResults', 'changes', 'editFiles', 'run_in_terminal', 'runCommands', 'runTests', 'findTestFiles', 'testFailure', 'git']
---
You are an experienced software developer tasked with executing an attached action plan md file to implemented an issue.

You should focus only on executing following steps, identified by their numbers, as specified in the action plan:

<steps_to_execute>

${input:steps}

</steps_to_execute>


Follow the following rules during plan execution.

1. When developer asks for executing plan step, it is always meant to be a step from the *next steps* section of the action plan.
2. When developer asks for complete plan execution, execute the plan step by step but stop and ask for confirmation before executing each step
3. When developer asks for single step execution, execute only that step
4. When developer asks additionally for some changes, update existing plan with the changes being made
5. Once finishing executing of the step, always mark the step as completed in the action plan by adding a ✅ right before step name.
6. Once finishing executing the whole phase, always mark the phase as completed in the action plan by adding a ✅ right before phase name.
7. If by any reason the step is skipped, it should be marked as skipped in the action plan by adding a ⏭️ right before step name. It should be clearly stated why it was skipped.
```

When this file is stored in the `.github/prompts` folder of your repository and its suffix conforms to `*.prompt.md`, it is automatically made available in your chat window when you type the '/' character.

![launch prompt](/genai/img/launch-prompt.png)

From my observations, anything you type after the prompt template name is automatically passed to the prompt template and whatever you place a `${input:value}` placeholder is pasted. According to the [documentation][prompts-vsc], you can specify multiple placeholders and pass multiple distinct values using key/value notation, for example, `/plan: task=..., issue=1234, name=implement abc`, but this does not seem to work properly with IntelliJ yet.

However, passing a single value is sufficient for now, so we can carefully pass steps to the 'exec' prompt and place them in the correct location in our prompt template.

As you can see, with the prompt template, we can leverage a wide range of different prompting techniques. This allows us to properly define a role ('You are an experienced software developer...') and concrete rules that should be obeyed by the agent. There is also a special header that uses a syntax called 'front matter', where you can specify a set of metadata that characterises this prompt. You can limit the prompt to a specific Copilot mode (here, we only use the "agent" mode), limit the set of tools the agent can use to execute this prompt and specify the language model to be used with this prompt. If unspecified, the agent will always use the model selected by the user.

Using this newly discovered capability, I quickly developed another prompt template, [plan-update][plan-update.prompt], which I use to supplement an existing plan with additional information. I also use this prompt template when I want to answer the questions that the AI agent asked in the original plan. Let's take a look:

```markdown
---
mode: 'agent'
description: 'Update action plan with new issue details, marks resolved questions and updates action plan steps as needed.'
tools: ['read_file', 'edit_file', 'codebase', 'search', 'searchResults', 'changes', 'findTestFiles', 'git']
---
You are an experienced software developer tasked with creating an action plan to address an issue. Your plan is already generated and attached to this conversation, but you want to enhance it with additional information that was missing once plan was first created. Your goal is to produce a comprehensive, step-by-step plan that will guide the resolution of this issue.

First, review the following information:

<issue_update>

${input:update}

</issue_update>

With information provided above, perform the following steps:

1. Incorporate information provided as issue_update to the action plan; crosscheck the plan to see if it requires an update having in mind issue_update information.
2. Check if any of the questions for others hasn't been answered by acceptance criteria and mark it accordingly if so.
3. If any new questions for others arise from the issue_update information, add them to the list of questions for others.
4. If at any point you see that relevant code parts section needs to be updated, update it accordingly.
```

Now we can really see the power of the prompt library and the ability to put these prompts into separate files. I doubt role-playing and other techniques would ever work if all these behaviours were coded directly into the copilot instructions file. I would expect chaos and completely erratic behaviour from an agent orchestrated in this way.

### Summary

We can already see the clear direction in which all AI-aided coding tools are heading. Thinking processes (also known as agentic modes) and automatic context building are essential. However, it is almost equally important to be able to orchestrate an AI agent so that it fits perfectly into the project and codebase it is working on. There are two cornerstones to this orchestration: project knowledgebases (such as Copilot instructions) and a prompt library. The latter defines a set of predefined behaviours that the agent can follow to achieve specific outcomes.

With a well-crafted set of prompt templates, we can effectively get a set of specialised agents that serve different purposes. My three prompts give me a specialised set of agents: a tech lead agent that performs software design and feature breakdowns using the ['plan'][plan.prompt] and ['update-plan'][plan-update.prompt] prompts, and a software engineer that implements according to the plan using the ['exec'][exec.prompt] prompt. The list of roles that can be orchestrated in this way is open.

The beauty of the design is that all of these files are Markdown text documents that are easy to write and have a relatively flexible structure. This means that each tech lead can now customise the AI agent to maximise benefits. Most importantly, these files can easily be shared amongst all team members via a Git repository. This achieves another level of standardisation, which is actually beneficial (trust me). We don't need to start coding our own AI agent — we can see what is already available and orchestrate it with simple Markdown!

Don't forget to review [Awesome Copilot][Awesome] for inspiration!



[the-rise-of-vibe-coding]: the-rise-of-vibe-coding
[karpathy]: https://x.com/karpathy/status/1886192184808149383?t=7EBkBxJuW-5De2hqxso_jA&s=03
[awesome]: https://github.com/github/awesome-copilot/
[manual]: https://docs.github.com/en/copilot/how-tos/configure-custom-instructions/add-repository-instructions#writing-effective-repository-custom-instructions
[manual-prompts]: https://docs.github.com/en/copilot/tutorials/customization-library/prompt-files
[prompts-vsc]: https://code.visualstudio.com/docs/copilot/customization/prompt-files
[plan.prompt]: https://github.com/c64lib/gradle-retro-assembler-plugin/blob/develop/.github/prompts/plan.prompt.md
[exec.prompt]: https://github.com/c64lib/gradle-retro-assembler-plugin/blob/develop/.github/prompts/exec.prompt.md
[plan-update.prompt]: https://github.com/c64lib/gradle-retro-assembler-plugin/blob/develop/.github/prompts/plan-update.prompt.md
