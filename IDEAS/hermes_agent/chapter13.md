---
created: 2026-07-03
type: reference
tags: [book, hermes, chapter]
chapter: 13
title: "Multi-Agent Intelligence — Turning Hermes into a Coordinated Team"
source: victoria-k-emails
---

# Chapter 13 — Multi-Agent Intelligence — Turning Hermes into a Coordinated Team

Multi-Agent Intelligence — Turning Hermes into a Coordinated Team “A single agent can think. A system of agents can operate.” Up to this point, Hermes is effectively a single, highly capable agent with:
memory tools (MCP) automation browser access coding ability observability But there’s a ceiling to single-agent design:
context gets overloaded tasks compete for attention specialization is limited long workflows become fragile This chapter breaks that limitation.
We evolve Hermes into a multi-agent system.

13.1 Why Multi-Agent Systems Exist
Real work is not one skill.
It’s a combination of roles:

researcher engineer reviewer planner operator debugger A single agent switching between all roles becomes inconsistent.
So instead we split intelligence into specialized workers.

13.2 The New Architecture
You │ Orchestrator │ ┌──────────┬────────┬──────────┐ ▼ ▼ ▼ ▼ Planner Researcher Coder Operator ▼ ▼ ▼ ▼ Shared Memory + MCP Tools

At the center is the Orchestrator Agent (main Hermes instance).
It delegates tasks to sub-agents.

13.3 The Orchestrator Role
The orchestrator:
breaks down tasks assigns work to agents merges results resolves conflicts ensures consistency decides execution order It does NOT do all the work itself.
It coordinates.

13.4 Sub-Agent Specializations
We define core roles:
1. Planner Agent
Responsibilities:
task decomposition architecture design workflow planning dependency mapping
2. Research Agent
Responsibilities:
web browsing documentation extraction comparison analysis knowledge synthesis
3. Coding Agent
Responsibilities:
repository modification debugging test execution patch generation
4. Operator Agent
Responsibilities:
MCP tool execution system commands file operations automation triggers
13.5 Shared Memory Model
All agents share:
Obsidian vault Qdrant vector DB project workspace logs system But they do NOT overwrite each other blindly.
They communicate through structured memory objects.

13.6 Task Delegation Flow
User request ↓ Orchestrator analyzes ↓ Task broken into subtasks ↓ Agents assigned ↓ Parallel execution ↓ Results merged ↓ Final output

This is how complex work scales.
13.7 Example: Build a Telegram Bot
Request:
“Build a Telegram bot that summarizes emails.” Execution:
Planner:
defines architecture identifies dependencies creates workflow Researcher:
looks up Telegram API patterns finds best libraries Coder:
implements bot writes handlers integrates email MCP Operator:
deploys service configures environment starts container Orchestrator:
merges everything validates output
13.8 Parallel Execution Model
Agents can run simultaneously:
Research ─┐ ├──► Merge ───► Output Coding ──┤ Operator ─┘

This reduces task completion time significantly.
13.9 Conflict Resolution
Sometimes agents disagree.
Example:

Research agent recommends Library A Coding agent prefers Library B The orchestrator decides based on:
reliability compatibility simplicity maintainability This prevents chaos.
13.10 Agent Communication Protocol
Agents communicate using structured messages:
{ "task": "implement_email_parser", "status": "in_progress", "dependencies": ["gmail_mcp"], "output": null, "confidence": 0.87 }

No free-form chaos.
Everything structured.

13.11 Resource Management
We enforce limits:
CPU per agent memory per task max concurrent agents execution timeout This prevents runaway systems.
13.12 Hierarchical Planning System
We introduce planning depth:
Level 0: Goal Level 1: Major tasks Level 2: Subtasks Level 3: Execution steps

Each layer can be delegated.
13.13 Shared State vs Isolation
Two memory types:
Shared Memory project context decisions final outputs Isolated Memory temporary reasoning intermediate steps failed experiments This keeps system clean.
13.14 Agent Lifecycle
Each agent:
receives task initializes context executes logs output terminates No persistent uncontrolled agents.
13.15 Debugging Multi-Agent Systems
We track:
which agent made which decision full execution chain tool usage per agent failure attribution Example:
“Bug introduced by coding agent during API refactor” Not:
“system error”
13.16 Scaling Behavior
As tasks increase:
more agents spawn (within limits) tasks are distributed bottlenecks are isolated But we always cap complexity to avoid fragmentation.
13.17 First Multi-Agent Test
Prompt:
“Analyze my project, fix bugs, and improve structure.” Expected flow:
Planner maps architecture Researcher checks best practices Coder fixes issues Operator runs tests Orchestrator merges results Memory updated
13.18 Why This Chapter Matters
Before:
one agent doing everything After:
coordinated intelligence system This is the shift from:
“assistant” to “AI organization” End of Chapter 13 You now have:
specialized agents orchestration layer structured communication parallel execution conflict resolution shared memory system What’s Next Chapter 14 is the final transformation layer:
We will build:

autonomous goal system (long-term objectives) self-improving workflows feedback loops performance optimization cycles system evolution rules governance and safety constraints This is where Hermes stops being a tool… and becomes a self-directed operational intelligence system under your supervision.

On Fri, Jul 3, 2026 at 4:14 AM Victoria K <vkushelnyk@gmail.com> wrote:

