# Learning context for Claude

This repo is a **learning project**. Read this file before you help.

## Who I am

- rizkym4l, a beginner in AI engineering. Background: web development.
- Goal: learn the basics well enough to build small AI-powered apps without surprises.
- Style: learn by doing (inspired by PewDiePie). About 80% building, 20% reading. Short sessions, about 80 minutes a day.

## How to teach me

1. Reply in simple, casual Indonesian. Keep code, commands, and paths exact.
2. Lead with the action. Use numbered steps. No long intros.
3. **You write the code, one task at a time. I run it and check it.** Follow [docs/WORKFLOW.md](docs/WORKFLOW.md). After each task, ask me 1–2 explain-back questions and suggest one thing to break on purpose. I type code myself when I experiment. Do not build whole phases at once.
4. Go slowly. Do not run many steps alone and then say "it works". Show what you ran and how I can check it myself.
5. When you explain a concept, use a small example or an analogy (I liked the restaurant analogy for thin clients).
6. When something is unclear, give 3 numbered options. I reply with a number.
7. Point me to the matching code in nakama when it helps (see below), as a reference, not a textbook.

## What I already learned

| Topic | What I know |
|---|---|
| Thin vs thick client | Thin client = only sends input and shows output; the brain is on the server. Nakama: web, CLI, Telegram, WhatsApp are all thin clients over one server. |
| Agent loop | LLM → asks for a tool → code runs the tool → result goes back to the LLM → repeat until no tool call. |
| Tools | 4 parts: `name`, `description`, `parameters` (JSON Schema), `run`. The LLM sees only the first 3. Description is a prompt. Validate input. Return errors to the model. Limit what tools can touch. |
| Tokens, temperature | LLM predicts the next token. Temperature 0 = consistent; high = varied. |
| `top_k`, `top_p` | `top_k` keeps the N most likely tokens; `top_p` keeps tokens until their total probability reaches P. Temperature then picks among them. |
| Chat history | The LLM has no memory. The app sends the full history every time, and history strongly changes the answer (I saw it repeat "Mimi"). |
| Experiment rule | Change one thing at a time (for example `/clear` between tries). |

Started but not finished: **messages and roles** (`system` / `user` / `assistant`) with the Ollama `/api/chat` API, including faking an `assistant` message. Not yet: context window in practice, structured output, embeddings/RAG, evals.

## My machine

- Windows 11, 32 GB RAM, GPU AMD RX 6700. Shells: PowerShell and Git Bash.
- Bun 1.4.4.
- Ollama 0.40.16 at `%LOCALAPPDATA%\Programs\Ollama\ollama.exe`. Model `qwen3:8b` runs 100% on GPU.
- Ollama context is still the default **4096**. Phase 0.2 sets `OLLAMA_CONTEXT_LENGTH=16384`.

## This project: crm-mini

A WhatsApp AI sales agent + simple CRM for a fake shop ("Toko Testing"). Practice only, fake data.

- Docs: [docs/PRD.md](docs/PRD.md), [docs/DESIGN.md](docs/DESIGN.md), [docs/TASKS.md](docs/TASKS.md), [docs/WORKFLOW.md](docs/WORKFLOW.md). Docs are in English.
- Key decisions: own agent loop in TypeScript (no framework), Bun + SQLite + Zod, Ollama `qwen3:8b`, Baileys for WhatsApp with a spare number, plain HTML web page, prices and statuses enforced by code (not by the model).
- **Next step: Phase 0 in TASKS.md.**

## Reference: nakama

Nakama is an open-source agent platform at `c:\Ai Engineering\nakama` (Bun + TypeScript). Useful files:

| Topic | File |
|---|---|
| Agent loop | `packages/agent/src/chat.ts` (`runConversation`, loop at about line 810) |
| One tool call | `packages/agent/src/tool-loop.ts` (`executeToolCall`) |
| Tool shape | `packages/core/src/contract.ts` (`ToolDefinition`), example `packages/core/src/tools/search-files.ts` |
| Tool to provider format | `apps/server/src/providers/openai/index.ts` (`toOpenAITools`) |
| System prompt | `packages/agent/src/chat-prompt.ts` (`buildChatSystemPrompt`) |
| Context compaction | `packages/agent/src/history-compaction.ts` |
| WhatsApp with Baileys | `apps/platform/whatsapp/src/socket.ts` |
| Architecture overview | `ARCHITECTURE.md` |
