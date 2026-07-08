# A-Developer's Guide to Efficient AI Prompting
Every Token Counts: A Developer's Guide to Efficient AI Prompting
# Stop Burning Tokens: A Developer's Guide to Smarter AI Prompting

> A practical, tool-agnostic guide for developers using GitHub Copilot, Cursor, 
> Claude, ChatGPT, Kiro, or any AI assistant.

---

## Introduction

I've watched developers open a chat with their AI tool, paste their entire 
codebase, type "fix this," and wonder why the response is slow, expensive, 
and barely useful.

If you're paying for an AI coding assistant — or using one on a team plan — 
you've probably felt the sting of a session that ate through your quota without 
delivering much. The frustrating part? Most of that waste is avoidable. It 
starts with how you write your prompts.

This guide is **tool-agnostic**. Whether you're on GitHub Copilot, Cursor, 
Claude, ChatGPT, Kiro, or anything else, these habits will get you better 
results and lower your token usage at the same time.

No fluff. Just practical habits you can apply today.

---

## Table of Contents

1. [What Is a Token?](#1-what-is-a-token)
2. [Be Specific. Always.](#2-be-specific-always)
3. [Send Only Relevant Context](#3-send-only-relevant-context)
4. [Work in Small Steps](#4-work-in-small-steps)
5. [Use Persistent Instructions](#5-use-persistent-instructions)
6. [Constrain the Output Format](#6-constrain-the-output-format)
7. [Correct Early, Not After](#7-correct-early-not-after)
8. [Know When Not to Use AI](#8-know-when-not-to-use-ai)
9. [Advanced Techniques](#9-advanced-techniques)
10. [Quick Reference Cheat Sheet](#10-quick-reference-cheat-sheet)
11. [Final Thoughts](#11-final-thoughts)

---

## 1. What Is a Token?

Before diving into habits, it helps to understand what you're actually paying for.

AI models don't process words the way humans read them. They break text into 
**tokens** — chunks of roughly 3–4 characters, or about ¾ of a word. 

Here's a rough breakdown:

| Text | Approx. Tokens |
|---|---|
| `"Hello, world!"` | 4 tokens |
| A typical line of code | 5–15 tokens |
| A 100-line function | 300–600 tokens |
| A 500-line file | 1,500–3,000 tokens |
| An entire codebase | tens of thousands |

Most platforms charge for both:
- **Input tokens** — everything you send (your prompt, attached files, context)
- **Output tokens** — everything the model sends back (the response)

Output tokens are often priced **higher** than input tokens. So a vague prompt 
that forces the model to guess and over-explain costs you on both ends.

The fix isn't to write less — it's to write smarter.

---

## 2. Be Specific Always.

This is the single most impactful habit. Vague prompts are expensive prompts. 
When you're not specific, the model fills in the gaps with assumptions — 
producing longer responses that may not even address your actual problem.

### Real-world examples

**Scenario: You have a bug in your authentication code**

❌ Vague prompt:

Fix my code 
The model doesn't know which file, which function, or what the bug is. 
It will either ask clarifying questions (more tokens) or make broad guesses 
(more tokens, probably wrong).

✅ Specific prompt:

In auth.ts, the getUser function on line 42 throws a null reference error when userId is undefined. Fix it so it returns null instead of throwing.

One prompt. One focused response. Done.

---

**Scenario: You want to add a feature**

❌ Vague prompt:


✅ Specific prompt:


The specific version tells the model exactly what to build, where to build it, 
and how to align with the existing code style. The response will be shorter, 
more accurate, and need fewer corrections.

---

**Scenario: You want an explanation**

❌ Vague prompt:

Explain this function


✅ Specific prompt:


Specificity also means telling the model the *format and length* you want — 
more on that in section 6.

---

### Why this saves tokens

Each clarification round costs tokens. A vague prompt often leads to:
1. Model asks for clarification (output tokens)
2. You provide more detail (input tokens)
3. Model responds again (more output tokens)

One specific prompt collapses all of that into a single exchange.

---

## 3. Send Only Relevant Context

Most AI tools let you attach files, paste code, or reference entire folders. 
More context feels safer — surely the model will do better with more 
information? In practice, the opposite is often true.

**Irrelevant context has two costs:**
1. It inflates your input token count directly
2. It dilutes the signal, making the model's response less focused

### Examples

**Scenario: Debugging a single function**

You have a 400-line file with a bug in one function. 

❌ What most people do:
Paste the entire 400-line file.

✅ What you should do:

Here's the function with the bug:

[paste the 20-line function]

Error message: "Cannot read properties of undefined (reading 'id')" It happens when the API returns an empty array. How should I handle this?


You went from ~1,200 tokens of context down to ~100. The model has everything 
it needs and nothing it doesn't.

---

**Scenario: Asking about a React component**

❌ Over-contexted prompt:

[pastes entire App.tsx, router config, all CSS, package.json, and three unrelated components]

Why isn't my button working?


✅ Lean prompt:

This button's onClick handler isn't firing. Here's the component:

[pastes just the 30-line component]

The button is inside a form — could that be the issue?


You already have a hypothesis. Give the model just enough to confirm or 
correct it.

---

**Scenario: Working with a large codebase**

When you genuinely need broad context, be strategic:
- Include the relevant file + any files it directly imports
- Include the error message or failing test output
- Describe the surrounding architecture briefly in plain text rather than 
  pasting it all

Plain text descriptions are token-efficient. "This is an Express REST API 
using PostgreSQL and Prisma" costs ~15 tokens and conveys architecture 
that would take hundreds of tokens to demonstrate with code.

---

## 4. Work in Small Steps

The "do everything in one prompt" approach is tempting but costly. A large 
request generates a large response, and if any part of it is wrong, you've 
spent all those tokens on something you need to redo.

Small, iterative prompts keep you in control and let you catch mistakes early 
— before they compound.

### Example: Building an authentication system

❌ One large prompt:


Build me a complete JWT authentication system with login, register, password reset, email verification, refresh tokens, rate limiting, and middleware for protected routes. Use Express and PostgreSQL.

This will generate hundreds of lines of code in one shot. If the structure 
is wrong, the token patterns are off, or it misunderstands your database 
schema, you've wasted a large context window.

✅ Iterative approach:

**Step 1 — Plan first:**

Outline the architecture for a JWT auth system in Express with PostgreSQL. List the files I'll need and what each one does. No code yet.


**Step 2 — Scaffold:**

Based on that outline, create the folder structure and empty files with just the function signatures and comments.

Implement the registerUser function in auth/authController.ts. Hash passwords with bcrypt and store in the users table. Here's the Prisma schema: [paste just the User model]


**Step 4 — Continue piece by piece:**
Now implement loginUser in the same file. Return a JWT signed with the user's id and role. Token should expire in 24 hours.


Each step is small, reviewable, and correctable. Total tokens used will 
likely be less than the single large prompt — and the result will be 
significantly better.

---

### The planning trick

Before any complex task, spend one cheap prompt on planning:

Before writing any code, outline the approach for [task]. List the steps, potential issues, and any decisions I need to make.


This forces alignment before implementation. A planning prompt costs 
~50–100 tokens. Finding out the model went the wrong direction after 
500 lines of code costs much more.

---

## 5. Use Persistent Instructions

If you find yourself typing the same context at the start of every session, 
you're paying for it every single time.

Common repeated context includes:
- Preferred language and framework
- Code style (tabs vs spaces, naming conventions)
- Libraries to use or avoid
- Project architecture overview
- Output preferences ("always include TypeScript types")

### How to set persistent instructions by tool

| Tool | How to Set Persistent Instructions |
|---|---|
| **ChatGPT** | Settings → Personalization → Custom Instructions |
| **Claude** | Create a Project, add instructions to the Project prompt |
| **Cursor** | Add a `.cursorrules` file to your project root |
| **GitHub Copilot** | Add a `.github/copilot-instructions.md` file |
| **Kiro** | Create `.kiro/steering/*.md` files in your project |
| **Windsurf** | Add a `.windsurfrules` file to your project root |

### Example persistent instruction (for a TypeScript project)

You are helping with a TypeScript React application using:

React 18 with functional components and hooks only (no class components)
Tailwind CSS for styling (no inline styles or CSS modules)
React Query for server state, Zustand for client state
Zod for validation
ESLint + Prettier (follow existing code style)
Always:

Include TypeScript types for all function parameters and return values
Handle loading and error states in components
Write accessible HTML (proper ARIA attributes, semantic elements)
Never:

Use 'any' type
Use default exports (named exports only)
Install new libraries without asking first



Write this once. Every session starts with the model already knowing your 
standards — no repeated context, no drift from your conventions.

---

## 6. Constrain the Output Format

If you don't specify how to respond, the model decides — and it defaults 
to verbose. Explicitly constraining the output format is one of the 
easiest ways to reduce output tokens.

### Format constraints you can use

**Length limits:**

Keep the response under 150 words. Answer in 3 bullet points maximum. Give me a one-sentence summary.


**Code-only responses:**
Return only the updated function, no explanation. Give me just the code. I'll ask questions if I need clarification. Return only the lines that changed, not the full file.


**Structured output:**

Format the response as: Problem / Root Cause / Fix List the steps as a numbered list, one line each. Return the result as a JSON object with keys: issue, cause, solution

**Scope limiting:**

Only change what's necessary to fix the bug. Don't refactor anything else. Don't add comments unless the logic is genuinely complex. Don't suggest alternative approaches — just implement what I asked.


### Example comparison

❌ Open-ended prompt:

What's wrong with this function?

[pastes function]


Likely response: 3–5 paragraphs analyzing the code, suggesting 
improvements, explaining the fix, showing the corrected version, 
and adding a note about best practices. Maybe 400–600 tokens.

✅ Constrained prompt:

What's wrong with this function? Return: one sentence describing the bug, then the fixed function. Nothing else.

[pastes function]

Likely response: One sentence + corrected function. Maybe 80–150 tokens. 
Exactly what you needed.

---

## 7. Correct Early, Not After

If a response starts going the wrong direction, stop it. Don't wait for 
the model to finish a 500-line response you're going to throw away.

In tools that stream responses (most of them), you can interrupt mid-generation. 
Use that. A two-sentence redirect is always cheaper than letting a bad 
response complete and then requesting a full redo.

### Recognizing when to redirect

Stop and redirect when you see:
- The model misunderstood the tech stack ("I'll use MongoDB" when you use Postgres)
- It's solving a different problem than you described
- It's adding features or complexity you didn't ask for
- It's rewriting code you wanted preserved
- The approach won't fit your architecture

### How to redirect efficiently

❌ Costly redirect:

That's wrong. Start over and do it the right way.

This gives the model nothing to work with. It'll make the same mistakes.

✅ Efficient redirect:

Stop — two issues:

1.We're using PostgreSQL with Prisma, not MongoDB
2.Don't modify the existing UserService class, add a new method to it Try again with those constraints.


Specific corrections cost fewer tokens than full regenerations.

---

## 8. Know When Not to Use AI

This is underrated but important. Every AI prompt costs tokens. Some tasks 
don't need AI at all.

### When to skip AI entirely

| Task | Better Tool |
|---|---|
| Syntax lookup | Official docs, MDN, DevDocs |
| Code formatting | Prettier, ESLint, your IDE |
| Renaming a variable | IDE refactor tool |
| Generating boilerplate | Snippets, templates, scaffolding CLIs |
| Running tests | Your test runner directly |
| Git operations | Git CLI or IDE git tools |
| Finding a file | Your IDE's file search |

### When AI genuinely earns its cost

- Debugging complex, multi-layered issues
- Understanding unfamiliar codebases
- Designing architecture and system structure
- Writing tests for edge cases
- Translating requirements into implementation plans
- Code review and identifying subtle bugs
- Writing documentation from code
- Explaining why something works (or doesn't)

The rule of thumb: if you could solve it in under 60 seconds with a 
different tool, use that tool.

---

## 9. Advanced Techniques

Once you've mastered the basics, these techniques push efficiency further.

### Role prompting

Giving the model a specific role narrows its response scope and reduces 
unnecessary hedging.

You are a senior TypeScript engineer doing a focused code review. Identify only security vulnerabilities in this function. List each one as: [severity] - [issue] - [fix]

[paste function]


Without the role, you might get a general review covering style, 
performance, readability, and security. With the role, you get 
exactly what you asked for.

---

### Chain of thought for complex problems

For genuinely complex debugging or architecture questions, explicitly 
asking the model to reason before answering improves accuracy — which 
means fewer correction rounds.


Before giving a solution, think through:

1.What the current code does
2.What the expected behavior is
3.Where the gap is
4.The simplest fix
5.Then provide the fix.


This costs slightly more tokens upfront but avoids the expensive 
back-and-forth of a wrong first answer.

---

### Diff-style responses

Instead of asking the model to return full files, ask for diffs:

Show me only the lines that need to change, in this format:

[old line]
[new line]


For large files, this can reduce output tokens by 80–90%.

---

### Batching related questions

If you have multiple small questions about the same code, batch them:

❌ Three separate prompts (3x overhead):

Prompt 1: What does this function do? Prompt 2: Is there a performance issue here? Prompt 3: How would you test this?


✅ One batched prompt:

For this function, answer three questions:

What does it do?
Are there any performance issues?
How would you test it? Keep each answer to 2–3 sentences.
[paste function]


Same information, roughly one-third the token overhead.

---

### Use examples to guide output style

If you have a strong preference for a specific code style, show an example 
rather than describing it:


Write a similar function to this one:

[paste example function in your preferred style]

The new function should [describe what it does]. Match the exact style of the example above.


Showing is more token-efficient than describing style in words, and 
the output will be far more consistent.

---

## 10. Quick Reference Cheat Sheet

### Prompt quality checklist

Before sending any prompt, run through this:

- [ ] Did I specify the file and function name (if relevant)?
- [ ] Did I include only the code the model actually needs?
- [ ] Did I describe the expected vs actual behavior (for bugs)?
- [ ] Did I constrain the output format?
- [ ] Is this a task AI is actually the right tool for?
- [ ] Have I already defined this context in persistent instructions?

---

### Token cost comparison

| Prompt Pattern | Relative Token Cost | Quality |
|---|---|---|
| Vague, no context | Low input, high output | Poor |
| Vague + entire codebase | Very high input, high output | Poor |
| Specific, relevant context | Medium input, low output | High |
| Specific + format constraint | Medium input, very low output | High |
| Iterative small steps | Low per step | High |

---

### Format constraint quick reference

Length
"Keep under [X] words" "Answer in [X] bullet points" "One sentence only"

Code
"Return only the changed function" "Code only, no explanation" "Show as a diff"

Structure
"Format as: Problem / Cause / Fix" "Return as JSON" "Numbered list, one line each"

Scope
"Don't refactor anything else" "Don't add features I didn't ask for" "Fix only this specific issue"


---

## 11. Final Thoughts

Better prompting is not just a cost optimization — it's a skill that makes 
you a more effective developer. The habits that reduce token usage are the 
same habits that produce cleaner, more accurate results:

- **Clarity** — knowing what you want before you ask
- **Precision** — giving the model exactly what it needs
- **Iteration** — building incrementally and correcting early
- **Reuse** — writing instructions once, not repeatedly

The underlying models will keep improving. Pricing will keep changing. 
Context windows will keep expanding. But these fundamentals don't expire 
— they just become better leverage as the tools get more powerful.

Start with one habit. The specificity one (section 2) has the highest 
immediate impact. Build from there.

---

## Contributing

Found a technique that saves tokens and improves results? Open a PR or 
raise an issue. This guide is meant to grow with the ecosystem.

---

## License

MIT — use freely, share widely, attribution appreciated.


---

## References

The claims and techniques in this guide are grounded in official documentation 
and peer-reviewed research. Sources are grouped by section.

---

### Tokens and Pricing

- **OpenAI** — [What are tokens and how to count them?](https://help.openai.com/en/articles/4936856-what-are-tokens-and-how-to-count-them)  
  Official explanation of how tokens work, including that they can be as short 
  as a single character or as long as a full word.

- **OpenAI** — [Tiktoken: Count tokens before sending requests](https://github.com/openai/openai-cookbook/blob/main/examples/How_to_count_tokens_with_tiktoken.ipynb)  
  OpenAI's official Python tokenizer for counting tokens locally before 
  making API calls.

- **OpenAI Tokenizer (interactive tool)** — [platform.openai.com/tokenizer](https://platform.openai.com/tokenizer)  
  Paste any text and see exactly how many tokens it uses.

---

### Specificity and Clarity in Prompts

- **OpenAI** — [Best practices for prompt engineering with the OpenAI API](https://help.openai.com/en/articles/6654000-best-practices-for-prompt-engineering-with-the-openai-api)  
  Recommends that prompts be clear, specific, and provide enough context to 
  avoid ambiguity.

- **OpenAI** — [Prompt engineering best practices for ChatGPT](https://help.openai.com/en/articles/10032626-prompt-engineering-best-practices-for-chatgpt)  
  Covers iterative refinement — starting with an initial prompt, reviewing 
  the response, and refining based on output.

- **Anthropic** — [Prompt engineering overview](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/overview)  
  Claude's official prompt engineering guide covering clarity, examples, 
  role prompting, and agentic systems.

- **Anthropic** — [Prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)  
  Model-specific guidance for current Claude models plus general techniques 
  for output formatting, tool use, and thinking.

---

### Working in Small Steps (Prompt Chaining)

- **Anthropic** — [Chain complex prompts for stronger performance](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/chain-prompts)  
  Official documentation on prompt chaining — breaking tasks into distinct 
  sequential steps to improve reliability and control.

- **Anthropic** — [Break complex tasks into subtasks](https://docs.anthropic.com/claude/docs/break-tasks-into-subtasks)  
  Explains when to split a single prompt into multiple prompts and how to 
  pass outputs between them.

---

### Persistent Instructions

- **GitHub** — [Adding repository custom instructions for GitHub Copilot](https://docs.github.com/en/copilot/how-tos/configure-custom-instructions/add-repository-instructions)  
  Official docs for creating `.github/copilot-instructions.md` to give 
  Copilot persistent context across all interactions.

- **GitHub** — [Your first custom instructions](https://docs.github.com/en/copilot/tutorials/customization-library/custom-instructions/your-first-custom-instructions)  
  Tutorial on setting up persistent instructions in GitHub Copilot.

- **Cursor** — [Rules documentation](https://docs.cursor.com/context/rules)  
  Official Cursor docs explaining how rules provide persistent, reusable 
  context at the prompt level for consistent AI guidance.

- **Anthropic** — [Long context prompting tips](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/long-context-tips)  
  Guidance on structuring prompts when working with large inputs, 
  including placement of context for best performance.

---

### Chain of Thought and Token Efficiency Research

- **arXiv (2024)** — [The Benefits of a Concise Chain of Thought on Problem-Solving in Large Language Models](https://arxiv.org/html/2401.05618v3)  
  Peer-reviewed research showing that Concise Chain of Thought (CCoT) 
  reduced average response length by ~49% while achieving an average 
  per-token cost reduction of ~23%, with negligible impact on accuracy 
  for most tasks.

- **arXiv (2024)** — [Token-Budget-Aware LLM Reasoning](https://arxiv.org/html/2412.18547v4)  
  Research on how Chain-of-Thought reasoning incurs significant token 
  overhead and strategies for managing token budgets in reasoning tasks.

---

### Tools Referenced

| Tool | Persistent Instructions Feature |
|---|---|
| [ChatGPT](https://chatgpt.com) | Settings → Personalization → Custom Instructions |
| [Claude](https://claude.ai) | Projects → Project Instructions |
| [Cursor](https://cursor.com) | `.cursor/rules/` or `.cursorrules` file |
| [GitHub Copilot](https://github.com/features/copilot) | `.github/copilot-instructions.md` |
| [Windsurf](https://windsurf.com) | `.windsurfrules` file |

---

### Further Reading

- [OpenAI Prompt Engineering Guide](https://platform.openai.com/docs/guides/prompt-engineering) — Official deep-dive into techniques like few-shot prompting, structured output, and system messages.
- [Anthropic's Prompt Library](https://docs.anthropic.com/en/resources/prompt-library/career-coach) — Real-world examples of well-structured prompts across different use cases.
- [GitHub Copilot Customization Cheat Sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet) — Quick reference for all Copilot customization features.

---

*Written by [Your Name] · [Your Website URL] · [Your Medium Profile URL]*




