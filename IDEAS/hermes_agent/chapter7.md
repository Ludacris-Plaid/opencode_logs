---
created: 2026-07-03
type: reference
tags: [book, hermes, chapter]
chapter: 7
title: "Tooling the Agent — Introducing MCP and External Capabilities"
source: victoria-k-emails
---

# Chapter 7 — Tooling the Agent — Introducing MCP and External Capabilities

Tooling the Agent — Introducing MCP and External Capabilities “Memory makes an AI informed. Tools make it useful.” At this point, Hermes can:
Run on your VPS Talk to you Use a model backend Store structured memory in Obsidian + Qdrant But there’s a hard limitation:
👉 It still cannot reliably do things in the world.

It can reason about your Git repo… but it cannot safely operate Git.

It can suggest email replies… but it cannot send them.

It can describe a Docker fix… but it cannot execute it.

This chapter fixes that.

We introduce MCP (Model Context Protocol) as the standardized tool layer between Hermes and the real world.

7.1 What MCP Actually Is
MCP is a tool interface layer for AI agents.
Instead of hardcoding integrations like:

“write a Gmail plugin” “write a GitHub plugin” “write a filesystem tool” “write a database connector” You instead plug everything into one standard:
Hermes ↓ MCP Client ↓ MCP Servers (tools) ↓ Real systems (Git, FS, DB, APIs)

Each tool becomes a small independent service.
7.2 Why MCP Is Critical
Without MCP, every integration becomes custom code.
That leads to:

messy architecture duplicated logic security issues fragile integrations With MCP:
every tool follows the same interface tools are interchangeable permissions are centralized new capabilities plug in instantly This is what makes your system scalable beyond 10–20 tools.
7.3 The Tool Philosophy
We are NOT building:
a chatbot with plugins We ARE building:
a general-purpose agent runtime with a tool ecosystem Think of MCP as:
“USB ports for intelligence” You can plug anything in.
Unplug it anytime.

7.4 Core Tool Categories
We will eventually connect Hermes to:
File System Tools read/write files search directories edit codebases Developer Tools Git GitHub CI/CD triggers Docker Data Tools PostgreSQL Qdrant Redis Communication Tools Email (Gmail API) Telegram bot Slack (optional) Automation Tools n8n workflows scheduled tasks cron jobs Research Tools web search documentation lookup scraping (Playwright)
7.5 MCP Architecture Overview
Hermes Agent │ MCP Client Layer │ ┌───────────────┼────────────────┐ ▼ ▼ ▼ Filesystem GitHub PostgreSQL ▼ ▼ ▼ Local files Repos Structured data

Each MCP server is:
independent sandboxed replaceable restartable auditable
7.6 Installing the MCP Host Layer
We now extend your ai-os stack.
Inside:

~/ai-os/compose/mcp/

We will eventually run multiple MCP servers as containers.
But first, create the structure:

mkdir -p ~/ai-os/compose/mcp mkdir -p ~/ai-os/services/mcp

7.7 MCP Security Model
This is important.
MCP servers must NEVER:

run as root have unrestricted filesystem access expose public ports directly store secrets unencrypted Instead:
run in Docker connect only to Hermes internally expose minimal API surface use explicit permission rules
7.8 Filesystem MCP (First Tool)
We start simple: filesystem access.
Why?

Because everything Hermes does eventually touches files.

This MCP server allows:

reading project files writing notes editing code searching directories But it will be sandboxed to:
~/ai-os/workspace

Nothing outside this folder.
7.9 Git MCP (Second Tool)
Next:
Git integration.

Capabilities:

status commit diff branch pull push (requires approval) This is what turns Hermes into a real coding assistant.
Without Git tools, it’s just writing suggestions.

With Git tools, it becomes a developer.

7.10 Database MCP (Third Tool)
We connect:
PostgreSQL Qdrant Redis (later) This allows Hermes to:
query structured data store memory efficiently manage embeddings track system state
7.11 Tool Permission System
We introduce a critical concept:
Tool permission levels Level Meaning read-only safe inspection suggest can propose changes draft can create outputs execute can run actions (restricted) admin never default

Example:
Hermes can READ emails It can DRAFT replies It cannot SEND without approval
7.12 MCP Client in Hermes
Hermes must be configured to:
discover MCP servers list available tools request execution handle responses log all tool calls This creates a traceable chain of actions.
7.13 Tool Call Lifecycle
Every action follows this flow:
User request ↓ Hermes reasoning ↓ Select tool ↓ MCP call request ↓ Tool executes in sandbox ↓ Result returned ↓ Hermes continues reasoning

This is how agents become reliable instead of chaotic.
7.14 First Real Use Case
Once MCP is working, Hermes can:
“Open my AI project and fix the Docker Compose issue.” Then:
Reads filesystem Finds compose file Detects misconfiguration Proposes fix Applies patch (if approved) Runs container Reports result This is the first moment your system becomes agentic.
7.15 Logging Everything
Every tool call must be logged:
tool name inputs outputs timestamps success/failure user approval status Stored in:
~/ai-os/logs/mcp/

This is critical for debugging and trust.
7.16 Why MCP Beats Traditional Plugins
Old model:
each tool is custom code tightly coupled to agent hard to maintain MCP model:
standardized protocol tool isolation easy swapping scalable architecture multi-agent compatibility This is why modern agent systems are moving toward it.
7.17 What We Are NOT Doing Yet
We are NOT yet connecting:
Gmail Telegram Browser automation Calendar Voice External APIs Reason:
If we connect everything at once, debugging becomes impossible.

We build incrementally.

7.18 Verification Checklist
Before moving on:
MCP folder structure exists filesystem tool restricted to workspace Git operations are sandboxed tool calls are logged Hermes can list available tools at least one tool executes successfully permission system is enforced If any of these fail, fix before continuing.
End of Chapter 7 You now have the missing piece that transforms Hermes from:
“an AI that talks” into:
“an AI that acts” But it is still isolated from your real workflows.
Right now it can:

read files edit projects manage code But it cannot yet touch your life systems:
email messaging scheduling external communication What’s Next Chapter 8 is where everything starts connecting.
We will integrate:

Gmail (email intelligence) Telegram (real-time communication) Calendar (time management) Notification system (alerts + approvals) This is the chapter where Hermes starts becoming a personal executive assistant, not just a developer tool.
From this point forward, each chapter will dramatically increase how much of your real-world workload the system can handle.

On Fri, Jul 3, 2026 at 4:11 AM Victoria K <vkushelnyk@gmail.com> wrote:

