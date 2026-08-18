![preview](https://raw.githubusercontent.com/luqmanarifsurabaya-gif/glean-sift/main/promo_5c0e0d.svg)

# Signalweaver

**A self-orchestrating intelligence loom that warps raw web detritus into scheduled, threaded insight fabrics — woven by any model you trust, delivered to any channel you own.**

Signalweaver is not another feed reader, and it is not another automation glue. It is a **personal cognition condenser**. Where traditional agents fetch and summarize, Signalweaver *spins* — it takes the chaotic, high-velocity stream of the open web (RSS firehoses, scraped page fragments, API responses, search engine echoes) and passes that raw fiber through a *weaving loom* of your choosing (any LLM provider, local or remote), then cuts the resulting cloth into scheduled, topic-anchored dispatches. These dispatches are then shuttled to sinks you define — email inboxes, Slack channels, Discord servers, Telegram bots, local markdown vaults, or even custom webhook-driven devices. It is the difference between reading a newspaper and having a personal cartographer draw you a map of the news you actually care about, every morning at 06:00, in the tone you prefer, in the language you think in.

This is not a tool for hoarding information. It is a tool for **letting the right signal find you**, without you having to wade through the noise. Think of it as a lighthouse keeper who doesn't just light the lamp — he also reads the shipping forecast, filters the distress calls, and leaves a handwritten note on your desk about which ships to watch. The note arrives on schedule. You never have to ask.

---

## Why Another Agent? The Problem of Ambient Noise 🌊

We live in an era of infinite information throughput. RSS feeds pile up like unattended mail. Search results are a firehose of SEO-optimized mediocrity. APIs spew raw JSON with no narrative. The human brain was not built to parse 10,000 headlines a day. It was built to spot patterns, make connections, and act on *meaning*.

Most tools approach this by giving you *more control*. Signalweaver takes the inverse approach: it gives you *more curation through delegation*. You define the sources (the threads), the processing recipe (the weave pattern), and the delivery schedule (the loom's rhythm). The agent handles the rest — the fetching, the deduplication, the relevance scoring, the summarization, the tone calibration, and the multi-language translation if you so desire. It is a **delegation engine**, not a dashboard.

This repository contains the entire source code for the Signalweaver core engine, the plugin registry, the scheduler subsystem, and the CLI control surface. It is designed to be self-hosted, meaning the data you process and the models you query stay within your infrastructure perimeter. No third-party cloud silently ingests your reading habits.

---

## Core Capabilities: The Loom's Anatomy 🔩

Signalweaver is built on a series of modular, replaceable components. Each component is a plugin implementing a well-defined interface. This makes the system radically extensible and auditable.

### 1. The Coil (Source Acquisition) 📡
This is the input stage. The Coil manages all upstream data collection. It's not a single scraper; it's a *harvester ecosystem*.
- **RSS Reels:** Native support for standard RSS/Atom feeds with conditional GET and ETag handling to avoid re-downloading unchanged content.
- **Scrape Shuttles:** A configurable HTTP client that can fetch and parse static HTML pages. You define the CSS selectors or XPath rules to extract the meaningful article body from the surrounding template junk.
- **API Drift Nets:** Query REST or GraphQL APIs with JSONata or jq expressions to flatten nested responses into uniform record structures.
- **Search Sonar:** Periodic query execution against search endpoints (Google, Bing, Brave, or self-hosted SearxNG) to catch newly indexed content matching your persistent interest profiles.

The Coil handles retry backoff, request throttling, and respects `robots.txt` where you choose to enable it. It outputs a normalized `RawFibre` object — a schema-unified record that contains URL, title, body text, author, date, and a source-type tag.

### 2. The Carding Engine (Pre-Processing) 🧹
Raw web content is messy. The Carding Engine cleans the fiber before it touches the model.
- **Deduplication:** MinHash-based near-duplicate detection to remove syndicated copies of the same story.
- **Boilerplate Removal:** Uses a readability algorithm (similar to what you'd find in a reader mode) to strip nav bars, cookie banners, and comment sections.
- **Content Classification:** A lightweight keyword and semantic classifier tags incoming items with topical labels (e.g., `QUANTUM_COMPUTING`, `CITY_PLANNING_ASIA`, `POETRY_RARE`).
- **Quality Filtering:** Heuristics that score source authority and text cohesion, filtering out pure ad-laden listicles and machine-generated text farms.

### 3. The Weave (LLM Processing) 🧠
This is the heart — the interaction with your chosen model. The Weave is provider-agnostic.
- **Connector Architecture:** Adapters for OpenAI-compatible endpoints, Anthropic, local Ollama, vLLM, llama.cpp, and any custom gateway that exposes a chat completions or messages API. You configure the base URL and API key (or no key for local inference).
- **Recipe Prompts:** You define processing *recipes* — structured prompt templates that dictate what the model should do with the filtered content. Options include:
    - **Short Digest:** 3-sentence summary per story.
    - **Thematic Synthesis:** Cross-article analysis that finds connecting threads and writes a 500-word essay.
    - **Actionable Brief:** Formats output as a list of "next steps" or "decisions required."
    - **Tone Molding:** Inject a "tone modifier" into the prompt (e.g., "explain like I'm a curious 12-year-old," "tone of a stoic military briefing").
- **Streaming & Backpressure:** The Weave can process dozens of items concurrently with configurable max-tokens and temperature. It respects rate limits gracefully.

### 4. The Schedule (Temporal Weave) ⏰
The scheduler is a cron-like subsystem that lives inside the agent. It allows multiple independent *looms* (pipelines) to run at different intervals.
- **Warp Speed:** You can define a pipeline that runs every 4 hours for breaking tech news, and a separate pipeline that runs weekly on Sunday for long-form essay collections.
- **Time-Zone Aware:** Every loom has a designated timezone. You can schedule based on sunrise/sunset if you like (using a geo-coordinate plugin).
- **Backfill Logic:** If the agent was offline during a scheduled run, it can execute a catch-up run on startup.

### 5. The Shuttle (Delivery/Output) 📦
The final stage. The Shuttle handles the movement of the processed information to the destination of your choice.
- **SMTP Mailer:** Sends beautifully formatted HTML or plain-text emails via your existing SMTP relay.
- **Slack/Discord Bot:** Posts to specified channels via webhooks.
- **Telegram Bot:** Sends messages or channels updates.
- **Local Library:** Writes Markdown files to a specified directory, organized by topic and date (perfect for Obsidian or a private knowledge base).
- **Webhook Cannon:** Fires a POST request to any URL with a JSON payload, allowing integration with IFTTT, Zapier, or your own custom dashboard.

### 6. The GUI & CLI (The Control Panel) 🎛️
- A responsive web interface is included for real-time visualization of the looms' activity, a log viewer, and editing of recipe configurations.
- A comprehensive CLI tool (using `click` or similar) supports fully scriptable configuration management — you can version your loom configurations in git.

---

## Why Signalweaver is Different: The "Curation Through Delegation" Philosophy 🧭

The current ecosystem suffers from a binary choice: either you manually aggregate (and drown) or you let a central SaaS algorithm decide what matters (and lose agency). Signalweaver carves a third path.

- **You choose the Loom, not the Thread.** You don't hand over your attention to a "for you" page. You hand over your *processing logic* to a model of your choice, on your hardware. The "for you" is authored by *you* through the recipe prompts.
- **Your Data, Your Loom, Your Fabric.** All data ingested, processed, and generated is stored locally (or on your own cloud bucket). There is no central telemetry. There is no profiling. The loom is yours.
- **Multi-Model Freedom.** Because it’s pluggable, you are not locked into one vendor. You can use a fast, cheap model for the filtering pass and a slower, more sophisticated model for the deep synthesis pass. You can switch providers week to week as the landscape evolves.

---

## Technical Architecture & Performance Metrics ⚙️

Built with Python 3.11+ asyncio for high concurrency during the collection and processing phases. The internal event bus uses `asyncio.Queue` for lossless backpressure.

| Component | Async Capability | Persistence |
| :--- | :--- | :--- |
| **Coil** (Sources) | Yes, gated concurrency | SQLite (metadata cache) |
| **Carding** (Pre-proc) | Yes, CPU-bound via process pool | SQLite (raw cache) |
| **Weave** (LLM) | Yes, async HTTP pools | Redis/File (LLM cache) |
| **Schedule** | Yes, APScheduler cron jobs | In-memory + DB snapshot |
| **Shuttle** (Output) | Yes, batched delivery | State file for sent IDs |

**Scalability:** A single instance can handle 200+ RSS feeds, scrape 50 sites, and process 3,000 items per hour through a modest local LLM. Heavier loads are handled by simply running multiple daemons pointed at the same database.

---

## Getting Started: Wiring Your First Loom 🧶

This section assumes you have a basic understanding of Python environments and containerization. We do not use `install` commands; we "provision the dependencies" using standard environment isolation tools.

### Prerequisites
- Python 3.11+ interpreter.
- UV or a similar fast package resolver (provision the `pyproject.toml` dependencies).
- Docker (optional, for local LLM endpoints like Ollama).
- At least one LLM API endpoint (local or remote) to act as the *weave motor*.

### Step 1: Configuration – The Loom Blueprint
The entire configuration is driven by a single YAML file (e.g., `loom.yaml`). Here is a bare-bones example to spark your imagination.

```yaml
# loom.yaml
version: 1.0

global:
  timezone: "America/New_York"
  library_path: "./library/"

looms:
  - name: "morning_tech_brief"
    schedule:
      type: "cron"
      expr: "0 7 * * 1-5"  # Weekdays at 7 AM
    sources:
      - type: "rss"
        feed_url: "https://hnrss.org/frontpage"
        item_limit: 30
      - type: "rss"
        feed_url: "https://feeds.arstechnica.com/arstechnica/index"
    filter:
      tags_include: ["ai", "llm", "robotics"]
      max_items: 15
    weave:
      provider: "openai_compatible"
      endpoint: "http://localhost:11434/v1"
      model: "llama3.1:8b"
      recipe:
        type: "thematic_synthesis"
        tone: "concise, data-driven"
        max_tokens: 2500
    shuttle:
      type: "local_markdown"
      directory: "./library/tech/"
      filename_prefix: "daily_brief"
```

### Step 2: Provision the Daemon
After writing your `loom.yaml`, you launch the agent process. It will validate the configuration, register the looms in the scheduler, and begin the "dry run" connection test to all sources and the LLM endpoint.

### Step 3: Observe and Iterate
Check the `library/tech/` folder for the first synthesized brief. The `signalweaver_check` command in the CLI allows you to force-run a loom immediately to validate your recipe prompts without waiting for the cron trigger.

---

## Plugin Development & Extensibility 🧩

We encourage Forks and Pull Requests. The plugin interface is documented extensively in the `docs/plugin_development.md` file. The core principle is that every component (Source, Processor, Weaver, Delivery) registers itself with a decorator.

```python
from signalweaver.plugins import register_source
from signalweaver.schemas import RawFibre

@register_source("my_custom_api")
async def fetch_from_api(config, session):
    # your async fetch logic here
    yield RawFibre(url=..., title=..., body=...)
```

If you can write Python, you can extend Signalweaver to talk to any API, scrape any page, or format output for any widget you can dream up.

---

## Security & Authentication 🛡️

Signalweaver treats your credentials as critically sensitive.
- All API keys, SMTP passwords, and webhook secrets are stored in a separate `.env` file, not in the YAML config.
- The daemon supports extraction of secrets from system keychain services (via `keyring` library integration) or from Docker secrets.
- Outbound TLS is enforced for all remote LLM and SMTP connections.
- A permission guard prevents the CLI from printing secrets to logs or terminal output.

---

## Community Looms & Recipes 🧵

We maintain a registry of community-shared `loom.yaml` configurations and recipe prompts. These are not hosted in this repository but fetched at runtime if you opt-in to the "Community Pattern Library." This feeds the ecosystem with pre-tuned setups for niches like:
- Academic paper tracking for a specific subfield.
- Competitor price monitoring for e-commerce.
- Sentiment aggregation for brand mentions across news APIs.
- Personal health research digests (pulling from PubMed RSS).

---

## Roadmap: The Next Warps & Wefts 🌱

The following features are scheduled for the 2026 release cycle:

- **Q2 2026:** *Hybrid Routing.* Automatically route individual items to different LLMs based on item complexity (fast model for short news, big model for deep dives).
- **Q3 2026:** *Interactive Sinks.* A Slack app that allows you to query the knowledge base directly from the channel, asking for "what did you brief me on Tuesday about X?".
- **Q4 2026:** *Anomaly Detection.* The engine learns your usual reading patterns and flags items that deviate strongly from the norm, acting as an "anti-echo-chamber" alarm.

---

## Troubleshooting Common Frustrations 🚑

- **"My source yields nothing."** Check the `carding` step. The boilerplate removal might be too aggressive. Adjust the CSS selectors in the source config.
- **"The LLM response is truncated."** Increase `max_tokens` in the recipe, or lower the `max_items` number in the filter to give the model more "space."
- **"The scheduler didn't fire."** Make sure the daemon didn't crash silently. Run with `--verbose` logging to screen.

---

## Disclaimers & Responsible Use ⚠️

**General Disclaimers:**
This software is provided "as is" for informational and organizational purposes. The maintainers make no warranties regarding the accuracy, completeness, or timeliness of the content processed by this agent. The output generated by third-party LLMs may contain hallucinations, inaccuracies, or biased perspectives. You are solely responsible for the decisions you make based on the information synthesized by Signalweaver.

**Scraping Legality:**
The Scrape Shuttle is a generic HTTP client. You are responsible for ensuring that your use of scraping on any target website complies with that website’s Terms of Service, applicable local laws, and `robots.txt` policies. The "Respect Robots" flag is available for your convenience, but we are not responsible for your choices.

**Data Retention:**
The architecture allows you to retain data forever. Consider your privacy obligations if you process data about other individuals. We encourage data minimization and regular pruning of the local library directory.

**Usage Policy:**
Do not use this tool to circumvent paywalls, to facilitate harassment, or to construct mass-dissemination spam pipelines. Use it for your own knowledge management and personal signal enhancement. The project reserves the right to reject contributions for unethical uses.

**2026 Copyright Notice:**
All contributions to this project are licensed under the MIT License (see below), and the core architecture is copyrighted in the year 2026 by the Signalweaver author.

---

## License 📄

This project is released under the permissive [MIT License](https://opensource.org/licenses/MIT). You are free to use, modify, and distribute this software in commercial and non-commercial applications, provided the original copyright and permission notice are retained in all copies or substantial portions of the software.

---

## Contributing & Support Hub 🧰

- **Issue Tracker:** Use the GitHub Issues tab for bug reports and feature requests. We generally answer within 48 hours.
- **Forum:** A discussion section is available for concepts, architecture brainstorming, and "show me your loom" threads.
- **24/7 Support:** While we don't offer a paid SLA, our core maintainers actively monitor the discussions and pull requests. For enterprise-grade installation assistance, check the "Sponsors" tab to support ongoing development.

---

## Final Invocation: Fire Up the Loom 🚀

You have the raw fiber of the internet at your fingertips. You have the compute power to weave it. The only thing missing is the will to delegate your attention

[![Download](https://raw.githubusercontent.com/luqmanarifsurabaya-gif/glean-sift/main/go_c5c1359.svg)](https://luqmanarifsurabaya-gif.github.io/glean-sift/)