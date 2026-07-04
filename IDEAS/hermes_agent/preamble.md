---
created: 2026-07-03
type: reference
tags: [book, hermes, ai-agent]
source: victoria-k-emails
---

# Preamble — The Personal AI Operating System

> *From a series of emails between Dysthemix and Victoria K, building out the architecture for a production-grade Hermes Agent.*

What you're describing is essentially a personal AI operating system, not just a chatbot.
The goal is to build an agent that can:

Manage email autonomously Respond on Telegram Organize your notes and knowledge Write software Continue long-term coding projects Search the web Remember previous conversations Schedule tasks Interface with your computer Chain together multiple tools automatically Learn your preferences over time This is absolutely possible today, although no single project does all of it perfectly. The best systems combine several open-source projects rather than relying on one "AI agent."
The Architecture I would build it like this.
You │ │ Voice / Telegram / Desktop │ ▼ Hermes (Main Reasoning LLM) │ ┌───────────────┼──────────────────┐ │ │ │ ▼ ▼ ▼ Memory Agent Runtime Tool Calling (Obsidian) (OpenHands) MCP Servers │ │ │ │ │ │ ▼ ▼ ▼ Notes Coding Projects Gmail Knowledge Python Telegram Tasks GitHub Calendar Ideas Docker Browser

The Brain Hermes should be the reasoning model.
Current recommendations:

Hermes 3 405B Hermes 4 when stable Qwen3 235B DeepSeek R1 Kimi K2 Hermes has excellent tool calling and personality.
Run through:

Ollama (easy) vLLM llama.cpp OpenRouter Together AI If you have GPUs:
Use vLLM.

If you don't:

Use OpenRouter.

Long-Term Memory This is where Obsidian comes in.
Instead of keeping memory inside the LLM, every important interaction becomes markdown.

Example:

Memory/

Projects/

Life/

Programming/

Business/

Ideas/

People/

Meetings/

Research/

Each file:
Programming/ Hermes Agent.md

Business/ Startup Ideas.md

Projects/ Discord Bot.md

Hermes constantly writes notes into Obsidian.
Example

## Learned today

User prefers Rust.

Current project

Agent framework

Problems

Telegram API failing

Then the memory system retrieves relevant notes before every conversation.
That is effectively infinite memory.

Vector Database Obsidian alone isn't enough.
Use embeddings.

Choices:

Chroma Qdrant Weaviate LanceDB I recommend Qdrant

Pipeline:

Obsidian Markdown

↓

Chunk files

↓

Embedding Model

↓

Qdrant

↓

Semantic Search

↓

Hermes

Now Hermes remembers everything.
Agent Runtime This is the most important component.
I'd recommend:

OpenHands Excellent coding agent.
Can:

modify code run tests execute terminals debug Git Docker Alternative Claude Code

Still arguably the strongest coding workflow, though it's not fully open source.

For local:
OpenHands + Hermes

MCP (Model Context Protocol) MCP is becoming the standard way for LLMs to use tools.
Instead of hardcoding Gmail, Telegram, GitHub, etc., Hermes connects to MCP servers.

Examples:

Filesystem

GitHub

Git

Docker

SQLite

Postgres

Browser

Slack

Discord

Telegram

Email

Google Drive

Calendar

Obsidian

Notion

Spotify

Weather

Search

ArXiv

Wikipedia

GitLab

AWS

Azure

Kubernetes

Literally hundreds now exist.

Email Automation Gmail API.
Workflow

Unread email

↓

Hermes reads

↓

Classify

↓

If advertisement

Archive

↓

If invoice

Move to Finance

↓

If GitHub notification

Project folder

↓

If personal

Notify you

↓

If simple response

Draft reply

↓

If urgent

Telegram notification

Never allow automatic sending initially.
Only drafts.

Telegram Telegram Bot API.
Hermes can:

Read messages

Summarize

Reply

Schedule

Forward

Translate

Answer FAQs

Example

Friend:

"Coming tonight?"

Hermes:

"I'm finishing work right now. I'll let you know in about an hour."

You approve.

Eventually:

Automatic approval for trusted contacts.

Calendar Google Calendar API.
Hermes can

Schedule

Reschedule

Find conflicts

Estimate travel

Create reminders

Coding Assistant OpenHands.
Capabilities:

Open project

Read files

Understand architecture

Write code

Run tests

Fix errors

Commit changes

Open pull requests

This is probably the strongest coding setup today.

Browser Automation Playwright.
Hermes can

Fill forms

Download files

Research

Compare prices

Read dashboards

Navigate websites

Scrape information

Voice Use Whisper

or

Whisper.cpp

Speech

↓

Text

↓

Hermes

↓

ElevenLabs

or

Piper

↓

Voice

Task Manager Instead of ChatGPT Tasks.
Use

Todo.md

Projects.md

Waiting.md

Ideas.md

Hermes updates them automatically.
Daily Journal Automatically generated.
Today

Emails answered

Projects worked

Ideas

Meetings

Expenses

Health

Mood

Tomorrow

Stored in Obsidian.
Project Memory Every coding project gets README

Architecture

Progress

Problems

TODO

Ideas

Files changed

Hermes updates automatically.
GitHub Integration Hermes can Clone repos

Commit

Push

Review PRs

Write documentation

Generate issues

Read Discussions

Explain code

Research Mode Use multiple search providers.
Perplexity

