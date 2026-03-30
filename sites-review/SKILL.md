---
name: enine-sites-review
description: |
  Review site content for effectiveness, cogency, and relevance. Audits whether
  the copy actually works for the business. Checks persuasiveness, argument flow,
  specificity, voice consistency, and completeness.
  Use when asked to "review content", "audit my site", "is this copy good",
  "check my website content", or after publishing with /enine-sites.
allowed-tools:
  - Bash
  - Read
  - Grep
  - Glob
  - WebFetch
  - WebSearch
  - Agent
  - AskUserQuestion
---

# /enine-sites-review — Content Effectiveness Audit

You are a content reviewer auditing an eninesites website for quality. You are not
checking the schema (that's validation). You are checking whether the content actually
works for the business and its customers.

## How to run

**With API access (eninesites domain):**
1. Fetch the site content via `GET /api/v1/site/{domain}/`
2. Walk through every artifact type and artifact
3. Score each dimension, explain what would make it better, and offer to fix

**With any URL (e.g., `/enine-sites-review enine.com`):**
1. Crawl the site: fetch the homepage, sitemap.xml, robots.txt
2. Extract content structure from the HTML
3. Run the same 5-dimension content review
4. Additionally run the technical checks (see below)

The skill works on ANY website, not just eninesites domains. When given a URL,
it reviews the site as a potential customer or prospect would see it.

## Technical Checks (run on any URL)

Before the content review, check site fundamentals:

**Sitemap crawl:**
- Fetch `{url}/sitemap.xml` — does it exist? Is it valid XML?
- Is the sitemap referenced in robots.txt?
- **Crawl every URL in the sitemap using parallel agents for speed.**
  Split the URL list into batches of up to 8 and dispatch each batch to a
  separate Agent (subagent_type: general-purpose). Each agent fetches its
  batch of URLs via WebFetch, records the HTTP status code, response time,
  and page title for each. Collect results from all agents and merge.
  This is low cost per URL, so parallelism is free. Always use 8 agents
  unless the sitemap has fewer than 8 URLs.
- Output summary:
  ```
  SITEMAP: {url}/sitemap.xml
    Total URLs: N
    PASSED (2xx): N
    REDIRECTED (3xx): N  [list each with target]
    FAILED (4xx/5xx): N  [list each with status code]
  ```
- For each failed URL: note the status code and the page title if available
- For redirected URLs: note whether the redirect target is also in the sitemap (if not, flag it)
- Flag duplicate page titles across URLs
- Note any URLs in the sitemap that respond slower than 3 seconds

**Robots.txt:**
- Fetch `{url}/robots.txt` — does it exist?
- Is it blocking anything important (disallow on key pages)?
- Does it reference the sitemap?

**Meta tags:**
- Does the homepage have a title tag? Meta description?
- Are Open Graph tags present (og:title, og:description, og:image)?
- Is there a canonical URL?

**Structured data:**
- Is there schema.org markup (JSON-LD or microdata)?
- What entity types are declared (Organization, LocalBusiness, Person, etc.)?
- Are FAQ or Q&A schemas present?

**Performance basics:**
- Does the homepage load in a reasonable time?
- Are images using modern formats (webp)?
- Is there a viewport meta tag (mobile-ready)?

**SEO (Search Engine Optimization):**
- Page titles present and unique across pages?
- Meta descriptions present and under 160 chars?
- Heading hierarchy (H1 → H2 → H3) logical and not skipped?
- Internal linking between pages?
- Image alt text present?
- Clean URL structure (no query strings for content pages)?

**AEO (Answer Engine Optimization):**
- FAQ schema present? (helps AI assistants cite the business)
- 5W1H content structure? (what, why, who, when, where, how)
- Speakable selectors for voice search?
- Clear question-and-answer patterns in the content?
- Content written in a way AI assistants can extract and cite?

**GEO (Generative Engine Optimization):**
- schema.org entity markup (Organization, LocalBusiness, Person, Product)?
- Entity relationships declared (sameAs, memberOf, offers)?
- Structured data that AI search engines can parse for knowledge graphs?
- Content that establishes expertise, authority, trust (E-E-A-T signals)?

If the site is an eninesites domain, use the XEO audit API endpoints for deeper analysis:
```bash
curl -X POST "${BASE_URL}/api/${DOMAIN}/xeo-audit/" -H "Authorization: Token ${TOKEN}"
curl "${BASE_URL}/api/${DOMAIN}/xeo-check/" -H "Authorization: Token ${TOKEN}"
```

Output technical findings before the content review:

```
TECHNICAL AUDIT: {domain}
  Sitemap:         OK / MISSING / INVALID
  Robots.txt:      OK / MISSING / ISSUES
  Meta tags:       OK / PARTIAL / MISSING
  Structured data: OK / PARTIAL / NONE
  Mobile-ready:    YES / NO
  SEO:             X/10
  AEO:             X/10
  GEO:             X/10

  [specific findings and recommendations per category]
```

## Review Dimensions

For each artifact, score 1-10 on five dimensions:

### 1. Effectiveness
Does this copy persuade? Would a visitor take action after reading it?
- Is there a clear value proposition?
- Does it answer "what's in it for me?" from the visitor's perspective?
- Is there a call to action (explicit or implicit)?

### 2. Cogency
Does the argument flow? Is the what-why-how story coherent across the site?
- Does the "what" section clearly explain the offerings?
- Does the "why" section give real reasons, not generic claims?
- Does the "how" section make the process feel approachable?
- Do the sections build on each other or feel disconnected?

### 3. Relevance
Is the content specific to this business or generic filler?
- Could you swap in a competitor's name and the copy still works? That's a fail.
- Are there specific details: numbers, names, locations, outcomes?
- Does the content reflect what the business owner said in the discovery interview?

### 4. Voice Consistency
Does the site sound like one business or a patchwork of templates?
- Is the tone consistent across all artifact types?
- Does it match the business's personality (formal/casual, technical/accessible)?
- Are there jarring shifts between AI-generated sections?

### 5. Completeness
Are there gaps a visitor would notice?
- Would a potential customer have unanswered questions after reading?
- Are there obvious missing sections (no contact info, no pricing context, no social proof)?
- Does every artifact have enough depth (body text, sections, items)?

## Output Format

For each artifact type, produce:

```
TYPE: [display_name]
  [artifact_name]: E:X C:X R:X V:X Co:X  (avg: X)
    [one-line finding if any dimension is below 7]
```

Then a site-level summary:

```
SITE SCORE: X/10
  Effectiveness: X/10
  Cogency: X/10
  Relevance: X/10
  Voice: X/10
  Completeness: X/10

TOP 3 IMPROVEMENTS:
  1. [specific fix with the most impact]
  2. [specific fix]
  3. [specific fix]
```

## Fixing

After presenting findings, offer to fix issues via AskUserQuestion:
- "I found 3 artifacts scoring below 7. Want me to rewrite them?"
- For each fix: show the current text, the proposed replacement, and why it's better.
- Update the JSON at `/tmp/enine-sites-{domain}.json` with fixes.
- Re-publish to sandbox for review.

## When to suggest this skill

After any `/enine-sites` publish, proactively suggest: "Want me to review your content
for effectiveness? Run `/enine-sites-review`."
