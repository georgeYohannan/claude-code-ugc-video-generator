# Skill: Research

Use this skill whenever George asks you to research a topic, company, person, trend, or question.

## How It Works

This skill calls the Perplexity API to get up-to-date, sourced answers — then formats the results in a way that's immediately useful to George.

## Steps

1. Load the API key from `.env`:
   ```bash
   source .env && echo $PERPLEXITY_API_KEY
   ```

2. Send the research query to Perplexity using the sonar model:
   ```bash
   source .env && curl -s https://api.perplexity.ai/chat/completions \
     -H "Authorization: Bearer $PERPLEXITY_API_KEY" \
     -H "Content-Type: application/json" \
     -d '{
       "model": "sonar",
       "messages": [
         {
           "role": "system",
           "content": "You are a research assistant. Provide clear, factual, well-structured answers with sources. Be concise but thorough."
         },
         {
           "role": "user",
           "content": "'"QUERY_HERE"'"
         }
       ]
     }'
   ```

3. Parse the response and extract `choices[0].message.content`.

## Output Format

Always return research results in this structure:

---

## Research: [Topic]

**Summary**
2–3 sentence TL;DR of the findings.

**Key Findings**
- Finding 1
- Finding 2
- Finding 3

**Details**
Expanded explanation where useful.

**Sources**
- List any URLs or citations returned by Perplexity

**Relevance to George**
One sentence on how this applies to his work or current priorities.

---

## Usage Examples

- "Research the latest AI tools being adopted by law firms"
- "Research competitors to GYanTech AI"
- "Research best practices for memorial video businesses"
- "Research Catholic apparel market trends"
- "Research how to generate leads as an AI consultant"
