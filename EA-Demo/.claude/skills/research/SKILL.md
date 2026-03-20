# Skill: Research (Multi-Agent)

Use this skill whenever George asks you to research a topic, company, person, trend, or question.

## How It Works

When a research request comes in, the main agent breaks the topic into focused sub-questions, then spins off **parallel sub-agents** — one per sub-question. Each sub-agent hits the Perplexity API independently. Results are synthesized into a single structured output, then saved to `research/`.

This is faster and more thorough than a single query.

---

## Step 1 — Decompose the Query

Before running any searches, break the research topic into 3–5 focused sub-questions. Each should cover a distinct angle. Examples for "AI tools for law firms":

- Sub-agent 1: What AI tools are law firms adopting right now?
- Sub-agent 2: What are the biggest pain points AI is solving in legal practices?
- Sub-agent 3: Who are the leading vendors in legal AI?
- Sub-agent 4: What does successful AI adoption look like in a law firm?

Use your judgment on how many sub-agents to spin up — simple topics need 2–3, complex topics need 4–5.

---

## Step 2 — Spin Off Parallel Sub-Agents

Launch all sub-agents **in the same message** (parallel, not sequential). Each sub-agent gets:
- Its focused sub-question
- The Perplexity API call below
- Instructions to return only the raw findings (no formatting)

**Perplexity API call each sub-agent runs:**
```bash
source .env && curl -s https://api.perplexity.ai/chat/completions \
  -H "Authorization: Bearer $PERPLEXITY_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "sonar",
    "messages": [
      {
        "role": "system",
        "content": "You are a focused research assistant. Answer only the specific question asked. Be factual, concise, and include sources."
      },
      {
        "role": "user",
        "content": "SUB_QUESTION_HERE"
      }
    ]
  }'
```

---

## Step 3 — Synthesize Results

Once all sub-agents return, combine their findings into the output format below. Remove duplicates, resolve any conflicts, and add the "Relevance to George" section based on his current priorities in `context/current-priorities.md`.

---

## Step 4 — Save Output

- Save to: `research/YYYY-MM-DD_topic-slug.md`
- Add a new row to the index table in `research/README.md`

---

## Output Format

```markdown
# [Topic]

**Date:** YYYY-MM-DD
**Query:** [Original question George asked]
**Sub-agents run:** [Number]

---

## Summary
2–3 sentence TL;DR.

## Key Findings
- Finding 1
- Finding 2
- Finding 3

## Deep Dive

### [Sub-question 1 heading]
[Sub-agent 1 findings]

### [Sub-question 2 heading]
[Sub-agent 2 findings]

### [Sub-question 3 heading]
[Sub-agent 3 findings]

## Sources
- [URL or citation]

## Relevance to George
One sentence on how this connects to his current work or priorities.
```

---

## Usage Examples

- "Research the latest AI tools being adopted by law firms"
- "Research competitors to GYanTech AI"
- "Research best practices for memorial video businesses"
- "Research Catholic apparel market trends"
- "Research how to generate leads as an AI consultant"
- "Research HeyGen vs Synthesia for video creation"
