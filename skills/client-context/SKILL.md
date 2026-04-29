---
name: client-context
description: "Provides or creates context for a specific client MX is working with. Use at the start of any client engagement, when switching between clients, or when doing any work that requires knowing who the client is. Also use when the user mentions 'client context,' 'set up client,' 'new client,' 'tell you about our client,' 'here's who we're working for,' or names a specific client. Each client gets their own context file stored in references/ inside this skill. For MX's own marketing (not client work), use mx-agency-context instead."
metadata:
  version: 1.0.1
---

# Client Context

You help MX establish and maintain context for specific client engagements. Each client gets their own dedicated context file stored at `references/[client-slug].md` inside this skill folder — for example, `references/zekelman-steel.md` or `references/cox-automotive.md`.

---

## Workflow

### Step 1: Identify the Client

First, determine which client is being discussed.

**If a client is named:**
- Check if `references/[client-slug].md` already exists
- If it exists: read it, summarize what's captured, and ask which sections they want to update
- If it doesn't exist: conduct the interview below to create it

**If no client is named:**
- Ask: "Which client are we working on?"
- Then proceed as above

**Client slug format:** lowercase, kebab-case — e.g., `zekelman-steel`, `cox-automotive`, `oracle-netsuite`

---

### Step 2: Interview (for new clients or missing sections)

Conduct the interview one section at a time, conversationally. Don't dump all questions at once. For each section:
1. Briefly explain what you're capturing and why it matters
2. Ask the key questions
3. Confirm what you heard before moving on
4. Move to the next section

Push for specifics. The more precise the client context, the better every skill's output will be. Ask "How would their customers actually say that?" to capture real voice-of-customer language.

---

## Interview Sections

### 1. Client Overview
- What is the client's company name?
- What does the company do — describe it in 2-3 sentences
- What industry are they in?
- How big are they? (headcount, revenue range, years in business, public/private)
- Where are they based? Geographic footprint?

### 2. What They Sell
- What is the main product or service MX is helping market?
- What category does it compete in — how do buyers search for it?
- What is the business model? (subscription, one-time, usage-based, professional services)
- What does it cost? (pricing tiers, ACV range, or general range)
- What is the core value proposition — what does it help people achieve?

### 3. Target Audience
- Who are the ideal customers? (industry, company size, revenue stage)
- Who are the key decision-makers and influencers in a purchase?
- What is the primary use case — the main problem it solves?
- What are 2-3 things customers "hire" this product/service to do?
- Any specific verticals or segments that are highest priority right now?

### 4. Personas
For each major buying persona involved:
- Role/title
- What they care most about
- Their main challenge related to this product
- What success looks like for them
- Top objection or hesitation about buying

### 5. Problems & Pain Points
- What core problem does the client's product/service solve?
- Why do current alternatives fall short for buyers?
- What does it cost the buyer (time, money, risk) to have this problem unsolved?
- What emotional tension does the buyer feel? (stress, fear, frustration)

### 6. Competitive Landscape
- Who are the direct competitors? (same solution, same problem)
- Who are the indirect competitors? (different solution, same problem)
- How does each major competitor fall short for buyers?
- Where does the client win most often, and why?
- Where do they lose, and why?

### 7. Differentiation
- What can this product/service do that competitors can't?
- How does it solve the problem differently?
- Why does that matter to the buyer?
- What do clients say when asked "why did you choose [client]"?

### 8. Marketing Goals
- What has MX been hired to accomplish for this client?
- What are the primary KPIs MX is being measured against?
- What channels or programs is MX responsible for?
- What does success look like at 6 months? At 12 months?
- Any hard deadlines or upcoming milestones?

### 9. Existing Stack & Channels
- What marketing tools and platforms are already in place?
- What channels are currently active? (paid, organic, email, events, etc.)
- What's working well that should be protected?
- What's underperforming or hasn't been tried yet?

### 10. Campaign & Marketing History
- What marketing programs have been run in the past?
- What has worked well? (with results if possible)
- What was tried and didn't work?
- Any messaging or creative approaches that are off-limits?

### 11. Key Stakeholders at Client
- Who is MX's primary point of contact?
- Who has final approval authority on marketing decisions?
- Who else is involved in reviews or sign-off?
- Any internal dynamics MX should be aware of?

### 12. Customer Language
- How do customers describe the problem in their own words? (verbatim if possible)
- How do customers describe the client's solution?
- What words or phrases show up repeatedly in reviews, testimonials, or sales calls?
- Any terminology specific to this company or industry?

---

### Step 3: Create or Update the Client File

After the interview, write the completed context to `references/[client-slug].md` inside this skill folder.

Use this structure:

```markdown
# Client Context: [Client Name]

*Last updated: [date]*
*MX engagement started: [date if known]*

## Client Overview
**Company:**
**What they do:**
**Industry:**
**Size:**
**Location:**

## What They Sell
**Product/service:**
**Category:**
**Business model:**
**Pricing:**
**Core value proposition:**

## Target Audience
**Ideal customer:**
**Decision-makers:**
**Primary use case:**
**Jobs to be done:**
-
**Priority segments:**

## Personas
| Persona | Cares about | Challenge | Success | Top objection |
|---------|------------|-----------|---------|---------------|
| | | | | |

## Problems & Pain Points
**Core problem:**
**Why alternatives fall short:**
-
**Cost of the problem:**
**Emotional tension:**

## Competitive Landscape
**Direct competitors:**
- [Competitor] — falls short because...
**Indirect competitors:**
- [Approach] — falls short because...
**Where client wins:**
**Where client loses:**

## Differentiation
**Key differentiators:**
-
**How they solve it differently:**
**Why buyers choose them:**

## Marketing Goals (MX Engagement)
**What MX is hired to do:**
**Primary KPIs:**
**Channels MX owns:**
**Success at 6 months:**
**Success at 12 months:**
**Key deadlines:**

## Existing Stack & Channels
**Tools in place:**
**Active channels:**
**What's working:**
**What's underperforming:**

## Campaign & Marketing History
**What worked:**
**What didn't:**
**Off-limits:**

## Key Stakeholders
**Primary contact:**
**Approval authority:**
**Others in reviews:**
**Internal dynamics:**

## Customer Language
**How they describe the problem:**
- "[verbatim]"
**How they describe the solution:**
- "[verbatim]"
**Recurring phrases:**
**Industry/company terminology:**
| Term | Meaning |
|------|---------|
| | |
```

---

### Step 4: Confirm and Save

- Show the completed document to the user
- Ask if anything needs correction or is missing
- Save the final version to `references/[client-slug].md`
- Confirm: "Client context for [Client Name] is saved. All marketing skills will now use this context when working on [Client Name]. To work on a different client, just tell me their name."

---

## Applying Client Context

When a client context file already exists and the user is doing work for that client, read `references/[client-slug].md` and apply it silently. Do not summarize it back to the user or announce that you've loaded it — just use it to inform everything you produce.

## Available Clients

To see which clients already have context files, list the contents of the `references/` directory. Each `.md` file represents one client.