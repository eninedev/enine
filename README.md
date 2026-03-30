# Enine

Small businesses get burned by websites. It takes too long to get going. It looks generic when it's done. Basic things don't work: sitemaps are broken, SEO is absent, AI search engines can't find you.

AI site generators make this worse. They generate freeform HTML — fast, but structureless. The output is slop: generic, hollow, indistinguishable.

**enine inverts this.** Instead of AI generating a website, AI generates *structured business content* that conforms to a typed data model. The structure is the quality gate. When content has a type, a relationship, and a place, everything works: sitemaps, SEO, AI search readiness, theme rendering.

You talk to Claude for 15 minutes. You get a working site with real content.

**enine skills** is a collection of [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skills for services from Enine including [eninesites.com](https://eninesites.com) — the platform that makes this possible.

## Quick start

1. Install enine skills (10 seconds — see below)
2. Run `/enine-sites`
3. Describe your business
4. Review the site on sandbox
5. Publish to production

## Install

**Requirements:** [Claude Code](https://docs.anthropic.com/en/docs/claude-code), [Git](https://git-scm.com/)

No build step. No dependencies. Just clone and run setup.

### Step 1: Install on your machine

Open Claude Code and paste this:

> Install enine skills: run **`git clone https://github.com/enine-dev/enine.git ~/.claude/skills/enine && cd ~/.claude/skills/enine && bash setup`**

That's it. The setup script creates symlinks so Claude Code discovers the skills automatically.

### Step 2: Configure your API token

You need an eninesites API token. Get one from your account at [eninesites.com](https://eninesites.com) or ask support@eninesites.com.

Then either:

```bash
# Option A: environment variable
export ENINE_API_TOKEN=your-token-here

# Option B: config file (persists across sessions)
mkdir -p ~/.enine
echo "token = your-token-here" > ~/.enine/config
```

The skill will ask for your token if it can't find one.

## See it work

Here's a real conversation building a website for eninesites itself:

```
You:    /enine-sites
        I want to build a website for a website building service.

Claude: Tell me about your business. What does a typical customer
        come to you for?

You:    We build websites for small businesses. AI interviews the
        customer, generates structured content, and publishes a
        working site.

Claude: Based on what you've told me, here are 3 services for your
        site. Each is a noun, consistent with your services page:

          1. Website Creation (fa-globe)
          2. Content Strategy (fa-pen)
          3. SEO Auditing (fa-chart-line)

        Accept / Modify / Skip?

You:    Accept.

Claude: Why do customers choose you over alternatives? What would
        your happiest customer say?

You:    The blank paper problem, function & form, auditability,
        plus it's structured, AI-ready, and affordable.

Claude: That's 6 — fits perfectly. All adjectives/value statements:

          1. Structured (fa-shield)
          2. AI-Ready (fa-robot)
          3. Affordable (fa-piggy-bank)
          4. Guided (fa-lightbulb)
          5. Beautiful & Functional (fa-palette)
          6. Auditable (fa-list-check)

        Accept / Modify / Skip?

You:    Accept.

Claude: [walks through how, who, where, blog — one at a time]

Claude: Here's your site structure:

        Your Site: eninesites.com

          Services (what)                    fa-briefcase
            ├── Website Creation             fa-globe
            ├── Content Strategy             fa-pen
            └── SEO Auditing                 fa-chart-line

          Why eninesites (why)               fa-star
            ├── Structured                   fa-shield
            ├── AI-Ready                     fa-robot
            ├── Affordable                   fa-piggy-bank
            ├── Guided                       fa-lightbulb
            ├── Beautiful & Functional       fa-palette
            └── Auditable                    fa-list-check

          How It Works (how)                 fa-cogs
            ├── Talk                         fa-comment
            ├── Review                       fa-eye
            └── Launch                       fa-rocket

          Our Work (where)                   fa-location-dot
            ├── batteryos.com                fa-bolt
            ├── enine.com                    fa-gem
            └── cllctivly.org                fa-people-group

        6 types, 21 artifacts. Approve / Modify / Cancel?

You:    Approve.

Claude: [generates real body copy for every artifact]
        [validates JSON against schema]
        [saves to /tmp/enine-sites-eninesites.com.json]
        [publishes to sandbox.eninesites.com]
        [runs XEO audit — health score: 92/100]

        Your site is live on the sandbox. Take a look.
        Ready to publish to production?

You:    Ship it.

Claude: Published to eninesites.com. XEO audit: 94/100.
        Here are 3 things I'd suggest next:
          1. Add a blog post: "Why AI Can't Find Your Business"
          2. Upload your logo via the media endpoint
          3. Update colors to match your brand
```

You said "website building service." The skill asked what your customers come to you for, what makes you different, how you work, and who you've worked with. It structured all of that into a typed content model, generated real copy, validated it, published to sandbox, ran an SEO audit, and shipped to production. One command.

## Skills

| Skill | What it does |
|-------|-------------|
| `/enine-sites` | Build and manage websites. Discovery interview, content generation, sandbox-first publishing, XEO audits. |

More skills coming: themes, analytics, templates, deploy.

## How it works

enine skills follow the [SKILL.md standard](https://github.com/anthropics/claude-code) — plain Markdown files with YAML frontmatter. No compiled binaries, no background processes, no dependencies beyond Claude Code.

The `/enine-sites` skill encodes the eninesites content model: what-why-how-who-where, the archetype system, content rules (linguistic consistency, cardinality of 3 or 6, types vs pages), and the discovery interview methodology. Claude reads this and becomes a content strategist who knows how to structure business content correctly.

## The content model

Every eninesites website is built on the same typed data model:

| Archetype | Purpose | Example |
|-----------|---------|---------|
| **what** | Your offerings/services | "Consulting", "Products", "Solutions" |
| **why** | Why customers choose you | "Reliable", "Affordable", "Proven" |
| **how** | Your process | "Discover", "Design", "Deliver" |
| **who** | Your team | "Jane Smith, CEO" |
| **where** | Proof of work | "batteryos.com", "ERCOT", "ISO 9001" |
| **blog** | Ongoing content | Article titles |
| **news** | Press/announcements | Headline titles |

Three rules:
1. **3 or 6 artifacts per type.** Depth beats breadth.
2. **Linguistic consistency.** Nouns for what, verbs for how, adjectives for why.
3. **Grouping is a view, not a type.** "Insights" grouping blogs + news is a page, not a collection.

## More info

- [enine.com](https://enine.com)
- [eninesites.com](https://eninesites.com)
- Questions? support@eninesites.com