> The Personal AI Operating System > Building a Production-Grade Hermes Agent with Memory, Automation, and > Autonomous Coding > Chapter 6 > Building Long-Term Memory — Giving Hermes a Second Brain > "Intelligence is knowing things. Wisdom is remembering what matters."
> One of the biggest limitations of most AI assistants is that they forget.
> You may spend hours explaining your project, your coding style, your > business, and your preferences, only to repeat yourself in the next > conversation.
> > That isn't acceptable for a personal AI operating system.
> > This chapter solves that problem.
> > By the end of this chapter, Hermes will have:
> > A human-readable knowledge base > Persistent memory > Semantic search > Automatic memory retrieval > Project documentation > Daily journals > A "second brain" that grows over time > This is where your AI stops acting like a chatbot and starts acting like a > colleague.
> 6.1 Understanding Memory > Memory is often misunderstood.
> Many people think:
> > "The AI should remember everything."
> That sounds useful until you realize it becomes expensive, slow, and noisy.
> Instead, we want layered memory.
> > Our system will have four types of memory:
> > Active Conversation > │ > Working Memory > │ > Project Memory > │ > Long-Term Knowledge > > Each layer has a different purpose.
> 6.2 The Four Memory Layers > Layer 1 – Conversation Memory > This is the current chat.
> It exists only while you're working.
> > Example:
> > Fix the login page.
> Hermes remembers the discussion until the task is complete.
> Layer 2 – Working Memory > Temporary notes for ongoing work.
> Examples:
> > Current bug > Next task > Test results > TODO list > This memory changes constantly.
> Layer 3 – Project Memory > Every project gets its own documentation.
> Example:
> > Projects/ > > AI Operating System/ > > README.md > > Architecture.md > > Known Bugs.md > > Roadmap.md > > Meeting Notes.md > > TODO.md > > Months later, Hermes immediately understands the project.
> Layer 4 – Long-Term Memory > Everything worth remembering eventually ends up here.
> Examples:
> > Your coding preferences > Favorite libraries > API keys (never stored in plain text) > Business ideas > Research > Documentation > Design decisions > Meeting summaries > This becomes your digital brain.
> 6.3 Why Obsidian?
> Many AI systems store memory inside a database.
> That works...
> > Until the software disappears.
> > Instead, we're going to store almost everything as Markdown.
> > Advantages:
> > Human readable > Future proof > Git compatible > Easy backup > Works without AI > No vendor lock-in > Your AI memory remains useful even if Hermes disappears tomorrow.
> 6.4 Installing Obsidian > Although Obsidian is a desktop application, think of it as the editor for > your AI's knowledge base.
> Create a vault called:
> > AI Brain > > Store the vault inside your synchronized project directory so it can be > backed up and version-controlled.
> 6.5 Folder Structure > Create the following structure:
> AI Brain/ > > Daily Notes/ > > Projects/ > > Programming/ > > Research/ > > People/ > > Meetings/ > > Ideas/ > > Business/ > > Health/ > > Learning/ > > Books/ > > Articles/ > > Scratchpad/ > > Templates/ > > Archive/ > > Don't over-organize.
> Folders help.
> > Links help more.
> > 6.6 The Philosophy of Notes > Good notes answer questions.
> Bad notes collect information.
> > Instead of writing:
> > Docker > > Write:
> Why did we choose Docker?
> > Advantages > > Disadvantages > > Alternatives > > Lessons learned > > Useful commands > > Your future AI can reason much better from structured notes.
> 6.7 Linking Notes > One of Obsidian's superpowers is linking.
> Example:
> > [[Hermes]] > > [[OpenHands]] > > [[Qdrant]] > > [[Telegram Automation]] > > After a year you'll have a knowledge graph instead of isolated documents.
> Hermes can use these relationships to provide richer answers.
> > 6.8 Daily Notes > Create one note every day.
> Example:
> > 2026-07-03 > > Today's goals > > Finished:
> > Installed Hermes > > Problems:
> > Docker networking > > Ideas:
> > Multi-agent architecture > > Tomorrow:
> > Install Qdrant > > Hermes will eventually help generate these automatically.
> 6.9 Templates > Create reusable templates.
> Example:
> > Project template:
> > # Project > > ## Purpose > > ## Architecture > > ## Technologies > > ## TODO > > ## Decisions > > ## Problems > > ## References > > Meeting template:
> # Meeting > > Participants > > Agenda > > Notes > > Actions > > Follow-up > > Consistency improves retrieval.
> 6.10 Semantic Search > Folders aren't enough.
> Imagine asking:
> > "Find every note where I discussed vector databases."
> You don't remember whether it was called:
> Qdrant > Embeddings > AI Memory > Database Research > Semantic search solves this.
> Instead of matching words...
> > It matches meaning.
> > 6.11 Vector Embeddings > This is how semantic memory works.
> Markdown > > ↓ > > Chunking > > ↓ > > Embedding Model > > ↓ > > Vector Database > > ↓ > > Semantic Search > > ↓ > > Hermes > > Hermes asks:
> "What memories relate to this conversation?"
> The vector database answers.
> 6.12 Choosing a Vector Database > There are many excellent options:
> Database Notes > Qdrant Fast, simple, open source > Chroma Lightweight > Weaviate Powerful but larger > LanceDB Excellent for local development > pgvector Good if you already use PostgreSQL > > For this book we'll use Qdrant because it's reliable, actively maintained, > and integrates well with retrieval pipelines.
> 6.13 What Should Be Embedded?
> Not everything.
> Good candidates:
> > Notes > Documentation > Research > Project plans > Meeting summaries > Books > Code explanations > API documentation > Avoid embedding:
> Large binary files > Passwords > Private keys > Certificates > Encrypted secrets > Those belong in secure storage.
> 6.14 Automatic Memory Pipeline > Eventually the system will look like this:
> Conversation > > ↓ > > Summarize > > ↓ > > Extract useful knowledge > > ↓ > > Write Markdown > > ↓ > > Create embedding > > ↓ > > Store in Qdrant > > ↓ > > Future retrieval > > You don't manually decide what to remember.
> Hermes helps curate the knowledge base.
> > 6.15 Memory Retrieval > Suppose you ask:
> "Continue working on the Telegram bot."
> Hermes should retrieve:
> Project notes > Architecture decisions > Previous conversations > Outstanding TODOs > Related documentation > Before answering.
> That makes responses far more useful.
> > 6.16 Designing Good Memories > Not every conversation deserves permanent storage.
> A useful heuristic is:
> > Store:
> > Decisions > Preferences > Long-term goals > Completed research > Project status > Lessons learned > Don't store:
> Small talk > Temporary mistakes > Duplicate information > One-off questions > Quality beats quantity.
> 6.17 Git for Knowledge > Initialize your Obsidian vault as a Git repository.
> Benefits:
> > Version history > Easy backup > Rollback > Synchronization > Branching for experiments > Your knowledge becomes software.
> 6.18 Security > Your second brain will eventually contain:
> Business plans > Research > Personal notes > Internal documentation > Treat it like source code.
> Back it up.
> > Encrypt sensitive devices.
> > Control access carefully.
> > 6.19 Looking Ahead > Right now:
> Hermes has memory.
> > But it still can't do much.
> > It doesn't know how to:
> > Read GitHub repositories > Search your filesystem > Use Docker > Query databases > Browse the web > Manage email > Control Telegram > To unlock those capabilities, we need a standardized way for Hermes to > communicate with external tools.
> End of Chapter 6 > Congratulations.
> Your AI now has the foundations of a persistent memory system.
> > Instead of forgetting everything after each session, it can begin building > a durable, searchable knowledge base that grows alongside your work.
> > What's Next > Chapter 7 introduces Model Context Protocol (MCP)—the technology that > allows Hermes to securely interact with tools and services.
> We'll cover:
> > What MCP is and why it matters.
> Installing an MCP host.
> Connecting filesystem, Git, GitHub, Docker, and database tools.
> Permission models and security.
> Building your first custom MCP server.
> Once Hermes has both memory and tools, it becomes capable of taking > meaningful actions rather than simply answering questions. From that point > on, each chapter will expand its abilities until it resembles the always-on > AI operating system we envisioned at the beginning of the book.
> > > On Fri, Jul 3, 2026 at 4:10 AM Victoria K <vkushelnyk@gmail.com> wrote:
> >> The Personal AI Operating System >> Building a Production-Grade Hermes Agent with Memory, Automation, and >> Autonomous Coding >> Chapter 5 >> Installing Hermes — The Heart of Your AI Operating System >> "Today your server stops being a Linux machine and starts becoming an >> intelligent assistant."
>> This is one of the most important chapters in the book.
>> Everything we've done so far has been preparation. Now we're installing >> the component that will orchestrate your entire AI operating system.
>> >> By the end of this chapter, you will have:
>> >> A working Hermes installation >> A connected language model >> Your first successful conversation >> A production-ready workspace >> Docker sandboxing for tool execution >> A foundation ready for memory, MCP, automation, and coding >> The Hermes documentation recommends a simple progression: install Hermes, >> run the setup wizard, configure one model provider, verify a working >> terminal conversation, and only then begin adding integrations. We'll >> follow that approach before layering on the rest of the system.
>> H >> Hermes Agent Logo >> +1 >> 5.1 Understanding Hermes' Role >> Hermes is not the language model.
>> Hermes is the agent runtime.
>> >> Think of it like this:
>> >> You >> │ >> ▼ >> Hermes Agent >> │ >> ┌───────────┼────────────┐ >> ▼ ▼ ▼ >> LLM File System Tools >> │ >> ▼ >> Memory >> >> Hermes decides:
>> Which tools to use >> Which files to read >> Whether to search memory >> When to ask follow-up questions >> How to break large tasks into smaller ones >> The language model simply provides reasoning.
>> 5.2 Choosing Your LLM >> Hermes can connect to many providers through its setup process.
>> H >> Hermes AI >> +1 >> For this book I recommend starting with:
>> >> Option A (Recommended) >> OpenRouter >> Advantages:
>> >> Access to many models >> Easy to switch models >> Pay only for usage >> Good starting models:
>> Hermes >> Qwen >> DeepSeek >> Claude >> GPT >> Option B >> OpenAI >> Very reliable.
>> >> Excellent coding.
>> >> Easy setup.
>> >> Option C >> Local models using an OpenAI-compatible endpoint >> Perfect once you own a GPU server.
>> >> We'll cover this later.
>> >> 5.3 Install Hermes >> The Hermes project provides an installer for Linux environments. Before >> running any installer you should review the script, then execute it if >> you're comfortable with what it does. The current installation guide and >> quickstart both recommend this workflow.
>> H >> Hermes AI >> +1 >> After installation, verify:
>> >> hermes --version >> >> Expected:
>> Hermes Agent x.x.x >> >> 5.4 Initial Setup >> Run:
>> hermes setup >> >> Hermes will ask:
>> Which LLM provider?
>> API key >> Model name >> Tool configuration >> Hermes also includes a newer "Blank Slate" setup mode if you want to >> start with only the essentials (provider, terminal, and file tools) and add >> everything else later.
>> R >> Reddit >> +1 >> For this book:
>> >> Choose Blank Slate if available.
>> >> We'll build everything manually.
>> >> 5.5 Configure the Model >> Run:
>> hermes model >> >> You'll configure:
>> Provider >> >> ↓ >> >> API key >> >> ↓ >> >> Model >> >> ↓ >> >> Context length >> >> ↓ >> >> Connection test >> >> Hermes supports switching models later without rebuilding your system.
>> H >> Hermes AI >> >> 5.6 Verify Hermes >> Launch:
>> hermes >> >> Ask:
>> Hello Hermes.
>> You should receive a response.
>> Congratulations.
>> >> You now have a working AI agent.
>> >> 5.7 Configure the Workspace >> Create:
>> ~/ai-os/workspace >> >> This becomes Hermes' primary working directory.
>> Inside:
>> >> workspace/ >> >> projects/ >> >> scripts/ >> >> downloads/ >> >> notes/ >> >> scratch/ >> >> >> This keeps generated files separate from your infrastructure >> configuration.
>> 5.8 Sandboxed Tool Execution >> One of the first production decisions is where Hermes should execute >> commands.
>> Options include:
>> >> Directly on the host >> Inside Docker >> Over SSH to another machine >> For an always-on VPS, I recommend Docker sandboxing for routine terminal >> tasks. Hermes supports changing the terminal backend, including Docker.
>> H >> Hermes AI >> +1 >> Benefits:
>> >> Safer experimentation >> Easier cleanup >> Better isolation >> 5.9 Your First Real Task >> The Hermes quickstart recommends using a real task instead of a toy >> prompt.
>> H >> Hermes Agent Logo >> Examples:
>> >> Summarize this Git repository.
>> or >> Explain this Docker Compose stack.
>> or >> Review this Python script for bugs.
>> Avoid:
>> Tell me a joke.
>> Real tasks expose configuration issues much faster.
>> 5.10 Sessions >> Hermes stores conversations.
>> That means you can continue later.
>> >> Example:
>> >> hermes --continue >> >> This resumes the previous session instead of starting over.
>> H >> Hermes AI >> 5.11 Skills >> Hermes supports installable Skills.
>> Think of Skills as reusable expertise.
>> >> Examples:
>> >> Kubernetes >> React >> Docker >> Rust >> Git >> Terraform >> We'll install many of these later, after our core system is stable.
>> 5.12 Keep the Core Small >> Many people immediately install:
>> MCP >> Voice >> Telegram >> Discord >> Browser tools >> Twenty Skills >> Don't.
>> At this stage you only need:
>> >> One model >> Terminal access >> File operations >> A stable conversation >> Everything else comes later.
>> 5.13 Configure Your Project >> Inside:
>> ~/ai-os >> >> Create:
>> assistant/ >> >> knowledge/ >> >> memory/ >> >> prompts/ >> >> templates/ >> >> automation/ >> >> >> These directories will gradually become the brain of your AI operating >> system.
>> 5.14 Create Your First System Prompt >> Instead of relying on defaults, define the role you want Hermes to play.
>> For example:
>> >> Act as a software engineer.
>> Prefer safe actions over risky ones.
>> Ask before making irreversible changes.
>> Explain reasoning when proposing architectural changes.
>> Write maintainable, well-documented code.
>> Store reusable knowledge in the memory system when appropriate.
>> We'll refine this prompt throughout the book.
>> 5.15 What We Are Not Doing Yet >> We are intentionally delaying:
>> Obsidian >> Qdrant >> PostgreSQL >> GitHub >> Telegram >> Gmail >> Calendar >> Browser automation >> Voice >> Long-term memory >> The temptation is to connect everything immediately.
>> Resist it.
>> >> A stable foundation is far more valuable than a large but fragile system.
>> >> 5.16 Verify Your Installation >> Before continuing, confirm:
>> Hermes launches successfully.
>> Your chosen model responds.
>> Sessions can be resumed.
>> Hermes can access its workspace.
>> Tool execution works using your chosen backend.
>> Configuration files are backed up.
>> Only when all of these checks pass should you continue.
>> What's Next >> The next chapter is one of the biggest in the entire book.
>> We'll build long-term memory.
>> >> Specifically, we'll connect:
>> >> Obsidian >> Markdown >> Semantic search >> Qdrant >> Embeddings >> Retrieval pipelines >> By the end of Chapter 6, Hermes will no longer rely solely on the current >> conversation. Instead, it will begin developing a searchable, persistent >> knowledge base that grows with every project, note, and interaction. That's >> the point where it starts to feel less like a chatbot and more like a true >> personal AI operating system.
>> >> H >> H >> R >> Sources >> >> >> On Fri, Jul 3, 2026 at 4:09 AM Victoria K <vkushelnyk@gmail.com> wrote:
>> >>> The Personal AI Operating System >>> Building a Production-Grade Hermes Agent with Memory, Automation, and >>> Autonomous Coding >>> Chapter 4 >>> Creating the AI Platform >>> "Today we stop preparing and start building."
>>> Up to this point you've built a secure server and designed the >>> architecture.
>>> This chapter is where your VPS begins to resemble an actual AI platform.
>>> >>> By the end of this chapter you will have:
>>> >>> Docker Compose configured properly >>> Traefik running >>> Automatic HTTPS certificates >>> A shared Docker network >>> Automatic service discovery >>> A project layout that can scale to dozens of services >>> The first production containers online >>> Chapter Goal >>> By the end of this chapter, you'll have an infrastructure that looks >>> like this:
>>> Internet >>> │ >>> ▼ >>> Cloudflare / DNS >>> │ >>> ▼ >>> HTTPS (443) >>> │ >>> ▼ >>> Traefik Reverse Proxy >>> │ >>> ┌──────────┼──────────┐ >>> │ │ │ >>> ▼ ▼ ▼ >>> Hermes n8n OpenHands >>> │ >>> ▼ >>> Internal Docker Network >>> │ >>> ┌────┴─────────────┐ >>> ▼ ▼ >>> PostgreSQL Qdrant >>> >>> Only Traefik is exposed to the internet.
>>> Everything else communicates privately.
>>> >>> This dramatically improves security.
>>> >>> 4.1 Why Docker Compose?
>>> You could manually launch containers one by one.
>>> That quickly becomes impossible to manage.
>>> >>> Instead, every service in this book will have:
>>> >>> docker-compose.yml >>> .env >>> README.md >>> >>> Starting a service becomes:
>>> docker compose up -d >>> >>> Stopping:
>>> docker compose down >>> >>> Updating:
>>> docker compose pull >>> docker compose up -d >>> >>> That's it.
>>> 4.2 Installing Docker Compose (if needed) >>> Most modern Docker installations already include the Compose plugin.
>>> Verify:
>>> >>> docker compose version >>> >>> Expected output:
>>> Docker Compose version v2.x.x >>> >>> If it works, you're ready.
>>> 4.3 Create the Shared Network >>> Create a permanent Docker network.
>>> docker network create ai-network >>> >>> Verify:
>>> docker network ls >>> >>> You should see:
>>> ai-network >>> bridge >>> host >>> none >>> >>> Every future service joins this network.
>>> 4.4 Create the Infrastructure Directory >>> Inside:
>>> ~/ai-os >>> >>> Create:
>>> compose/ >>> >>> Then:
>>> compose/ >>> >>> traefik/ >>> >>> postgres/ >>> >>> redis/ >>> >>> qdrant/ >>> >>> hermes/ >>> >>> openhands/ >>> >>> n8n/ >>> >>> grafana/ >>> >>> prometheus/ >>> >>> This organization keeps each service isolated.
>>> 4.5 Installing Traefik >>> Traefik becomes the front door to your AI operating system.
>>> Responsibilities:
>>> >>> HTTPS >>> SSL certificates >>> Routing >>> Load balancing >>> Automatic container discovery >>> Unlike Nginx, Traefik automatically discovers Docker containers.
>>> Launch a new service...
>>> >>> Traefik immediately knows about it.
>>> >>> 4.6 Create Traefik Configuration >>> Create:
>>> compose/traefik/ >>> >>> Inside:
>>> docker-compose.yml >>> >>> Traefik will:
>>> expose ports 80 and 443 >>> mount the Docker socket (read-only) >>> store Let's Encrypt certificates >>> join ai-network >>> restart automatically >>> watch Docker for new services >>> We'll keep all routing rules in Docker labels rather than large >>> configuration files.
>>> 4.7 Automatic HTTPS >>> One of Traefik's biggest advantages is automatic certificate management.
>>> Instead of manually creating certificates, Traefik will:
>>> >>> Detect a new hostname.
>>> Request a certificate from Let's Encrypt.
>>> Renew it automatically before expiration.
>>> Redirect HTTP traffic to HTTPS.
>>> Once configured, every new service can receive HTTPS with minimal >>> additional configuration.
>>> 4.8 Prepare DNS >>> At your DNS provider, create records such as:
>>> Type Name Target >>> A ai VPS IP >>> A hermes VPS IP >>> A n8n VPS IP >>> A grafana VPS IP >>> A api VPS IP >>> >>> If you use Cloudflare, leave the proxy enabled unless a specific service >>> requires direct access.
>>> 4.9 Docker Volumes >>> Containers are temporary.
>>> Data isn't.
>>> >>> Create persistent storage:
>>> >>> data/ >>> >>> postgres/ >>> >>> redis/ >>> >>> qdrant/ >>> >>> traefik/ >>> >>> grafana/ >>> >>> prometheus/ >>> >>> hermes/ >>> >>> Every important service writes here.
>>> Deleting a container never deletes your data.
>>> >>> 4.10 Central Environment File >>> Inside:
>>> ~/ai-os >>> >>> Create:
>>> .env >>> >>> Eventually it will contain:
>>> DOMAIN=example.com >>> >>> EMAIL=you@example.com >>> >>> TZ=America/Edmonton >>> >>> POSTGRES_USER=aiadmin >>> >>> POSTGRES_PASSWORD=...
>>> >>> OPENAI_API_KEY=...
>>> >>> TELEGRAM_TOKEN=...
>>> >>> GITHUB_TOKEN=...
>>> >>> OBSIDIAN_PATH=/home/aiadmin/ai-os/obsidian >>> >>> Never commit this file to Git.
>>> 4.11 Create Your First Service >>> Instead of deploying Hermes immediately, start with a simple container >>> to verify your infrastructure.
>>> A lightweight web server is enough to confirm:
>>> >>> Docker networking works.
>>> Traefik routes correctly.
>>> HTTPS certificates are issued.
>>> DNS points to the correct machine.
>>> Once this works, every future service follows the same pattern.
>>> 4.12 Service Labels >>> One of Traefik's strengths is that services describe themselves.
>>> A typical service advertises:
>>> >>> its hostname (for example, hermes.example.com) >>> which internal port it listens on >>> whether HTTPS should be enabled >>> Traefik reads this metadata automatically and creates the routing rules >>> without you editing a central configuration file.
>>> 4.13 Logging >>> Create:
>>> logs/ >>> >>> traefik/ >>> >>> docker/ >>> >>> system/ >>> >>> >>> Logs are invaluable when diagnosing:
>>> certificate failures >>> DNS issues >>> container crashes >>> routing problems >>> Rotate logs regularly so they don't consume excessive disk space.
>>> 4.14 Health Checks >>> Every service should expose a health endpoint where possible.
>>> Docker can periodically test whether a service is responsive.
>>> >>> If a health check fails repeatedly, Docker can restart the container >>> automatically.
>>> >>> This is one of the easiest ways to improve long-term reliability.
>>> >>> 4.15 Verify the Platform >>> At this stage, you should be able to confirm:
>>> Docker is running.
>>> The ai-network network exists.
>>> Traefik starts without errors.
>>> DNS records resolve correctly.
>>> HTTPS certificates are issued.
>>> A test service is reachable through your chosen hostname.
>>> Once those checks pass, you've built a reusable platform for every >>> component that follows.
>>> 4.16 The Philosophy Behind the Stack >>> Notice what we've not done:
>>> Installed Hermes.
>>> Installed databases.
>>> Installed automation tools.
>>> That was intentional.
>>> Infrastructure should come first.
>>> >>> Once the platform is solid, adding a new service becomes routine instead >>> of risky.
>>> >>> What's Coming Next >>> Chapter 5 is where your AI operating system becomes genuinely >>> intelligent.
>>> We'll install and configure Hermes Agent itself, including:
>>> >>> Choosing a language model backend (cloud or local).
>>> Connecting Hermes to your infrastructure.
>>> Configuring its tool system.
>>> Setting up its initial memory.
>>> Understanding its architecture.
>>> Running your first successful conversations.
>>> From there, we'll begin expanding Hermes with persistent memory, coding >>> capabilities, automation, and integrations until it becomes the central >>> orchestrator of your personal AI operating system.
>>> A note before we continue >>> As we move into Hermes, we'll shift from high-level planning to hands-on >>> implementation. I'll include configuration examples where they're stable, >>> but I won't invent or freeze commands for projects that change rapidly.
>>> Where a project's installation process evolves frequently, we'll follow the >>> current official documentation and adapt it to fit the architecture we've >>> built, rather than copying outdated snippets.
>>> That approach makes the book more durable and gives you a system that's >>> easier to maintain as the AI ecosystem evolves.
>>> >>> >>> On Fri, Jul 3, 2026 at 4:08 AM Victoria K <vkushelnyk@gmail.com> wrote:
>>> >>>> The Personal AI Operating System >>>> Building a Production-Grade Hermes Agent with Memory, Automation, and >>>> Autonomous Coding >>>> Chapter 3 >>>> Building the Production Platform >>>> "A good server runs software. A great server runs services reliably."
>>>> In the previous chapter, we built a secure Linux server. Now we're >>>> going to transform it into a platform capable of running dozens of AI >>>> services that work together.
>>>> By the end of this chapter, you'll have:
>>>> >>>> A production Docker environment >>>> Automatic HTTPS >>>> A reverse proxy >>>> Internal Docker networking >>>> DNS configured >>>> Centralized environment variables >>>> A scalable directory layout >>>> Monitoring foundations >>>> Automatic container restarts >>>> After this chapter, every future component (Hermes, Qdrant, PostgreSQL, >>>> n8n, OpenHands, Grafana, etc.) will simply plug into the platform.
>>>> 3.1 Thinking Like an Infrastructure Engineer >>>> A common mistake is running every application independently:
>>>> docker run postgres >>>> docker run qdrant >>>> docker run hermes >>>> docker run redis >>>> docker run nginx >>>> >>>> That quickly becomes difficult to manage.
>>>> Instead, we'll organize services using Docker Compose.
>>>> >>>> Every service will have:
>>>> >>>> its own configuration >>>> persistent storage >>>> internal networking >>>> restart policy >>>> health checks >>>> backups >>>> 3.2 The Final Architecture >>>> By the end of the book your VPS will look like this:
>>>> Internet >>>> │ >>>> Cloudflare DNS >>>> │ >>>> Reverse Proxy >>>> (Traefik) >>>> │ >>>> ┌─────────────────┼──────────────────┐ >>>> │ │ │ >>>> ▼ ▼ ▼ >>>> Hermes Agent n8n OpenHands >>>> │ >>>> ▼ >>>> MCP Servers >>>> │ >>>> ┌────┴─────────────┐ >>>> ▼ ▼ >>>> Qdrant PostgreSQL >>>> │ >>>> ▼ >>>> Obsidian Vault >>>> >>>> >>>> Every service communicates internally.
>>>> Only Traefik is exposed to the public Internet.
>>>> >>>> 3.3 Why Traefik?
>>>> Many tutorials recommend Nginx.
>>>> Nginx is excellent.
>>>> >>>> However, Traefik is designed for Docker.
>>>> >>>> Advantages:
>>>> >>>> Automatic HTTPS >>>> Automatic service discovery >>>> Automatic SSL renewal >>>> Docker integration >>>> Simple labels >>>> Easy scaling >>>> Whenever we launch a new container, Traefik immediately discovers it.
>>>> No configuration files need updating.
>>>> >>>> 3.4 Domain Configuration >>>> Suppose your domain is:
>>>> example.com >>>> >>>> Create these DNS records:
>>>> Host Points To >>>> ai VPS IP >>>> api VPS IP >>>> n8n VPS IP >>>> qdrant VPS IP >>>> grafana VPS IP >>>> hermes VPS IP >>>> >>>> If using Cloudflare:
>>>> Enable:
>>>> >>>> Proxy >>>> Automatic HTTPS >>>> DNSSEC (optional) >>>> 3.5 Directory Structure >>>> Expand the folders we created.
>>>> ai-os/ >>>> >>>> compose/ >>>> >>>> configs/ >>>> >>>> logs/ >>>> >>>> data/ >>>> >>>> services/ >>>> >>>> scripts/ >>>> >>>> monitoring/ >>>> >>>> backups/ >>>> >>>> obsidian/ >>>> >>>> projects/ >>>> >>>> >>>> Inside compose:
>>>> compose/ >>>> >>>> traefik/ >>>> >>>> postgres/ >>>> >>>> qdrant/ >>>> >>>> hermes/ >>>> >>>> n8n/ >>>> >>>> openhands/ >>>> >>>> grafana/ >>>> >>>> prometheus/ >>>> >>>> redis/ >>>> >>>> >>>> Every service receives:
>>>> docker-compose.yml >>>> >>>> .env >>>> >>>> README.md >>>> >>>> >>>> Keeping services isolated makes upgrades much easier.
>>>> 3.6 Docker Networks >>>> We'll create two Docker networks.
>>>> public >>>> >>>> Used only by:
>>>> Traefik >>>> Everything else stays on:
>>>> internal >>>> >>>> This means your database is never directly accessible from the Internet.
>>>> This is one of the biggest security improvements you can make.
>>>> >>>> 3.7 Persistent Storage >>>> Containers should be disposable.
>>>> Data should not.
>>>> >>>> Every service stores data outside the container.
>>>> >>>> Example:
>>>> >>>> data/ >>>> >>>> postgres/ >>>> >>>> qdrant/ >>>> >>>> redis/ >>>> >>>> grafana/ >>>> >>>> hermes/ >>>> >>>> >>>> If a container crashes:
>>>> Delete it.
>>>> >>>> Recreate it.
>>>> >>>> Your data survives.
>>>> >>>> 3.8 Environment Variables >>>> Never hardcode secrets.
>>>> Instead create:
>>>> >>>> .env >>>> >>>> Example:
>>>> POSTGRES_USER=aiadmin >>>> >>>> POSTGRES_PASSWORD=...
>>>> >>>> OPENAI_API_KEY=...
>>>> >>>> TELEGRAM_TOKEN=...
>>>> >>>> GITHUB_TOKEN=...
>>>> >>>> >>>> Docker automatically loads these values.
>>>> 3.9 Automatic Restarts >>>> Every container should include:
>>>> restart: unless-stopped >>>> >>>> If the VPS reboots:
>>>> Everything starts automatically.
>>>> >>>> 3.10 Health Checks >>>> A container running isn't necessarily healthy.
>>>> Each service should expose a health endpoint.
>>>> >>>> Docker periodically asks:
>>>> >>>> Are you alive?
>>>> >>>> If not:
>>>> Restart.
>>>> >>>> This prevents many subtle failures.
>>>> >>>> 3.11 Logging >>>> Instead of every container writing logs everywhere, create:
>>>> logs/ >>>> >>>> traefik/ >>>> >>>> hermes/ >>>> >>>> n8n/ >>>> >>>> postgres/ >>>> >>>> >>>> Centralized logs make debugging much easier.
>>>> 3.12 Monitoring >>>> Eventually we'll install:
>>>> Prometheus >>>> Grafana >>>> cAdvisor >>>> Node Exporter >>>> These provide:
>>>> CPU >>>> >>>> RAM >>>> >>>> Disk >>>> >>>> Network >>>> >>>> Container health >>>> >>>> Temperature (where supported) >>>> >>>> Database performance >>>> >>>> API latency >>>> >>>> LLM usage >>>> >>>> You'll know immediately if something fails.
>>>> >>>> 3.13 Backup Strategy >>>> Never trust a single disk.
>>>> Daily:
>>>> >>>> Database dump >>>> >>>> Weekly:
>>>> >>>> Obsidian backup >>>> >>>> Monthly:
>>>> >>>> Entire Docker volumes >>>> >>>> Also back up:
>>>> >>>> configs/ >>>> >>>> compose/ >>>> >>>> scripts/ >>>> >>>> >>>> These files are often more valuable than the containers themselves.
>>>> 3.14 GitHub Repository >>>> Create a private repository.
>>>> Example:
>>>> >>>> AI-Operating-System >>>> >>>> Commit:
>>>> compose/ >>>> >>>> scripts/ >>>> >>>> configs/ >>>> >>>> docs/ >>>> >>>> >>>> Never commit:
>>>> .env >>>> >>>> keys >>>> >>>> certificates >>>> >>>> tokens >>>> >>>> database dumps >>>> >>>> >>>> 3.15 Documentation >>>> Create:
>>>> docs/ >>>> >>>> Document everything.
>>>> Example:
>>>> >>>> Server Architecture.md >>>> >>>> Network Diagram.md >>>> >>>> Secrets.md >>>> >>>> Deployment.md >>>> >>>> Troubleshooting.md >>>> >>>> >>>> Future you will thank present you.
>>>> 3.16 Planning the Services >>>> Here's what we'll install over the next several chapters.
>>>> Service Purpose >>>> Traefik Reverse proxy >>>> PostgreSQL Main relational database >>>> Redis Cache and message broker >>>> Qdrant Vector database >>>> Hermes Agent AI orchestrator >>>> OpenHands Coding agent >>>> n8n Workflow automation >>>> Grafana Dashboards >>>> Prometheus Metrics >>>> Obsidian Sync Long-term knowledge >>>> MCP Servers Tool integrations >>>> Playwright Browser automation >>>> >>>> Each will run in its own container.
>>>> Each can be updated independently.
>>>> >>>> 3.17 Security Philosophy >>>> Everything inside the server should follow least privilege.
>>>> Examples:
>>>> >>>> Hermes should not have root access.
>>>> >>>> OpenHands should not control Docker directly.
>>>> >>>> The browser automation service should run in an isolated container.
>>>> >>>> Databases should not be exposed publicly.
>>>> >>>> Treat every service as potentially vulnerable.
>>>> >>>> Isolation limits the damage if something goes wrong.
>>>> >>>> 3.18 Resource Planning >>>> Estimated idle memory usage:
>>>> Service RAM >>>> Traefik 80 MB >>>> PostgreSQL 300 MB >>>> Redis 100 MB >>>> Qdrant 500 MB >>>> Hermes Variable >>>> n8n 400 MB >>>> OpenHands 1–2 GB >>>> Grafana 250 MB >>>> Prometheus 400 MB >>>> >>>> A 16 GB VPS is enough to get started, though 32 GB gives you more room >>>> as you add services.
>>>> 3.19 Your Infrastructure Is Ready >>>> At this point you've designed a platform that can grow from five >>>> containers to fifty without needing to be rebuilt.
>>>> That scalability is intentional.
>>>> >>>> As new AI tools appear, you'll add them as independent services instead >>>> of tearing apart the existing system.
>>>> >>>> End of Chapter 3 >>>> Your server is no longer "just a VPS." It's a production platform ready >>>> to host a complete AI operating system.
>>>> What's Coming Next >>>> Chapter 4 is where the real fun begins.
>>>> We'll deploy:
>>>> >>>> Traefik >>>> Automatic HTTPS with Let's Encrypt >>>> Docker Compose stacks >>>> Internal Docker networking >>>> Your first production services >>>> By the end of Chapter 4, you'll be able to visit >>>> https://ai.yourdomain.com (or a similar subdomain you choose) and see >>>> your own infrastructure responding over HTTPS. That marks the transition >>>> from preparing the platform to actually deploying the services that will >>>> power your personal AI operating system.
>>>> A note on keeping this book current >>>> One thing I'd like to improve over a traditional printed book is to >>>> keep the build aligned with current best practices. AI infrastructure >>>> changes quickly—projects evolve, APIs change, and new tools emerge every >>>> few months.
>>>> As we continue, I'll favor:
>>>> >>>> Official documentation over outdated community examples.
>>>> Stable, actively maintained components.
>>>> Configurations that are practical in production rather than just enough >>>> to get something running.
>>>> Explanations of why we're making a choice, so you can substitute >>>> components later if the ecosystem changes.
>>>> That way, the end result isn't just a snapshot of today's tools—it's an >>>> architecture that can adapt over time.
>>>> >>>> On Fri, Jul 3, 2026 at 4:07 AM Victoria K <vkushelnyk@gmail.com> wrote:
>>>> >>>>> Chapter 2 >>>>> >>>>> The Personal AI Operating System >>>>> Building a Production-Grade Hermes Agent with Memory, Automation, and >>>>> Autonomous Coding >>>>> Chapter 2 >>>>> Building the Foundation – Your VPS, Domain, and Secure Linux Server >>>>> "A house is only as strong as its foundation."
>>>>> Before we install a single AI component, we need infrastructure that >>>>> is reliable, secure, and easy to maintain.
>>>>> Many AI tutorials jump straight into running a model. That works for >>>>> experiments, but if you're building something that will manage your emails, >>>>> code repositories, documents, and personal knowledge, you want an >>>>> environment you can trust.
>>>>> >>>>> By the end of this chapter, you'll have:
>>>>> >>>>> A VPS running Ubuntu Server LTS >>>>> SSH key authentication >>>>> A hardened Linux server >>>>> A configured firewall >>>>> Automatic security updates >>>>> Docker prerequisites installed >>>>> A registered domain name pointing at your server >>>>> A clean project structure ready for the AI operating system >>>>> 2.1 Choosing a VPS Provider >>>>> You do not need a GPU server to begin. Hermes can use cloud-hosted >>>>> models while all of your automation, memory, and tooling run on your VPS.
>>>>> Look for these minimum specifications:
>>>>> >>>>> Component Recommended >>>>> CPU 4–8 vCPUs >>>>> RAM 16 GB minimum (32 GB preferred) >>>>> Storage 200 GB NVMe SSD >>>>> OS Ubuntu Server 24.04 LTS >>>>> IPv4 Dedicated static address >>>>> Backups Daily snapshots >>>>> >>>>> Good providers include:
>>>>> DigitalOcean >>>>> Hetzner >>>>> Vultr >>>>> Linode >>>>> OVHcloud >>>>> For this book, we'll assume Ubuntu Server 24.04 LTS.
>>>>> 2.2 Registering a Domain >>>>> You'll eventually expose services like:
>>>>> ai.yourdomain.com >>>>> >>>>> n8n.yourdomain.com >>>>> >>>>> api.yourdomain.com >>>>> >>>>> grafana.yourdomain.com >>>>> >>>>> vault.yourdomain.com >>>>> >>>>> Keeping services on subdomains makes SSL certificates and reverse >>>>> proxy configuration much easier later.
>>>>> 2.3 Creating the VPS >>>>> After creating your VPS:
>>>>> Choose:
>>>>> >>>>> Ubuntu Server 24.04 LTS >>>>> >>>>> Region:
>>>>> >>>>> Choose one geographically close to you.
>>>>> >>>>> Disk:
>>>>> >>>>> At least 200 GB.
>>>>> >>>>> Enable:
>>>>> >>>>> Daily backups >>>>> Monitoring >>>>> IPv4 >>>>> IPv6 (optional) >>>>> 2.4 Generating SSH Keys >>>>> Never log in using passwords.
>>>>> On Linux/macOS:
>>>>> >>>>> ssh-keygen -t ed25519 -C "your@email.com"
>>>>> >>>>> Windows PowerShell:
>>>>> ssh-keygen >>>>> >>>>> This creates:
>>>>> ~/.ssh/id_ed25519 >>>>> >>>>> and >>>>> ~/.ssh/id_ed25519.pub >>>>> >>>>> Upload the public key to your VPS provider during creation.
>>>>> 2.5 First Login >>>>> Connect:
>>>>> ssh root@YOUR_SERVER_IP >>>>> >>>>> If successful:
>>>>> Welcome to Ubuntu 24.04 LTS >>>>> >>>>> Congratulations.
>>>>> You now own a Linux server.
>>>>> >>>>> 2.6 Update Everything >>>>> Immediately update packages.
>>>>> apt update >>>>> >>>>> apt upgrade -y >>>>> >>>>> Install useful utilities:
>>>>> apt install -y \ >>>>> git \ >>>>> curl \ >>>>> wget \ >>>>> htop \ >>>>> vim \ >>>>> nano \ >>>>> unzip \ >>>>> zip \ >>>>> build-essential \ >>>>> ca-certificates >>>>> >>>>> 2.7 Create a Non-Root User >>>>> Never work as root.
>>>>> Create a new user:
>>>>> >>>>> adduser aiadmin >>>>> >>>>> Add to sudo:
>>>>> usermod -aG sudo aiadmin >>>>> >>>>> Copy SSH keys:
>>>>> rsync --archive --chown=aiadmin:aiadmin ~/.ssh /home/aiadmin >>>>> >>>>> Log out.
>>>>> Reconnect:
>>>>> >>>>> ssh aiadmin@YOUR_SERVER_IP >>>>> >>>>> 2.8 Disable Root Login >>>>> Edit:
>>>>> sudo nano /etc/ssh/sshd_config >>>>> >>>>> Set:
>>>>> PermitRootLogin no >>>>> PasswordAuthentication no >>>>> PubkeyAuthentication yes >>>>> >>>>> Restart SSH:
>>>>> sudo systemctl restart ssh >>>>> >>>>> Now only SSH keys work.
>>>>> 2.9 Configure the Firewall >>>>> Ubuntu includes UFW.
>>>>> Enable:
>>>>> >>>>> sudo ufw allow OpenSSH >>>>> >>>>> Later we'll open:
>>>>> 80 >>>>> 443 >>>>> >>>>> Enable:
>>>>> sudo ufw enable >>>>> >>>>> Check:
>>>>> sudo ufw status >>>>> >>>>> Expected:
>>>>> 22 ALLOW >>>>> >>>>> 2.10 Automatic Security Updates >>>>> Install:
>>>>> sudo apt install unattended-upgrades >>>>> >>>>> Enable:
>>>>> sudo dpkg-reconfigure unattended-upgrades >>>>> >>>>> Now security patches install automatically.
>>>>> 2.11 Set Timezone >>>>> sudo timedatectl set-timezone America/Edmonton >>>>> >>>>> Verify:
>>>>> timedatectl >>>>> >>>>> Accurate time matters for logs, certificates, and scheduled workflows.
>>>>> 2.12 Install Fail2Ban >>>>> Fail2Ban helps protect against repeated login attempts.
>>>>> sudo apt install fail2ban >>>>> >>>>> Enable:
>>>>> sudo systemctl enable fail2ban >>>>> sudo systemctl start fail2ban >>>>> >>>>> Check status:
>>>>> sudo systemctl status fail2ban >>>>> >>>>> 2.13 Install Docker >>>>> Remove old versions if present:
>>>>> sudo apt remove docker docker-engine docker.io containerd runc >>>>> >>>>> Install prerequisites:
>>>>> sudo apt install \ >>>>> ca-certificates \ >>>>> curl \ >>>>> gnupg >>>>> >>>>> Create the keyring directory:
>>>>> sudo install -m 0755 -d /etc/apt/keyrings >>>>> >>>>> Download Docker's GPG key:
>>>>> curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \ >>>>> sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg >>>>> >>>>> Add the Docker repository:
>>>>> echo \ >>>>> "deb [arch=$(dpkg --print-architecture) >>>>> signed-by=/etc/apt/keyrings/docker.gpg] \ >>>>> https://download.docker.com/linux/ubuntu \ >>>>> $(. /etc/os-release && echo $VERSION_CODENAME) stable" | \ >>>>> sudo tee /etc/apt/sources.list.d/docker.list >>>>> >>>>> Update:
>>>>> sudo apt update >>>>> >>>>> Install Docker:
>>>>> sudo apt install docker-ce docker-ce-cli containerd.io >>>>> docker-buildx-plugin docker-compose-plugin >>>>> >>>>> Verify:
>>>>> docker --version >>>>> >>>>> 2.14 Allow Docker Without sudo >>>>> sudo usermod -aG docker $USER >>>>> >>>>> Log out.
>>>>> Log back in.
>>>>> >>>>> Verify:
>>>>> >>>>> docker ps >>>>> >>>>> No errors means success.
>>>>> 2.15 Create Your Project Structure >>>>> Under your home directory:
>>>>> ~/ai-os/ >>>>> >>>>> Inside:
>>>>> ai-os/ >>>>> │ >>>>> ├── docker/ >>>>> ├── configs/ >>>>> ├── backups/ >>>>> ├── logs/ >>>>> ├── scripts/ >>>>> ├── data/ >>>>> ├── compose/ >>>>> ├── secrets/ >>>>> ├── docs/ >>>>> ├── projects/ >>>>> ├── obsidian/ >>>>> └── monitoring/ >>>>> >>>>> Create it:
>>>>> mkdir -p >>>>> ~/ai-os/{docker,configs,backups,logs,scripts,data,compose,secrets,docs,projects,obsidian,monitoring} >>>>> >>>>> 2.16 Initialize Git >>>>> cd ~/ai-os >>>>> >>>>> git init >>>>> >>>>> Create:
>>>>> .gitignore >>>>> >>>>> Example:
>>>>> .env >>>>> *.key >>>>> *.pem >>>>> secrets/ >>>>> >>>>> Commit:
>>>>> git add .
>>>>> git commit -m "Initial AI operating system structure"
>>>>> >>>>> 2.17 Plan for Secrets >>>>> Never hard-code:
>>>>> API keys >>>>> Passwords >>>>> OAuth tokens >>>>> SSH keys >>>>> We'll use .env files for local development and migrate to a proper >>>>> secrets manager later.
>>>>> 2.18 Verify the Foundation >>>>> Before moving on, confirm:
>>>>> SSH keys work >>>>> Root login is disabled >>>>> Firewall is active >>>>> Automatic updates are enabled >>>>> Fail2Ban is running >>>>> Docker is installed >>>>> Docker Compose is available >>>>> Git is initialized >>>>> Project directories exist >>>>> At this point, you have a secure, clean Linux server that can host the >>>>> rest of your AI operating system.
>>>>> What's Next?
>>>>> In Chapter 3, we'll transform this server into a production platform >>>>> by:
>>>>> Installing a reverse proxy >>>>> Obtaining free HTTPS certificates >>>>> Configuring DNS >>>>> Setting up Docker Compose for multiple services >>>>> Creating an internal Docker network >>>>> Adding health checks and restart policies >>>>> Preparing the infrastructure that every future component—Hermes, >>>>> Qdrant, PostgreSQL, n8n, and more—will use >>>>> By the end of the next chapter, you'll have the production-grade >>>>> hosting environment that the rest of the AI operating system will run on.
>>>>> From there, we'll begin installing the core AI components themselves.
>>>>> >>>>> >>>>> On Fri, Jul 3, 2026 at 4:07 AM Victoria K <vkushelnyk@gmail.com> >>>>> wrote:
>>>>> >>>>>> Chapter 1 >>>>>> >>>>>> I'd be happy to. I also think we should do it properly instead of >>>>>> rushing through it.
>>>>>> We'll write it as if it were a professional technical book that you >>>>>> can actually build from. Every chapter will be detailed enough that you can >>>>>> complete it before moving on to the next. By the end, you'll have a >>>>>> production-ready AI operating system running on your own infrastructure.
>>>>>> >>>>>> The Personal AI Operating System >>>>>> Building a Production-Grade Hermes Agent with Memory, Automation, and >>>>>> Autonomous Coding >>>>>> Author: ChatGPT (OpenAI) & You >>>>>> Chapter 1 >>>>>> Designing Your Personal AI Operating System >>>>>> "Before writing a single command, we need to decide what we're >>>>>> building."
>>>>>> One of the biggest mistakes people make is starting by installing >>>>>> software.
>>>>>> They install Docker.
>>>>>> >>>>>> Then Ollama.
>>>>>> >>>>>> Then Obsidian.
>>>>>> >>>>>> Then a vector database.
>>>>>> >>>>>> Then some random GitHub repository.
>>>>>> >>>>>> After a week they have twenty containers running that barely >>>>>> communicate with one another.
>>>>>> >>>>>> Eventually they give up.
>>>>>> >>>>>> We're not going to do that.
>>>>>> >>>>>> Instead, we're going to design the system first.
>>>>>> >>>>>> What We're Actually Building >>>>>> This is not a chatbot.
>>>>>> It is not an AI assistant.
>>>>>> >>>>>> It is not another wrapper around a language model.
>>>>>> >>>>>> We're building a personal AI operating system.
>>>>>> >>>>>> Think of it as hiring an extremely capable junior engineer, executive >>>>>> assistant, researcher, and knowledge manager who works 24 hours a day.
>>>>>> >>>>>> The AI should become an extension of your mind.
>>>>>> >>>>>> It should remember what you're building.
>>>>>> >>>>>> It should know your current projects.
>>>>>> >>>>>> It should know your coding style.
>>>>>> >>>>>> It should remember conversations from months ago.
>>>>>> >>>>>> It should help finish software.
>>>>>> >>>>>> It should organize your life.
>>>>>> >>>>>> It should automate repetitive work.
>>>>>> >>>>>> Most importantly, it should continue improving alongside you.
>>>>>> >>>>>> Design Philosophy >>>>>> Every major design decision in this book follows six principles.
>>>>>> Principle 1: Modularity >>>>>> Every component should be replaceable.
>>>>>> If a better language model appears next month, you should swap it in >>>>>> without rebuilding everything else.
>>>>>> >>>>>> If you decide to replace your vector database, nothing else should >>>>>> break.
>>>>>> >>>>>> Loose coupling keeps the system maintainable.
>>>>>> >>>>>> Principle 2: Local Ownership >>>>>> Your knowledge belongs to you.
>>>>>> Your notes.
>>>>>> >>>>>> Your projects.
>>>>>> >>>>>> Your memories.
>>>>>> >>>>>> Your documents.
>>>>>> >>>>>> Whenever practical, store them in open formats like Markdown and Git >>>>>> repositories rather than proprietary databases.
>>>>>> >>>>>> Principle 3: Human Approval for High-Risk Actions >>>>>> The system can:
>>>>>> classify emails >>>>>> summarize documents >>>>>> draft replies >>>>>> write code >>>>>> organize notes >>>>>> But before it:
>>>>>> sends an email >>>>>> deletes files >>>>>> pushes to production >>>>>> spends money >>>>>> grants permissions >>>>>> …it should ask for approval until you intentionally relax those >>>>>> safeguards.
>>>>>> Principle 4: Long-Term Memory >>>>>> Most AI assistants forget everything after the conversation ends.
>>>>>> Our system won't.
>>>>>> >>>>>> Every useful interaction becomes structured knowledge.
>>>>>> >>>>>> Over time, the AI develops a searchable memory of your work.
>>>>>> >>>>>> Principle 5: Automation Without Chaos >>>>>> Automation is only useful if it's predictable.
>>>>>> Every automated workflow should be:
>>>>>> >>>>>> observable >>>>>> logged >>>>>> reversible >>>>>> testable >>>>>> If something goes wrong, you should be able to understand why.
>>>>>> Principle 6: Open Standards >>>>>> Where possible, we'll use:
>>>>>> Docker >>>>>> Git >>>>>> Markdown >>>>>> Model Context Protocol (MCP) >>>>>> REST APIs >>>>>> PostgreSQL >>>>>> Open-source components >>>>>> This makes the system easier to migrate and extend.
>>>>>> The High-Level Architecture >>>>>> Imagine the system as five layers.
>>>>>> YOU >>>>>> │ >>>>>> ┌───────────┴───────────┐ >>>>>> │ Voice • Chat • Web UI │ >>>>>> └───────────┬───────────┘ >>>>>> │ >>>>>> Hermes Agent >>>>>> │ >>>>>> ┌───────────────┼────────────────┐ >>>>>> │ │ │ >>>>>> ▼ ▼ ▼ >>>>>> Memory Tool Layer Reasoning >>>>>> (Obsidian + (MCP Servers) (LLM) >>>>>> Qdrant) >>>>>> │ │ >>>>>> └───────────────┘ >>>>>> │ >>>>>> Your Digital Life >>>>>> >>>>>> Each layer has a specific responsibility.
>>>>>> Layer 1 – The User Interface >>>>>> This is how you interact with the system.
>>>>>> Initially we'll use:
>>>>>> >>>>>> a web interface >>>>>> a terminal >>>>>> Telegram >>>>>> Later we'll add:
>>>>>> voice >>>>>> desktop notifications >>>>>> mobile access >>>>>> The interface is intentionally simple because intelligence belongs in >>>>>> the backend.
>>>>>> Layer 2 – Hermes Agent >>>>>> Hermes acts as the orchestrator.
>>>>>> It doesn't just answer questions.
>>>>>> >>>>>> It decides:
>>>>>> >>>>>> which tools to use >>>>>> when to retrieve memory >>>>>> whether to search documentation >>>>>> whether code needs to be executed >>>>>> when to ask for clarification >>>>>> Think of Hermes as the operating system's scheduler.
>>>>>> Layer 3 – Memory >>>>>> Memory is split into two parts.
>>>>>> Human-readable memory >>>>>> >>>>>