> The Personal AI Operating System > Building a Production-Grade Hermes Agent with Memory, Automation, and > Autonomous Coding > Chapter 12 > The Control Center — Observability, Dashboards, and System Awareness > “A system you cannot observe is a system you cannot trust.” > At this point, Hermes is already doing a lot:
> managing communication (email, Telegram) > scheduling events > using tools via MCP > browsing the web > running automation workflows > writing and modifying code in sandboxes > storing long-term memory > But there’s a problem:
> 👉 You don’t have a unified way to see what’s happening.
> > This chapter fixes that by building your AI Operating System Control > Center.
> > 12.1 Why Observability Is Essential > As systems grow, three things always happen:
> you forget what the system did > debugging becomes guesswork > automation becomes “black box behavior” > We solve this with observability layers:
> logs > metrics > traces > dashboards > decision history > 12.2 The Control Center Architecture > Hermes Agent > │ > ┌───────────────┼────────────────┐ > ▼ ▼ ▼ > Logs Metrics Decision Memory > │ │ │ > └───────────────┼────────────────┘ > ▼ > Control Dashboard > │ > ▼ > You (Operator) > > Everything Hermes does becomes visible.
> Nothing is hidden.
> > 12.3 What We’re Building > Your control center will include:
> system health dashboard > workflow execution history > tool usage tracking > memory exploration UI > active tasks monitor > error tracking panel > cost tracking (LLM + APIs) > agent decision timeline > 12.4 Choosing the Dashboard Stack > We will use a combination:
> Grafana > metrics visualization > system monitoring > time-series data > Prometheus > metrics collection > system performance tracking > Custom Web Dashboard > Hermes activity feed > decision logs > tool call history > memory explorer > 12.5 Installing Prometheus > Inside your stack:
> ~/ai-os/compose/prometheus/ > > Prometheus will collect:
> CPU usage > RAM usage > disk I/O > container health > network activity > It becomes your system pulse monitor.
> 12.6 Installing Grafana > Grafana sits on top of Prometheus:
> It provides:
> > graphs > dashboards > alerts > historical trends > Example dashboards:
> “AI system load over time” > “Hermes tool usage frequency” > “Automation success rate” > 12.7 Logging System Design > We unify logs into a single structure:
> ~/ai-os/logs/ > > system/ > tools/ > mcp/ > workflows/ > coding/ > browser/ > memory/ > errors/ > > Every subsystem writes here.
> No exceptions.
> > 12.8 Decision Logging (Critical Feature) > We introduce a new concept:
> “Why did Hermes do that?” > Every meaningful action stores:
> input > reasoning > selected tool > result > confidence score > Example:
> Action: Send email reply > > Reason:
> User requested response drafting for client inquiry.
> > Tools used:
> Gmail MCP > > Decision:
> High confidence > > Outcome:
> Draft created, awaiting approval > > This makes the system auditable.
> 12.9 Hermes Activity Feed > We build a real-time stream:
> email processed > workflow started > tool executed > memory updated > error occurred > Think of it like:
> “system live feed for your AI brain” > 12.10 Memory Explorer > We expose your Obsidian + Qdrant memory visually:
> You can:
> > search past decisions > trace project evolution > view knowledge clusters > inspect embeddings > explore related notes > This turns memory into something navigable, not invisible.
> 12.11 Tool Usage Analytics > We track:
> Metric Purpose > tool frequency understand usage patterns > success rate reliability > latency performance > failure rate debugging > cost per tool optimization > > Example insight:
> “Browser automation is responsible for 40% of system errors.” > 12.12 Error Tracking System > All errors are:
> captured > categorized > linked to workflows > stored for analysis > Example categories:
> MCP failure > API timeout > authentication error > browser crash > model hallucination > 12.13 Real-Time System Health Panel > You will be able to see:
> CPU usage > memory load > disk usage > active containers > running workflows > queued tasks > This ensures transparency.
> 12.14 Alert System > If something goes wrong:
> Hermes triggers alerts:
> > Telegram:
> > “Workflow failure in email processing pipeline.” > Dashboard:
> red status indicator > error logs attached > retry option available > 12.15 Control vs Autonomy Balance > We introduce a key principle:
> “The more autonomous the system becomes, the more visible it must be.” > So we enforce:
> Autonomy Level Observability Requirement > low basic logs > medium structured logs + metrics > high full decision trace > critical real-time dashboard + approval > > 12.16 Cost Tracking (Important in Real Systems) > We track:
> LLM token usage > API calls > browser automation time > storage growth > This prevents silent cost escalation.
> 12.17 First Integrated Dashboard > After setup, you’ll have:
> Grafana (system metrics) > Prometheus (data source) > custom Hermes dashboard > log explorer > memory viewer > All accessible from:
> https://ai.yourdomain.com/dashboard > > 12.18 Why This Chapter Matters > Before this chapter:
> system was powerful but opaque > After this chapter:
> system becomes observable and controllable > This is the difference between:
> “a collection of AI tools” > and > “an AI operating system” > End of Chapter 12 > You now have full visibility into your AI system:
> what it is doing > why it is doing it > how it is performing > what it has learned > What’s Next > Chapter 13 is where we upgrade from “system visibility” to system > intelligence management.
> We will build:
> > multi-agent coordination (Hermes + sub-agents) > task delegation system > specialization roles (researcher, coder, operator) > load balancing between agents > conflict resolution between decisions > hierarchical planning system > This is where Hermes stops being a single agent… > and becomes a team of coordinated AI workers under your control.
> > > On Fri, Jul 3, 2026 at 4:14 AM Victoria K <vkushelnyk@gmail.com> wrote:
> >> The Personal AI Operating System >> Building a Production-Grade Hermes Agent with Memory, Automation, and >> Autonomous Coding >> Chapter 11 >> Autonomous Coding — Turning Hermes into a Software Engineer >> “The moment an AI can reliably modify code, it stops being a tool and >> starts becoming a collaborator.” >> Up to this point, Hermes can:
>> think and reason >> remember context >> use tools (MCP) >> interact with your real systems >> browse the web >> run scheduled automation >> manage communication channels >> But there’s still one major gap:
>> 👉 It cannot yet own and evolve software projects safely and >> systematically.
>> >> This chapter changes that.
>> >> We build a controlled autonomous coding environment.
>> >> 11.1 What “Autonomous Coding” Actually Means >> We are NOT building:
>> a bot that randomly edits files >> a system that pushes unreviewed code to production >> a chaotic self-modifying agent >> We ARE building:
>> A structured software engineering assistant that can:
>> understand repositories >> propose changes >> run tests >> debug failures >> iterate safely >> submit reviewable changes >> 11.2 The Architecture Shift >> We introduce a dedicated layer:
>> Hermes Agent >> │ >> MCP Tool Layer >> │ >> ┌────────────┼────────────┐ >> ▼ ▼ ▼ >> Browser Email Calendar >> │ >> ▼ >> Coding Runtime (NEW) >> │ >> ▼ >> Git Repositories + Docker Sandbox >> >> This is your software engineering subsystem.
>> 11.3 Why We Use a Coding Runtime >> Direct filesystem editing is dangerous.
>> Instead, we isolate coding inside:
>> >> containers >> cloned repositories >> test environments >> So Hermes never directly touches production state.
>> It works like this:
>> >> Clone repo >> ↓ >> Work in sandbox >> ↓ >> Run tests >> ↓ >> Iterate >> ↓ >> Create patch >> ↓ >> Request approval >> ↓ >> Apply to main repo >> >> 11.4 Introducing OpenHands-Style Workflow >> We replicate the idea of an autonomous coding agent loop:
>> The loop:
>> Understand codebase >> Identify task >> Plan changes >> Modify code >> Run tests >> Fix errors >> Repeat >> Produce final diff >> This loop is the foundation of modern coding agents.
>> 11.5 Repository Workspace Structure >> Inside your AI OS:
>> ~/ai-os/projects/ >> >> Each repo becomes:
>> project-name/ >> repo/ >> sandbox/ >> logs/ >> tests/ >> notes/ >> patches/ >> >> Hermes never works directly on live repos.
>> Only sandbox copies.
>> >> 11.6 Git Integration Layer >> We extend MCP with Git capabilities:
>> Hermes can:
>> >> clone repositories >> create branches >> commit changes >> generate diffs >> revert commits >> open pull requests (with approval) >> But always inside controlled workflows.
>> 11.7 Code Understanding Phase >> Before making changes, Hermes must:
>> read project structure >> identify entry points >> understand dependencies >> map architecture >> detect test framework >> This is critical.
>> Bad agents skip understanding.
>> >> Good agents analyze first.
>> >> 11.8 Planning Layer (Very Important) >> No code changes happen immediately.
>> Instead:
>> >> User request >> ↓ >> Analysis >> ↓ >> Plan generated >> ↓ >> User approval (optional but recommended) >> ↓ >> Execution >> >> Example plan:
>> “Fix authentication bug in login module” >> Plan:
>> locate auth handler >> identify failure condition >> add validation check >> update unit tests >> run test suite >> 11.9 Execution Sandbox >> All code execution happens in Docker:
>> isolated filesystem >> controlled network access >> limited permissions >> reproducible environment >> This prevents:
>> system corruption >> accidental file deletion >> security issues >> dependency conflicts >> 11.10 Test-Driven Loop >> Hermes should always:
>> Run tests >> Observe failure >> Modify code >> Retry >> This loop continues until:
>> tests pass >> or max iteration limit reached >> 11.11 Patch-Based Output >> Instead of rewriting files blindly, Hermes outputs:
>> diffs >> patches >> commit messages >> Example:
>> Fix: resolve null pointer in auth module >> >> - added null check before token validation >> - improved error handling >> - updated unit test coverage >> >> This makes review easy.
>> 11.12 Safety Boundaries >> We enforce strict rules:
>> Hermes must NOT:
>> >> modify system files >> access secrets directly >> execute production scripts >> push without approval >> delete repositories >> Allowed:
>> sandbox edits >> test execution >> patch generation >> proposal creation >> 11.13 Multi-Step Debugging Loop >> When errors occur:
>> Run code >> ↓ >> Capture error >> ↓ >> Analyze stack trace >> ↓ >> Hypothesize cause >> ↓ >> Apply fix >> ↓ >> Retry >> >> This is how real engineers work.
>> 11.14 Logging Development Sessions >> Every coding session is stored:
>> logs/ >> coding/ >> session-id/ >> plan.md >> changes.diff >> tests.log >> decisions.md >> >> This becomes part of your AI memory system.
>> 11.15 First Real Autonomous Task >> Try something like:
>> “Add input validation to my API server.” >> Expected flow:
>> Hermes clones repo >> analyzes API structure >> identifies endpoints >> adds validation logic >> runs tests >> fixes errors >> outputs patch >> requests approval >> 11.16 Why This Is a Major Milestone >> Before this chapter:
>> Hermes could assist coding >> After this chapter:
>> Hermes can participate in software development >> This is the difference between:
>> “code helper” >> and >> “junior engineer inside your system” >> 11.17 What We Are NOT Doing Yet >> We still avoid:
>> autonomous production deployments >> self-writing large systems without review >> uncontrolled multi-repo changes >> continuous self-improvement loops >> AI modifying its own core runtime >> Those require governance layers we will build later.
>> 11.18 System Behavior Shift >> Your AI OS now has:
>> Capability Status >> reasoning ✔ >> memory ✔ >> tools ✔ >> communication ✔ >> automation ✔ >> web access ✔ >> coding ✔ >> autonomous iteration ✔ >> >> End of Chapter 11 >> Your system can now:
>> understand codebases >> modify software safely >> test changes automatically >> iterate like a developer >> What’s Next >> Chapter 12 is where everything starts converging into a unified system.
>> We will build:
>> >> a unified dashboard for Hermes >> real-time system observability >> centralized logs and traces >> decision history tracking >> tool usage analytics >> memory visualization >> workflow control panel >> This is where your AI OS stops being a collection of services… >> and becomes a single coherent system you can observe and control.
>> >> On Fri, Jul 3, 2026 at 4:13 AM Victoria K <vkushelnyk@gmail.com> wrote:
>> >>> The Personal AI Operating System >>> Building a Production-Grade Hermes Agent with Memory, Automation, and >>> Autonomous Coding >>> Chapter 10 >>> Making It Run on Its Own — Automation, Scheduling, and Background Agents >>> “An assistant that only responds is useful. An assistant that acts on >>> its own schedule becomes infrastructure.” >>> Up to this point, Hermes is powerful—but still fundamentally reactive:
>>> You ask → it responds >>> You trigger → it acts >>> That’s not an operating system yet.
>>> An operating system has processes running in the background, even when >>> you’re not actively using it.
>>> >>> This chapter introduces that layer.
>>> >>> 10.1 What We Are Building Now >>> We are adding autonomous background execution:
>>> Hermes Agent >>> │ >>> Automation Scheduler >>> │ >>> ┌───────────────┼────────────────┐ >>> ▼ ▼ ▼ >>> Cron Jobs Event Triggers Workflow Engine >>> ▼ ▼ ▼ >>> Reports Monitoring Actions (MCP tools) >>> >>> Hermes will now:
>>> run tasks on a schedule >>> monitor systems continuously >>> generate reports automatically >>> react to external events >>> trigger workflows without being asked >>> 10.2 Two Types of Automation >>> We separate automation into two categories:
>>> 1. Scheduled Automation (Predictable) >>> Runs at fixed intervals:
>>> every hour >>> daily >>> weekly >>> monthly >>> Examples:
>>> daily email summary >>> weekly project report >>> system health checks >>> 2. Event-Based Automation (Reactive) >>> Runs when something happens:
>>> email received >>> message sent on Telegram >>> file updated >>> API event triggered >>> Examples:
>>> new email → classify + draft reply >>> GitHub PR → review code >>> server error → notify Telegram >>> 10.3 Introducing n8n (Workflow Engine) >>> We use:
>>> n8n >>> Why:
>>> visual workflow builder >>> supports webhooks >>> supports cron schedules >>> integrates with APIs easily >>> self-hostable on VPS >>> works well with MCP tools >>> 10.4 Installing n8n >>> Inside your server:
>>> mkdir -p ~/ai-os/compose/n8n >>> cd ~/ai-os/compose/n8n >>> >>> Create Docker Compose file:
>>> version: "3.9"
>>> >>> services:
>>> n8n:
>>> image: n8nio/n8n:latest >>> restart: unless-stopped >>> ports:
>>> - "5678:5678"
>>> environment:
>>> - TZ=America/Edmonton >>> - N8N_BASIC_AUTH_ACTIVE=true >>> - N8N_BASIC_AUTH_USER=admin >>> - N8N_BASIC_AUTH_PASSWORD=change_this >>> volumes:
>>> - ../../data/n8n:/home/node/.n8n >>> networks:
>>> - ai-network >>> >>> networks:
>>> ai-network:
>>> external: true >>> >>> Start it:
>>> docker compose up -d >>> >>> Access:
>>> http://YOUR_VPS_IP:5678 >>> >>> 10.5 First Workflow: Daily System Report >>> We build your first automation.
>>> Goal:
>>> Every morning Hermes generates a report:
>>> system health >>> inbox summary >>> calendar overview >>> active projects >>> pending tasks >>> Workflow Steps:
>>> Cron trigger (8:00 AM) >>> Call Hermes MCP tool:
>>> gather system data >>> Summarize results >>> Store in Obsidian >>> Send Telegram message >>> 10.6 Cron Scheduling Concept >>> n8n uses cron expressions:
>>> 0 8 * * *
>>> >>> Meaning:
>>> Every day at 08:00 >>> We will use this pattern constantly.
>>> 10.7 Event-Driven Automation >>> Now we connect real-time triggers.
>>> Example:
>>> >>> Gmail → n8n → Hermes >>> Flow:
>>> New email arrives >>> ↓ >>> Webhook trigger >>> ↓ >>> n8n workflow starts >>> ↓ >>> Hermes classifies email >>> ↓ >>> Stores in memory >>> ↓ >>> Optional: draft reply >>> ↓ >>> Telegram notification >>> >>> 10.8 Telegram as Control Panel >>> Telegram becomes your:
>>> approval system >>> notification system >>> emergency interface >>> remote control panel >>> Example:
>>> Hermes:
>>> "New email requires response.
>>> Draft:
>>> 'Thanks, I will review this today.' >>> Approve?"
>>> >>> [Approve] [Edit] [Reject] >>> >>> 10.9 Background Agent Pattern >>> We define a key concept:
>>> “Always-on workers” >>> Instead of one big AI, we run multiple small processes:
>>> Worker Purpose >>> Email Worker inbox processing >>> Calendar Worker scheduling >>> System Worker monitoring >>> Research Worker web ingestion >>> Memory Worker summarization >>> >>> Each runs independently.
>>> 10.10 System Monitoring Agent >>> We add a continuous system observer:
>>> Every 5–10 minutes:
>>> >>> CPU usage >>> RAM usage >>> disk space >>> container health >>> failed services >>> If something breaks:
>>> → Telegram alert >>> >>> 10.11 Memory Automation Pipeline >>> We now automate memory creation:
>>> New conversation or event >>> ↓ >>> Summarize >>> ↓ >>> Extract key insights >>> ↓ >>> Write Markdown note >>> ↓ >>> Embed into Qdrant >>> ↓ >>> Store in Obsidian >>> >>> No manual note-taking required.
>>> 10.12 Scheduled Research Agent >>> Once per day:
>>> Hermes will:
>>> >>> browse selected topics >>> update knowledge base >>> track changes in tools >>> summarize updates >>> Example:
>>> “What changed in AI agent frameworks today?” >>> 10.13 Workflow Reliability Design >>> Every workflow must include:
>>> retry logic >>> timeout limits >>> failure notifications >>> logging >>> We never assume automation always works.
>>> We design for failure.
>>> >>> 10.14 Avoiding Automation Chaos >>> A critical rule:
>>> Never let multiple workflows modify the same system state without >>> coordination.
>>> We avoid:
>>> duplicate email replies >>> conflicting calendar events >>> repeated API calls >>> memory duplication >>> We introduce:
>>> locking >>> deduplication >>> idempotent operations >>> 10.15 Logging Everything >>> We expand logging:
>>> logs/ >>> workflows/ >>> cron/ >>> triggers/ >>> failures/ >>> >>> Each entry includes:
>>> trigger source >>> execution steps >>> outcome >>> duration >>> errors (if any) >>> 10.16 First End-to-End Autonomous System >>> After this chapter, your system can:
>>> Every morning:
>>> >>> check system health >>> summarize inbox >>> review calendar >>> update memory >>> send you a report >>> Without you asking.
>>> This is the first true autonomous layer of your AI OS.
>>> >>> 10.17 What We Are NOT Doing Yet >>> We still avoid:
>>> full autonomous email sending without approval >>> autonomous purchases >>> uncontrolled API usage >>> long-running web scraping loops >>> multi-agent self-replication >>> Those come later with stricter governance.
>>> 10.18 Why This Chapter Matters >>> Before this chapter:
>>> Hermes was reactive >>> After this chapter:
>>> Hermes becomes proactive >>> That shift is fundamental.
>>> You are no longer “using an AI.” >>> >>> You are now running an AI system in the background of your life.
>>> >>> End of Chapter 10 >>> Your AI operating system now has:
>>> reasoning (Hermes) >>> memory (Obsidian + Qdrant) >>> tools (MCP) >>> real-world integration (email, Telegram, calendar) >>> web access (Playwright) >>> and now automation (n8n + scheduled agents) >>> What’s Next >>> Chapter 11 is where we take a major leap:
>>> We will build OpenHands-style autonomous coding capability:
>>> >>> repository understanding >>> automated debugging >>> code generation pipelines >>> test execution loops >>> pull request creation >>> self-healing software workflows >>> This is where Hermes stops just assisting development… >>> and starts becoming a software engineer inside your system.
>>> >>> On Fri, Jul 3, 2026 at 4:12 AM Victoria K <vkushelnyk@gmail.com> wrote:
>>> >>>> The Personal AI Operating System >>>> Building a Production-Grade Hermes Agent with Memory, Automation, and >>>> Autonomous Coding >>>> Chapter 9 >>>> Giving Hermes Eyes — Browser Automation and Web Intelligence >>>> “An agent that cannot see the web is operating blind in a world that >>>> lives online.” >>>> At this point, Hermes can:
>>>> Read and write files >>>> Use MCP tools >>>> Manage email >>>> Handle Telegram >>>> Work with your calendar >>>> Store and retrieve memory >>>> But there is still a major gap.
>>>> It cannot reliably interact with the web itself.
>>>> >>>> This chapter fixes that by giving Hermes controlled browser access >>>> through automation.
>>>> >>>> 9.1 Why Browser Access Matters >>>> Almost everything you do lives on the web:
>>>> Research >>>> Documentation >>>> SaaS tools >>>> Dashboards >>>> GitHub browsing >>>> APIs >>>> Admin panels >>>> Without browser access, Hermes must rely on:
>>>> APIs (limited) >>>> manual copy/paste >>>> or your instructions >>>> With browser automation, Hermes can:
>>>> navigate websites >>>> extract structured data >>>> fill forms >>>> download files >>>> compare information across sources >>>> perform research workflows >>>> 9.2 The Architecture Upgrade >>>> We add a new tool layer:
>>>> Hermes Agent >>>> │ >>>> MCP Tool Layer >>>> │ >>>> ┌───────────────┼────────────────┐ >>>> ▼ ▼ ▼ >>>> Email Telegram Calendar >>>> │ >>>> ▼ >>>> Browser Automation (NEW) >>>> │ >>>> ▼ >>>> Web / SaaS / APIs / Dashboards >>>> >>>> 9.3 Choosing the Browser Engine >>>> We use:
>>>> Playwright >>>> Why:
>>>> modern automation framework >>>> supports Chromium, Firefox, WebKit >>>> stable headless mode >>>> strong selector system >>>> excellent for scraping and workflows >>>> 9.4 Installing Playwright on the VPS >>>> Inside your VPS:
>>>> sudo apt update >>>> sudo apt install -y nodejs npm >>>> >>>> Install Playwright:
>>>> npm init -y >>>> npm install playwright >>>> >>>> Install browsers:
>>>> npx playwright install >>>> >>>> Verify:
>>>> npx playwright test >>>> >>>> 9.5 Creating the Browser MCP Server >>>> We wrap Playwright in an MCP server.
>>>> Location:
>>>> >>>> ~/ai-os/services/mcp/browser/ >>>> >>>> Capabilities:
>>>> open page >>>> click element >>>> type text >>>> extract content >>>> screenshot page >>>> download file >>>> run structured scraping tasks >>>> 9.6 Browser Security Model >>>> This is extremely important.
>>>> Hermes must NEVER have:
>>>> >>>> unrestricted browser access to your accounts >>>> ability to bypass CAPTCHAs >>>> access to password managers >>>> uncontrolled session persistence >>>> We enforce:
>>>> Rule Behavior >>>> No saved credentials required login prompts >>>> No hidden browsing all actions logged >>>> Session isolation per-task browser instance >>>> Screenshot logging every step traceable >>>> Time limits prevents infinite loops >>>> >>>> 9.7 Browser Sandboxing >>>> Each task runs in a fresh containerized session:
>>>> Task starts >>>> ↓ >>>> New browser instance >>>> ↓ >>>> Execute steps >>>> ↓ >>>> Capture logs + screenshots >>>> ↓ >>>> Destroy session >>>> >>>> This prevents memory leaks and credential leakage.
>>>> 9.8 First Capability: Web Navigation >>>> Hermes can now:
>>>> “Go to GitHub and find repositories about AI memory systems.” >>>> Flow:
>>>> Open browser >>>> Navigate to GitHub >>>> Search query >>>> Filter results >>>> Extract top repositories >>>> Return structured summary >>>> 9.9 Structured Web Extraction >>>> Instead of dumping raw HTML, we convert results into structured data:
>>>> Title >>>> URL >>>> Description >>>> Stars >>>> Last updated >>>> Relevance score >>>> >>>> This is critical for combining browser data with memory systems later.
>>>> 9.10 Research Agent Workflow >>>> We define a standard research pipeline:
>>>> User question >>>> ↓ >>>> Plan search strategy >>>> ↓ >>>> Open multiple pages >>>> ↓ >>>> Extract structured data >>>> ↓ >>>> Cross-check sources >>>> ↓ >>>> Summarize findings >>>> ↓ >>>> Store in memory >>>> >>>> 9.11 Example Use Case: Competitor Research >>>> Prompt:
>>>> “Compare vector databases for AI agents.” >>>> Hermes will:
>>>> search documentation sites >>>> open product pages >>>> extract features >>>> compare pricing >>>> summarize tradeoffs >>>> store results in Obsidian >>>> 9.12 Screenshot-Based Verification >>>> Hermes can capture screenshots at each step:
>>>> Step 1: homepage.png >>>> Step 2: search_results.png >>>> Step 3: details_page.png >>>> >>>> This makes debugging deterministic.
>>>> 9.13 Browser + MCP Integration >>>> We expose Playwright through MCP:
>>>> Capabilities exposed:
>>>> >>>> navigate(url) >>>> click(selector) >>>> type(selector, text) >>>> extract(selector) >>>> screenshot() >>>> evaluate(script) >>>> Hermes calls these as tools.
>>>> 9.14 Rate Limiting and Safety >>>> To prevent abuse or runaway automation:
>>>> limit requests per minute >>>> enforce max page visits per task >>>> require confirmation for login actions >>>> block sensitive domains if needed >>>> 9.15 Combining Browser + Memory >>>> This is where things become powerful.
>>>> Example flow:
>>>> >>>> Hermes researches topic >>>> Extracts structured data >>>> Saves to Obsidian >>>> Embeds into Qdrant >>>> Future queries use that knowledge instantly >>>> The web becomes a live ingestion pipeline for your brain system.
>>>> 9.16 First Full Test >>>> Try:
>>>> “Find the best frameworks for building AI agents in 2026 and summarize >>>> them.” >>>> Expected:
>>>> browser opens multiple sources >>>> extracts frameworks >>>> compares capabilities >>>> stores summary in memory >>>> returns structured answer >>>> 9.17 What We Are NOT Doing Yet >>>> We are NOT yet enabling:
>>>> autonomous login to sensitive accounts >>>> scraping behind paywalls >>>> CAPTCHA solving >>>> continuous background browsing >>>> financial transactions >>>> These require stricter governance layers we will build later.
>>>> 9.18 Why This Chapter Is Important >>>> Before this chapter:
>>>> Hermes knew your system >>>> Hermes knew your memory >>>> Hermes knew your communication tools >>>> After this chapter:
>>>> 👉 Hermes can learn from the entire internet >>>> >>>> This is what transforms it from:
>>>> >>>> assistant >>>> into:
>>>> research + automation intelligence layer >>>> End of Chapter 9 >>>> You now have a system that can:
>>>> think >>>> remember >>>> communicate >>>> schedule >>>> and explore the web >>>> That combination is extremely powerful.
>>>> What’s Next >>>> Chapter 10 is where we move into true automation engineering.
>>>> We will build:
>>>> >>>> scheduled workflows (cron + n8n) >>>> background agents >>>> recurring research tasks >>>> automated reporting systems >>>> self-updating memory pipelines >>>> This is where Hermes stops being reactive… >>>> …and starts running processes continuously in the background like a >>>> real operating system.
>>>> >>>> On Fri, Jul 3, 2026 at 4:12 AM Victoria K <vkushelnyk@gmail.com> wrote:
>>>> >>>>> The Personal AI Operating System >>>>> Building a Production-Grade Hermes Agent with Memory, Automation, and >>>>> Autonomous Coding >>>>> Chapter 8 >>>>> Connecting Your Life — Email, Telegram, and Calendar Integration >>>>> “An AI agent becomes powerful when it stops living in isolation.” >>>>> At this point, Hermes can:
>>>>> Reason >>>>> Use tools via MCP >>>>> Read and write files >>>>> Work inside your coding environment >>>>> But it still cannot interact with your actual day-to-day life.
>>>>> This chapter changes that.
>>>>> >>>>> We will connect Hermes to:
>>>>> >>>>> Email (Gmail API) >>>>> Telegram (real-time messaging) >>>>> Google Calendar (time management) >>>>> This is the moment your system stops being “a developer tool” and >>>>> starts becoming a personal operations assistant.
>>>>> 8.1 The New Architecture Layer >>>>> We are adding a new layer to the system:
>>>>> Hermes Agent >>>>> │ >>>>> MCP Tool Layer >>>>> │ >>>>> ┌─────────────┼─────────────┐ >>>>> ▼ ▼ ▼ >>>>> Email Telegram Calendar >>>>> (Gmail API) (Bot API) (Google API) >>>>> >>>>> Each system becomes:
>>>>> a tool >>>>> exposed through MCP >>>>> controlled with permissions >>>>> logged for auditability >>>>> 8.2 Design Principle: Inbox ≠ Autonomy >>>>> We will NOT allow full autonomy yet.
>>>>> Instead, we follow this rule:
>>>>> >>>>> Action Behavior >>>>> Read email Allowed >>>>> Summarize email Allowed >>>>> Draft reply Allowed >>>>> Send email Requires approval >>>>> Delete email Requires explicit approval >>>>> Schedule event Suggest + confirm >>>>> >>>>> This prevents accidental actions from becoming irreversible problems.
>>>>> 8.3 Gmail Integration (Email Intelligence Layer) >>>>> Step 1 — Google Cloud Setup >>>>> Go to:
>>>>> Google Cloud Console >>>>> Create a new project >>>>> Enable Gmail API >>>>> Create credentials:
>>>>> OAuth client ID >>>>> Desktop or Web application >>>>> Download:
>>>>> credentials.json >>>>> >>>>> Store securely:
>>>>> ~/ai-os/secrets/gmail/ >>>>> >>>>> Step 2 — MCP Email Tool >>>>> We wrap Gmail inside an MCP server.
>>>>> Capabilities:
>>>>> >>>>> list inbox >>>>> read emails >>>>> classify emails >>>>> draft responses >>>>> label emails >>>>> search history >>>>> We explicitly do NOT allow silent sending.
>>>>> Step 3 — Email Workflow Logic >>>>> Hermes will process email like this:
>>>>> New email arrives >>>>> ↓ >>>>> Classify intent >>>>> ↓ >>>>> ┌──────────────┬──────────────┐ >>>>> ▼ ▼ ▼ >>>>> Spam Informational Action Required >>>>> ▼ ▼ ▼ >>>>> Archive Summarize Draft reply >>>>> ↓ >>>>> Request approval >>>>> >>>>> Step 4 — Email Memory Integration >>>>> Important emails are:
>>>>> summarized >>>>> stored in Obsidian >>>>> embedded into Qdrant >>>>> This allows Hermes to later answer:
>>>>> “What did that client say last month?” >>>>> 8.4 Telegram Integration (Real-Time Interface) >>>>> Telegram becomes your live control channel.
>>>>> You can:
>>>>> >>>>> send commands >>>>> receive alerts >>>>> approve actions >>>>> interact with Hermes remotely >>>>> Step 1 — Create Bot >>>>> In Telegram:
>>>>> Open BotFather >>>>> Create bot >>>>> Save API token >>>>> Store:
>>>>> ~/ai-os/secrets/telegram/token.env >>>>> >>>>> Step 2 — MCP Telegram Server >>>>> Capabilities:
>>>>> receive messages >>>>> send responses >>>>> send alerts >>>>> request approvals >>>>> forward summaries >>>>> Step 3 — Notification System >>>>> Hermes uses Telegram for:
>>>>> urgent emails >>>>> failed workflows >>>>> scheduled reminders >>>>> system alerts >>>>> “approval required” requests >>>>> Example:
>>>>> Hermes:
>>>>> "I found 3 emails requiring responses.
>>>>> Draft replies ready.
>>>>> Approve sending?"
>>>>> [Approve] [Edit] [Reject] >>>>> >>>>> 8.5 Calendar Integration (Time Awareness Layer) >>>>> Without a calendar, AI assistants are blind to time.
>>>>> We integrate Google Calendar.
>>>>> >>>>> Step 1 — Google Calendar API >>>>> Enable:
>>>>> Google Calendar API >>>>> OAuth credentials >>>>> Store securely in:
>>>>> ~/ai-os/secrets/calendar/ >>>>> >>>>> Step 2 — MCP Calendar Tool >>>>> Capabilities:
>>>>> list events >>>>> create events >>>>> reschedule events >>>>> detect conflicts >>>>> suggest time slots >>>>> Step 3 — Scheduling Intelligence >>>>> Hermes can now reason about time:
>>>>> Example:
>>>>> >>>>> “Schedule a meeting with John next week.” >>>>> Flow:
>>>>> Check availability >>>>> ↓ >>>>> Suggest slots >>>>> ↓ >>>>> Ask confirmation >>>>> ↓ >>>>> Create event >>>>> >>>>> 8.6 Unified Inbox Concept >>>>> We now define a key abstraction:
>>>>> Everything is an “event stream.” >>>>> Source Type >>>>> Email messages >>>>> Telegram messages >>>>> Calendar time events >>>>> MCP tools actions >>>>> System logs alerts >>>>> >>>>> Hermes treats all of these as inputs into one system.
>>>>> 8.7 Notification Layer >>>>> We introduce a central notification rule:
>>>>> Hermes must notify you when:
>>>>> >>>>> action requires approval >>>>> system error occurs >>>>> email is high priority >>>>> task is completed >>>>> scheduled event is approaching >>>>> All notifications go through Telegram first.
>>>>> Later we can expand to:
>>>>> >>>>> mobile push >>>>> email digests >>>>> desktop alerts >>>>> 8.8 Action Approval System >>>>> We define three action levels:
>>>>> Level 1 — Auto >>>>> Level 2 — Suggest + Wait >>>>> Level 3 — Require explicit approval >>>>> >>>>> Examples:
>>>>> Action Level >>>>> Read email 1 >>>>> Summarize email 1 >>>>> Draft reply 2 >>>>> Send email 3 >>>>> Create calendar event 2 >>>>> Delete data 3 >>>>> >>>>> This prevents catastrophic automation mistakes.
>>>>> 8.9 Logging & Audit Trail >>>>> Every external action is logged:
>>>>> logs/ >>>>> email/ >>>>> telegram/ >>>>> calendar/ >>>>> actions/ >>>>> >>>>> Each log includes:
>>>>> input >>>>> decision >>>>> tool used >>>>> outcome >>>>> approval status >>>>> This is critical for debugging and trust.
>>>>> 8.10 First End-to-End Test >>>>> Once everything is connected:
>>>>> Send yourself an email like:
>>>>> >>>>> “Can we meet next Tuesday at 3pm?” >>>>> Expected flow:
>>>>> Gmail MCP detects email >>>>> Hermes reads and classifies it >>>>> Calendar MCP checks availability >>>>> Hermes drafts response >>>>> Telegram sends approval request >>>>> You approve >>>>> Email is sent >>>>> Calendar event is created (if needed) >>>>> Logs are stored >>>>> This is your first real-world autonomous workflow >>>>> 8.11 What We Are NOT Doing Yet >>>>> We are NOT yet adding:
>>>>> browser automation >>>>> voice control >>>>> full autonomous email sending >>>>> financial transactions >>>>> multi-agent systems >>>>> Reason:
>>>>> We need stability before autonomy.
>>>>> >>>>> 8.12 Why This Chapter Is a Turning Point >>>>> Before this chapter:
>>>>> Hermes lived inside your server >>>>> After this chapter:
>>>>> Hermes lives inside your life systems >>>>> It can now:
>>>>> communicate with people >>>>> manage your schedule >>>>> process your inbox >>>>> notify you in real time >>>>> This is the point where it becomes a true personal assistant system, >>>>> not just a technical tool.
>>>>> End of Chapter 8 >>>>> You now have a working bridge between Hermes and your real-world >>>>> communication channels.
>>>>> It can:
>>>>> >>>>> read your messages >>>>> manage your time >>>>> draft responses >>>>> request approvals >>>>> keep everything synchronized >>>>> What’s Next >>>>> Chapter 9 is where things become significantly more powerful.
>>>>> We will add:
>>>>> >>>>> browser automation (Playwright) >>>>> research agent capabilities >>>>> web scraping and structured extraction >>>>> document ingestion pipelines >>>>> real-time knowledge retrieval >>>>> This is where Hermes begins to understand and navigate the internet >>>>> itself, not just your personal systems.
>>>>> >>>>> >>>>> On Fri, Jul 3, 2026 at 4:11 AM Victoria K <vkushelnyk@gmail.com> >>>>> wrote:
>>>>> >>>>>> The Personal AI Operating System >>>>>> Building a Production-Grade Hermes Agent with Memory, Automation, and >>>>>> Autonomous Coding >>>>>> Chapter 7 >>>>>> Tooling the Agent — Introducing MCP and External Capabilities >>>>>> “Memory makes an AI informed. Tools make it useful.” >>>>>> At this point, Hermes can:
>>>>>> Run on your VPS >>>>>> Talk to you >>>>>> Use a model backend >>>>>> Store structured memory in Obsidian + Qdrant >>>>>> But there’s a hard limitation:
>>>>>> 👉 It still cannot reliably do things in the world.
>>>>>> >>>>>> It can reason about your Git repo… >>>>>> but it cannot safely operate Git.
>>>>>> >>>>>> It can suggest email replies… >>>>>> but it cannot send them.
>>>>>> >>>>>> It can describe a Docker fix… >>>>>> but it cannot execute it.
>>>>>> >>>>>> This chapter fixes that.
>>>>>> >>>>>> We introduce MCP (Model Context Protocol) as the standardized tool >>>>>> layer between Hermes and the real world.
>>>>>> >>>>>> 7.1 What MCP Actually Is >>>>>> MCP is a tool interface layer for AI agents.
>>>>>> Instead of hardcoding integrations like:
>>>>>> >>>>>> “write a Gmail plugin” >>>>>> “write a GitHub plugin” >>>>>> “write a filesystem tool” >>>>>> “write a database connector” >>>>>> You instead plug everything into one standard:
>>>>>> Hermes >>>>>> ↓ >>>>>> MCP Client >>>>>> ↓ >>>>>> MCP Servers (tools) >>>>>> ↓ >>>>>> Real systems (Git, FS, DB, APIs) >>>>>> >>>>>> Each tool becomes a small independent service.
>>>>>> 7.2 Why MCP Is Critical >>>>>> Without MCP, every integration becomes custom code.
>>>>>> That leads to:
>>>>>> >>>>>> messy architecture >>>>>> duplicated logic >>>>>> security issues >>>>>> fragile integrations >>>>>> With MCP:
>>>>>> every tool follows the same interface >>>>>> tools are interchangeable >>>>>> permissions are centralized >>>>>> new capabilities plug in instantly >>>>>> This is what makes your system scalable beyond 10–20 tools.
>>>>>> 7.3 The Tool Philosophy >>>>>> We are NOT building:
>>>>>> a chatbot with plugins >>>>>> We ARE building:
>>>>>> a general-purpose agent runtime with a tool ecosystem >>>>>> Think of MCP as:
>>>>>> “USB ports for intelligence” >>>>>> You can plug anything in.
>>>>>> Unplug it anytime.
>>>>>> >>>>>> 7.4 Core Tool Categories >>>>>> We will eventually connect Hermes to:
>>>>>> File System Tools >>>>>> read/write files >>>>>> search directories >>>>>> edit codebases >>>>>> Developer Tools >>>>>> Git >>>>>> GitHub >>>>>> CI/CD triggers >>>>>> Docker >>>>>> Data Tools >>>>>> PostgreSQL >>>>>> Qdrant >>>>>> Redis >>>>>> Communication Tools >>>>>> Email (Gmail API) >>>>>> Telegram bot >>>>>> Slack (optional) >>>>>> Automation Tools >>>>>> n8n workflows >>>>>> scheduled tasks >>>>>> cron jobs >>>>>> Research Tools >>>>>> web search >>>>>> documentation lookup >>>>>> scraping (Playwright) >>>>>> 7.5 MCP Architecture Overview >>>>>> Hermes Agent >>>>>> │ >>>>>> MCP Client Layer >>>>>> │ >>>>>> ┌───────────────┼────────────────┐ >>>>>> ▼ ▼ ▼ >>>>>> Filesystem GitHub PostgreSQL >>>>>> ▼ ▼ ▼ >>>>>> Local files Repos Structured data >>>>>> >>>>>> Each MCP server is:
>>>>>> independent >>>>>> sandboxed >>>>>> replaceable >>>>>> restartable >>>>>> auditable >>>>>> 7.6 Installing the MCP Host Layer >>>>>> We now extend your ai-os stack.
>>>>>> Inside:
>>>>>> >>>>>> ~/ai-os/compose/mcp/ >>>>>> >>>>>> We will eventually run multiple MCP servers as containers.
>>>>>> But first, create the structure:
>>>>>> >>>>>> mkdir -p ~/ai-os/compose/mcp >>>>>> mkdir -p ~/ai-os/services/mcp >>>>>> >>>>>> 7.7 MCP Security Model >>>>>> This is important.
>>>>>> MCP servers must NEVER:
>>>>>> >>>>>> run as root >>>>>> have unrestricted filesystem access >>>>>> expose public ports directly >>>>>> store secrets unencrypted >>>>>> Instead:
>>>>>> run in Docker >>>>>> connect only to Hermes internally >>>>>> expose minimal API surface >>>>>> use explicit permission rules >>>>>> 7.8 Filesystem MCP (First Tool) >>>>>> We start simple: filesystem access.
>>>>>> Why?
>>>>>> >>>>>> Because everything Hermes does eventually touches files.
>>>>>> >>>>>> This MCP server allows:
>>>>>> >>>>>> reading project files >>>>>> writing notes >>>>>> editing code >>>>>> searching directories >>>>>> But it will be sandboxed to:
>>>>>> ~/ai-os/workspace >>>>>> >>>>>> Nothing outside this folder.
>>>>>> 7.9 Git MCP (Second Tool) >>>>>> Next:
>>>>>> Git integration.
>>>>>> >>>>>> Capabilities:
>>>>>> >>>>>> status >>>>>> commit >>>>>> diff >>>>>> branch >>>>>> pull >>>>>> push (requires approval) >>>>>> This is what turns Hermes into a real coding assistant.
>>>>>> Without Git tools, it’s just writing suggestions.
>>>>>> >>>>>> With Git tools, it becomes a developer.
>>>>>> >>>>>> 7.10 Database MCP (Third Tool) >>>>>> We connect:
>>>>>> PostgreSQL >>>>>> Qdrant >>>>>> Redis (later) >>>>>> This allows Hermes to:
>>>>>> query structured data >>>>>> store memory efficiently >>>>>> manage embeddings >>>>>> track system state >>>>>> 7.11 Tool Permission System >>>>>> We introduce a critical concept:
>>>>>> Tool permission levels >>>>>> Level Meaning >>>>>> read-only safe inspection >>>>>> suggest can propose changes >>>>>> draft can create outputs >>>>>> execute can run actions (restricted) >>>>>> admin never default >>>>>> >>>>>> Example:
>>>>>> Hermes can READ emails >>>>>> It can DRAFT replies >>>>>> It cannot SEND without approval >>>>>> 7.12 MCP Client in Hermes >>>>>> Hermes must be configured to:
>>>>>> discover MCP servers >>>>>> list available tools >>>>>> request execution >>>>>> handle responses >>>>>> log all tool calls >>>>>> This creates a traceable chain of actions.
>>>>>> 7.13 Tool Call Lifecycle >>>>>> Every action follows this flow:
>>>>>> User request >>>>>> ↓ >>>>>> Hermes reasoning >>>>>> ↓ >>>>>> Select tool >>>>>> ↓ >>>>>> MCP call request >>>>>> ↓ >>>>>> Tool executes in sandbox >>>>>> ↓ >>>>>> Result returned >>>>>> ↓ >>>>>> Hermes continues reasoning >>>>>> >>>>>> This is how agents become reliable instead of chaotic.
>>>>>> 7.14 First Real Use Case >>>>>> Once MCP is working, Hermes can:
>>>>>> “Open my AI project and fix the Docker Compose issue.” >>>>>> Then:
>>>>>> Reads filesystem >>>>>> Finds compose file >>>>>> Detects misconfiguration >>>>>> Proposes fix >>>>>> Applies patch (if approved) >>>>>> Runs container >>>>>> Reports result >>>>>> This is the first moment your system becomes agentic.
>>>>>> 7.15 Logging Everything >>>>>> Every tool call must be logged:
>>>>>> tool name >>>>>> inputs >>>>>> outputs >>>>>> timestamps >>>>>> success/failure >>>>>> user approval status >>>>>> Stored in:
>>>>>> ~/ai-os/logs/mcp/ >>>>>> >>>>>> This is critical for debugging and trust.
>>>>>> 7.16 Why MCP Beats Traditional Plugins >>>>>> Old model:
>>>>>> each tool is custom code >>>>>> tightly coupled to agent >>>>>> hard to maintain >>>>>> MCP model:
>>>>>> standardized protocol >>>>>> tool isolation >>>>>> easy swapping >>>>>> scalable architecture >>>>>> multi-agent compatibility >>>>>> This is why modern agent systems are moving toward it.
>>>>>> 7.17 What We Are NOT Doing Yet >>>>>> We are NOT yet connecting:
>>>>>> Gmail >>>>>> Telegram >>>>>> Browser automation >>>>>> Calendar >>>>>> Voice >>>>>> External APIs >>>>>> Reason:
>>>>>> If we connect everything at once, debugging becomes impossible.
>>>>>> >>>>>> We build incrementally.
>>>>>> >>>>>> 7.18 Verification Checklist >>>>>> Before moving on:
>>>>>> MCP folder structure exists >>>>>> filesystem tool restricted to workspace >>>>>> Git operations are sandboxed >>>>>> tool calls are logged >>>>>> Hermes can list available tools >>>>>> at least one tool executes successfully >>>>>> permission system is enforced >>>>>> If any of these fail, fix before continuing.
>>>>>> End of Chapter 7 >>>>>> You now have the missing piece that transforms Hermes from:
>>>>>> “an AI that talks” >>>>>> into:
>>>>>> “an AI that acts” >>>>>> But it is still isolated from your real workflows.
>>>>>> Right now it can:
>>>>>> >>>>>> read files >>>>>> edit projects >>>>>> manage code >>>>>> But it cannot yet touch your life systems:
>>>>>> email >>>>>> messaging >>>>>> scheduling >>>>>> external communication >>>>>> What’s Next >>>>>> Chapter 8 is where everything starts connecting.
>>>>>> We will integrate:
>>>>>> >>>>>> Gmail (email intelligence) >>>>>> Telegram (real-time communication) >>>>>> Calendar (time management) >>>>>> Notification system (alerts + approvals) >>>>>> This is the chapter where Hermes starts becoming a personal executive >>>>>> assistant, not just a developer tool.
>>>>>> From this point forward, each chapter will dramatically increase how >>>>>> much of your real-world workload the system can handle.
>>>>>> >>>>>> >>>>>> On Fri, Jul 3, 2026 at 4:11 AM Victoria K <vkushelnyk@gmail.com> >>>>>> wrote:
>>>>>> >>>>>>> The Personal AI Operating System >>>>>>> Building a Production-Grade Hermes Agent with Memory, Automation, >>>>>>> and Autonomous Coding >>>>>>> Chapter 6 >>>>>>> Building Long-Term Memory — Giving Hermes a Second Brain >>>>>>> "Intelligence is knowing things. Wisdom is remembering what matters."
>>>>>>> One of the biggest limitations of most AI assistants is that they >>>>>>> forget.
>>>>>>> You may spend hours explaining your project, your coding style, your >>>>>>> business, and your preferences, only to repeat yourself in the next >>>>>>> conversation.
>>>>>>> >>>>>>> That isn't acceptable for a personal AI operating system.
>>>>>>> >>>>>>> This chapter solves that problem.
>>>>>>> >>>>>>> By the end of this chapter, Hermes will have:
>>>>>>> >>>>>>> A human-readable knowledge base >>>>>>> Persistent memory >>>>>>> Semantic search >>>>>>> Automatic memory retrieval >>>>>>> Project documentation >>>>>>> Daily journals >>>>>>> A "second brain" that grows over time >>>>>>> This is where your AI stops acting like a chatbot and starts acting >>>>>>> like a colleague.
>>>>>>> 6.1 Understanding Memory >>>>>>> Memory is often misunderstood.
>>>>>>> Many people think:
>>>>>>> >>>>>>> "The AI should remember everything."
>>>>>>> That sounds useful until you realize it becomes expensive, slow, and >>>>>>> noisy.
>>>>>>> Instead, we want layered memory.
>>>>>>> >>>>>>> Our system will have four types of memory:
>>>>>>> >>>>>>> Active Conversation >>>>>>> │ >>>>>>> Working Memory >>>>>>> │ >>>>>>> Project Memory >>>>>>> │ >>>>>>> Long-Term Knowledge >>>>>>> >>>>>>> Each layer has a different purpose.
>>>>>>> 6.2 The Four Memory Layers >>>>>>> Layer 1 – Conversation Memory >>>>>>> This is the current chat.
>>>>>>> It exists only while you're working.
>>>>>>> >>>>>>> Example:
>>>>>>> >>>>>>> Fix the login page.
>>>>>>> Hermes remembers the discussion until the task is complete.
>>>>>>> Layer 2 – Working Memory >>>>>>> Temporary notes for ongoing work.
>>>>>>> Examples:
>>>>>>> >>>>>>> Current bug >>>>>>> Next task >>>>>>> Test results >>>>>>> TODO list >>>>>>> This memory changes constantly.
>>>>>>> Layer 3 – Project Memory >>>>>>> Every project gets its own documentation.
>>>>>>> Example:
>>>>>>> >>>>>>> Projects/ >>>>>>> >>>>>>> AI Operating System/ >>>>>>> >>>>>>> README.md >>>>>>> >>>>>>> Architecture.md >>>>>>> >>>>>>> Known Bugs.md >>>>>>> >>>>>>> Roadmap.md >>>>>>> >>>>>>> Meeting Notes.md >>>>>>> >>>>>>> TODO.md >>>>>>> >>>>>>> Months later, Hermes immediately understands the project.
>>>>>>> Layer 4 – Long-Term Memory >>>>>>> Everything worth remembering eventually ends up here.
>>>>>>> Examples:
>>>>>>> >>>>>>> Your coding preferences >>>>>>> Favorite libraries >>>>>>> API keys (never stored in plain text) >>>>>>> Business ideas >>>>>>> Research >>>>>>> Documentation >>>>>>> Design decisions >>>>>>> Meeting summaries >>>>>>> This becomes your digital brain.
>>>>>>> 6.3 Why Obsidian?
>>>>>>> Many AI systems store memory inside a database.
>>>>>>> That works...
>>>>>>> >>>>>>> Until the software disappears.
>>>>>>> >>>>>>> Instead, we're going to store almost everything as Markdown.
>>>>>>> >>>>>>> Advantages:
>>>>>>> >>>>>>> Human readable >>>>>>> Future proof >>>>>>> Git compatible >>>>>>> Easy backup >>>>>>> Works without AI >>>>>>> No vendor lock-in >>>>>>> Your AI memory remains useful even if Hermes disappears tomorrow.
>>>>>>> 6.4 Installing Obsidian >>>>>>> Although Obsidian is a desktop application, think of it as the >>>>>>> editor for your AI's knowledge base.
>>>>>>> Create a vault called:
>>>>>>> >>>>>>> AI Brain >>>>>>> >>>>>>> Store the vault inside your synchronized project directory so it can >>>>>>> be backed up and version-controlled.
>>>>>>> 6.5 Folder Structure >>>>>>> Create the following structure:
>>>>>>> AI Brain/ >>>>>>> >>>>>>> Daily Notes/ >>>>>>> >>>>>>> Projects/ >>>>>>> >>>>>>> Programming/ >>>>>>> >>>>>>> Research/ >>>>>>> >>>>>>> People/ >>>>>>> >>>>>>> Meetings/ >>>>>>> >>>>>>> Ideas/ >>>>>>> >>>>>>> Business/ >>>>>>> >>>>>>> Health/ >>>>>>> >>>>>>> Learning/ >>>>>>> >>>>>>> Books/ >>>>>>> >>>>>>> Articles/ >>>>>>> >>>>>>> Scratchpad/ >>>>>>> >>>>>>> Templates/ >>>>>>> >>>>>>> Archive/ >>>>>>> >>>>>>> Don't over-organize.
>>>>>>> Folders help.
>>>>>>> >>>>>>> Links help more.
>>>>>>> >>>>>>> 6.6 The Philosophy of Notes >>>>>>> Good notes answer questions.
>>>>>>> Bad notes collect information.
>>>>>>> >>>>>>> Instead of writing:
>>>>>>> >>>>>>> Docker >>>>>>> >>>>>>> Write:
>>>>>>> Why did we choose Docker?
>>>>>>> >>>>>>> Advantages >>>>>>> >>>>>>> Disadvantages >>>>>>> >>>>>>> Alternatives >>>>>>> >>>>>>> Lessons learned >>>>>>> >>>>>>> Useful commands >>>>>>> >>>>>>> Your future AI can reason much better from structured notes.
>>>>>>> 6.7 Linking Notes >>>>>>> One of Obsidian's superpowers is linking.
>>>>>>> Example:
>>>>>>> >>>>>>> [[Hermes]] >>>>>>> >>>>>>> [[OpenHands]] >>>>>>> >>>>>>> [[Qdrant]] >>>>>>> >>>>>>> [[Telegram Automation]] >>>>>>> >>>>>>> After a year you'll have a knowledge graph instead of isolated >>>>>>> documents.
>>>>>>> Hermes can use these relationships to provide richer answers.
>>>>>>> >>>>>>> 6.8 Daily Notes >>>>>>> Create one note every day.
>>>>>>> Example:
>>>>>>> >>>>>>> 2026-07-03 >>>>>>> >>>>>>> Today's goals >>>>>>> >>>>>>> Finished:
>>>>>>> >>>>>>> Installed Hermes >>>>>>> >>>>>>> Problems:
>>>>>>> >>>>>>> Docker networking >>>>>>> >>>>>>> Ideas:
>>>>>>> >>>>>>> Multi-agent architecture >>>>>>> >>>>>>> Tomorrow:
>>>>>>> >>>>>>> Install Qdrant >>>>>>> >>>>>>> Hermes will eventually help generate these automatically.
>>>>>>> 6.9 Templates >>>>>>> Create reusable templates.
>>>>>>> Example:
>>>>>>> >>>>>>> Project template:
>>>>>>> >>>>>>> # Project >>>>>>> >>>>>>> ## Purpose >>>>>>> >>>>>>> ## Architecture >>>>>>> >>>>>>> ## Technologies >>>>>>> >>>>>>> ## TODO >>>>>>> >>>>>>> ## Decisions >>>>>>> >>>>>>> ## Problems >>>>>>> >>>>>>> ## References >>>>>>> >>>>>>> Meeting template:
>>>>>>> # Meeting >>>>>>> >>>>>>> Participants >>>>>>> >>>>>>> Agenda >>>>>>> >>>>>>> Notes >>>>>>> >>>>>>> Actions >>>>>>> >>>>>>> Follow-up >>>>>>> >>>>>>> Consistency improves retrieval.
>>>>>>> 6.10 Semantic Search >>>>>>> Folders aren't enough.
>>>>>>> Imagine asking:
>>>>>>> >>>>>>> "Find every note where I discussed vector databases."
>>>>>>> You don't remember whether it was called:
>>>>>>> Qdrant >>>>>>> Embeddings >>>>>>> AI Memory >>>>>>> Database Research >>>>>>> Semantic search solves this.
>>>>>>> Instead of matching words...
>>>>>>> >>>>>>> It matches meaning.
>>>>>>> >>>>>>> 6.11 Vector Embeddings >>>>>>> This is how semantic memory works.
>>>>>>> Markdown >>>>>>> >>>>>>> ↓ >>>>>>> >>>>>>> Chunking >>>>>>> >>>>>>> ↓ >>>>>>> >>>>>>> Embedding Model >>>>>>> >>>>>>> ↓ >>>>>>> >>>>>>> Vector Database >>>>>>> >>>>>>> ↓ >>>>>>> >>>>>>> Semantic Search >>>>>>> >>>>>>> ↓ >>>>>>> >>>>>>> Hermes >>>>>>> >>>>>>> Hermes asks:
>>>>>>> "What memories relate to this conversation?"
>>>>>>> The vector database answers.
>>>>>>> 6.12 Choosing a Vector Database >>>>>>> There are many excellent options:
>>>>>>> Database Notes >>>>>>> Qdrant Fast, simple, open source >>>>>>> Chroma Lightweight >>>>>>> Weaviate Powerful but larger >>>>>>> LanceDB Excellent for local development >>>>>>> pgvector Good if you already use PostgreSQL >>>>>>> >>>>>>> For this book we'll use Qdrant because it's reliable, actively >>>>>>> maintained, and integrates well with retrieval pipelines.
>>>>>>> 6.13 What Should Be Embedded?
>>>>>>> Not everything.
>>>>>>> Good candidates:
>>>>>>> >>>>>>> Notes >>>>>>> Documentation >>>>>>> Research >>>>>>> Project plans >>>>>>> Meeting summaries >>>>>>> Books >>>>>>> Code explanations >>>>>>> API documentation >>>>>>> Avoid embedding:
>>>>>>> Large binary files >>>>>>> Passwords >>>>>>> Private keys >>>>>>> Certificates >>>>>>> Encrypted secrets >>>>>>> >>>>>>