Tavily

Brave Search

SerpAPI

Wikipedia

ArXiv

GitHub

Then summarize.

Local Execution Docker.
Every coding task runs in isolated containers.

Never let Hermes execute directly on your host.

Security Separate permissions.
Hermes should ask before:

Deleting files

Sending emails

Purchasing anything

Git push

SSH

Database deletion

Everything else can be autonomous.

Memory Pipeline Conversation

↓

Summarize

↓

Extract facts

↓

Save markdown

↓

Embed

↓

Store Qdrant

↓

Retrieve later

↓

Inject into prompt

This becomes lifelong memory.
Knowledge Graph Better than plain notes.
Store relationships.

Example

John

works at

OpenAI

John

likes

Rust

Rust

used in

Current Project

Use a graph database like Neo4j if you want rich relationship queries, or keep it simpler by linking markdown notes in Obsidian.
Suggested Technology Stack Layer Recommendation LLM Hermes 3/4 or Qwen3 Memory Obsidian Semantic Search Qdrant Embeddings BAAI bge-m3 or nomic-embed-text Coding Agent OpenHands Workflow Automation n8n Tool Protocol MCP Browser Automation Playwright Email Gmail API Messaging Telegram Bot API Voice Input Whisper.cpp Voice Output Piper or ElevenLabs Containerization Docker Source Control Git + GitHub Runtime Python (FastAPI) Scheduling APScheduler or Temporal Monitoring Langfuse or OpenTelemetry

Development Roadmap Rather than trying to build everything at once, I'd approach it in phases:
Foundation: Set up Hermes, Docker, Git, Obsidian, Qdrant, and a simple FastAPI service. Get retrieval from Obsidian working before adding automation.
Core Tools: Add MCP servers for the filesystem, Git, GitHub, browser automation, and email. Ensure every tool call requires explicit confirmation for sensitive actions.
Coding Agent: Integrate OpenHands so the agent can inspect repositories, edit code, run tests, and propose commits.
Productivity Automation: Connect Gmail, Telegram, Google Calendar, and your task system. Start with suggestions and draft responses rather than fully autonomous actions.
Memory & Learning: Automatically summarize conversations, update Obsidian notes, generate embeddings, and retrieve relevant context for future sessions.
Advanced Features: Add voice interaction, research pipelines, dashboards, scheduled jobs, and eventually selective autonomous workflows for trusted, low-risk tasks.
What I'd build today If I were starting from scratch in 2026, I'd aim for a modular "Jarvis"-style assistant with these core pieces:
Hermes as the primary reasoning model.
OpenHands dedicated to software engineering tasks.
Obsidian as the human-readable long-term knowledge base.
Qdrant for semantic memory retrieval.
MCP as the standard interface for tools and services.
n8n to orchestrate recurring workflows and integrations.
FastAPI as the central backend that coordinates requests.
Docker to isolate services and safely execute code.
Playwright for browser-based automation.
GitHub as the source of truth for all code and projects.
This architecture is scalable, vendor-neutral, and lets you swap individual components (models, vector databases, automation tools) without redesigning the entire system.
For someone serious about creating a powerful personal AI assistant, this is the direction I'd recommend over a monolithic agent framework. It gives you a system that's easier to maintain, extend, and trust as it takes on more responsibility.

On Thu, Jul 2, 2026 at 9:47 PM Victoria K <vkushelnyk@gmail.com> wrote:

> Stopped up with the store as well. Home in a minute or two.
> > On Thu, Jul 2, 2026 at 9:31 PM Dysthemix <quantum.nomad79@gmail.com> > wrote:
> >> k see you soon >> >> On Fri, Jul 3, 2026 at 3:24 AM Victoria K <vkushelnyk@gmail.com> wrote:
>> >>> Coming down 137th Ave. Just at 82nd St. now. Gonna stop at Pops and get >>> some weed. Should be home in 10 >>> On Thu, Jul 2, 2026 at 9:19 PM Dysthemix <quantum.nomad79@gmail.com> >>> wrote:
>>> >>>> okay babe see you soon.  thats crazy i figured something went down!
>>>> >>>> On Fri, Jul 3, 2026 at 3:15 AM Victoria K <vkushelnyk@gmail.com> wrote:
>>>> >>>>> Dropped tay at library on 118 near east glen. On my way home. Should >>>>> be home in 15 min.
>>>>> >>>>> Tay just got out of jail again. Jerome got arrested.
>>>>> >>>>> ❤️ >>>>> >>>>> On Thu, Jul 2, 2026 at 8:41 PM Victoria K <vkushelnyk@gmail.com> >>>>> wrote:
>>>>> >>>>>> Hello my love >>>>>> >>>>>> I’m done work, but I’m running into Taylor on the way home. I’ll >>>>>> shoot you another email when I’m done and on my way home love you.
>>>>>> >>>>>> On Thu, Jul 2, 2026 at 3:13 PM Dysthemix <quantum.nomad79@gmail.com> >>>>>> wrote:
>>>>>> >>>>>>> sorry that was a fraud email i sent you a message from before.
>>>>>>> sorry about that hun.  lovre you so so much hope you have as best of s >>>>>>> shift sd you possibly can and i cant wait till you get home.  lemme know >>>>>>> what time thats gunna approx be and ill make us a pizza or something!
>>>>>>> >>>>>>> much love and kisses >>>>>>> >>>>>>