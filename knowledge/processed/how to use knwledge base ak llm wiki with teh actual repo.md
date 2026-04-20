Since you’ve set up the LLM Wiki (knowledge base) as a separate entity from your code, you’ve essentially built a "Long-Term Memory" for your trading system.
Here is how you actually utilize this setup as you transition from design to active development:
## 1. The "Context Injection" Workflow
When you start a coding session in the trading-system repo, your first step is to "prime" your LLM (or yourself) using the Wiki.

* The Move: Feed the AGENTS.md and relevant adr/ files into your LLM prompt.
* The Result: The LLM won't just give you generic Python/Go code; it will give you code that respects your "modular monolith" architecture and "Deterministic Control Layer" because it has been explicitly told those rules exist in the Wiki.

## 2. Using the knowledge/raw/ as a Scratchpad
As you develop the trading system, you will encounter edge cases (e.g., "How should I handle partial fills in the domain model?").

* The Move: Don't let these thoughts vanish. Create a dated file in knowledge/raw/: 2026-04-19-order-fill-logic.md.
* The Result: You offload the mental burden. Later, you can "promote" the best parts of that note to a canonical page in knowledge/entities/order.md.

## 3. Traceability (The "Why" vs. The "What")

* The Code Repo (trading-system): Contains the "What" (the actual logic, Dockerfiles, schemas).
* The Wiki Repo (knowledge-base): Contains the "Why" (the logic behind the schema, the failed ideas you discarded).
* The Utilization: When you hit a bug six months from now in your "Context Intelligence Layer," you search the Wiki for the topic. It will tell you the intent of that layer, helping you realize if the bug is a code error or a design flaw.

## 4. Evolution of "Prompts"
You have a prompts/ directory in the Wiki. Use this to store System Instructions for different tasks.

* Example: A code-reviewer.md prompt that tells the LLM: "When I paste code from the trading-system repo, check it against the rules in adr/002-rules-vs-context.md."
* The Result: You create a feedback loop where the Wiki enforces the quality of the code repo.

## 5. Managing the "Modular Monolith"
Since you are starting a modular monolith, boundaries are everything.

* The Move: Use knowledge/index/system-map.md to define exactly which modules are allowed to talk to each other.
* The Result: Before you write a single line of import code, you verify the connection in the Wiki. This prevents the "spaghetti" that usually kills trading bots.

In the Karpathy LLM Wiki pattern, you should mention the existence of each to the other but maintain a clear separation of "Labor" between them. Think of them as two different job descriptions for the same AI. [1, 2, 3] 
## 1. The Repo Root AGENTS.md (The "Builder")
This file should focus on execution and code standards. Its main job is to tell the AI how to behave inside the trading-system/ repository. [3, 4] 

* Key Reference: Add a section at the top of this file:

Project Knowledge Base: This repository has a separate, permanent memory located at C:\Users\bosto\dockerstuff\knowledge-base\trading-system\. Reference that directory for deep architecture context, historical decisions (ADRs), and canonical entity definitions before implementing new features.

* Focus: Build commands, test suites, linter rules, and local directory structure. [5] 

## 2. The Knowledge Base AGENTS.md (The "Librarian")
This file focuses on maintenance and ingestion. Its job is to keep the Wiki organized and ensure new information is cross-linked. [1, 5, 6] 

* Key Reference: Add a section here:

Linked Application Repo: The active codebase for this project is at C:\Users\bosto\dockerstuff\trading-system. When summarizing new "Raw" notes, verify if they contradict the existing README.md or active code in the main repo.

* Focus: Front matter requirements, wikilink formatting, and the process of "promoting" raw notes to canonical entities. [1] 

## Why keep them separate?

* Context Budgets: Every word in an AGENTS.md file is loaded into the AI's "short-term memory" (context window) every time you send a message. If you merge them into one giant file, you waste tokens and "distract" the AI with building instructions when it's just trying to write a wiki entry.
* Role Clarity: By having two files, you can tell the AI: "You are now in 'Librarian' mode; follow the Wiki rules" or "You are in 'Developer' mode; follow the Repo rules". [2, 3, 7, 8, 9] 

## Practical Tip for CLI Tools
If you use a tool like Cursor or Aider, you can "attach" the other repository's AGENTS.md or a specific ADR file by simply mentioning its path or using a specific command (like @file in Cursor) to bridge the two worlds when needed. [5] 

[1] [https://gist.github.com](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
[2] [https://code.visualstudio.com](https://code.visualstudio.com/docs/copilot/customization/custom-agents)
[3] https://agents.md
[4] [https://mintlify.com](https://mintlify.com/agentsmd/agents.md/introduction#:~:text=AGENTS.md%20is%20a%20Markdown%20file%20you%20place,agents%20can%20find:%20Build%20and%20test%20commands.)
[5] [https://www.youtube.com](https://www.youtube.com/watch?v=M-VbHKuYTPY)
[6] [https://www.mindstudio.ai](https://www.mindstudio.ai/blog/karpathy-llm-wiki-pattern-personal-knowledge-base-without-rag)
[7] [https://www.augmentcode.com](https://www.augmentcode.com/guides/how-to-build-agents-md)
[8] [https://www.aihero.dev](https://www.aihero.dev/a-complete-guide-to-agents-md)
[9] [https://todatabeyond.substack.com](https://todatabeyond.substack.com/p/do-agentsmdclaudemd-files-help-coding)


