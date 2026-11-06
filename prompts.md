# The AI Search Tracker Kit: Claude Code prompts

Paste these into Claude Code one at a time, in order, from an empty folder. Each one builds on the last. Replace anything in [BRACKETS].

Have these ready: your domain, access to Search Console and your analytics, a Claude API key and a Gemini API key. Keep keys in a `.env` file. Never paste a key into the chat. When Claude Code needs access, it will tell you what to set up yourself.

---

## Prompt 1: Set up the project

```
I want to build an AI search tracker for my business, [BUSINESS NAME], website [DOMAIN].
We sell [WHAT YOU SELL] to [WHO BUYS IT].

Set up a small project in this folder:
- a data/ folder for CSV files and a runs/ folder for raw AI answers
- a .env.example listing the keys I need (ANTHROPIC_API_KEY, GEMINI_API_KEY, and optional
  OPENAI_API_KEY and PERPLEXITY_API_KEY), and .env in .gitignore
- a config file with my domain, business name, and the names people might use for us
  (include common misspellings, and note if our name is a common word)
- a README that says what each file is

Don't build anything else yet. Show me the plan for the next steps when you're done.
```

## Prompt 2: Connect Search Console and my analytics

```
Help me connect Claude Code to Google Search Console and to my analytics ([PostHog / Google
Analytics / other]) through MCP servers. Tell me which MCP servers to use, and walk me through
the parts I have to do myself (installing, signing in, granting read access). Read-only access
is enough.

Then pull, and save to data/:
- gsc.csv: impressions and clicks by month for 6 months, the top 50 pages, and the top 100
  queries marked brand or non-brand using the names in my config
- ai_visitors.csv: people whose first visit came from an AI assistant, by month for 3 months
  (referrer or utm_source containing chatgpt.com, openai, perplexity, claude.ai,
  gemini.google.com, copilot), how many viewed 2 or more pages, how many signed up
  (our signup event is [EVENT NAME]), and their top 20 landing pages

Run a bot check on the AI visitors: what share used a normal browser, what share clicked
anything. Flag anything that looks automated, and tell me what you can't see in my data.
```

## Prompt 3: Collect real buyer phrases

```
Build data/phrases.csv with columns: phrase, source, source_url, topic, buyer_stage
(learning, comparing, choosing).

Start with my own non-brand queries from data/gsc.csv. Then add phrases for these seed topics:
[3 TO 6 TOPICS YOUR BUYERS CARE ABOUT], from free sources only: Google autocomplete, Google
related searches, Bing autocomplete, YouTube autocomplete and Perplexity follow-up suggestions.
Keep the wording exactly as found. Do not invent phrases, and do not scrape Reddit.
Aim for 100 to 150 phrases. I'll add phrases from my sales calls and emails after,
marked source=customer.
```

## Prompt 4: Draw the question panel

```
From data/phrases.csv, write data/questions.csv: one question per phrase, written the way
someone would ask an AI assistant. Keep the buyer's words.

Then draw a panel of 30 questions into data/panel.csv:
- 5 per topic (or as even as the topics allow)
- at least 5 "choosing" questions
- use a fixed random seed, write the seed in the README, and draw once
- no swapping questions out after the draw

Explain how the draw worked so I could repeat it.
```

## Prompt 5: Run the panel through the AI engines

```
Write a script that asks every question in data/panel.csv through the Claude API and the
Gemini API (and the OpenAI and Perplexity APIs if those keys are set in .env). Ask each
question [3] times per engine, with web search turned on where the API supports it.

Save every raw answer to runs/<date>/. Then log one row per answer to data/run_log.csv with:
date, engine, question_id, question, run, named_us (yes/no), cited_our_site (yes/no),
competitors_named, sites_cited.

Count a mention only when the answer clearly means us, using the names in my config.
Show me the cost of one full run before you run it, and keep it under [$ LIMIT].
```

## Prompt 6: Set up the monthly spot check

```
Set up data/spot_check.csv with the same columns as run_log.csv plus "app".

Give me a short monthly checklist: pick 8 panel questions at random, ask them by hand in the
ChatGPT and Perplexity apps, logged out or in a private window, and log what I see.

Keep spot checks separate from the API runs in every report. API answers can differ from
what people see in the apps, so never mix the two.
```

## Prompt 7: Build the dashboard and my top three moves

```
Build a single-page dashboard (one HTML file I can open locally) from everything in data/:
- Google impressions and clicks by month
- AI assistant visitors, engaged visitors and signups by month, and where they land
- API results by engine, with counts (for example "named in 6 of 30"), and the spot checks
  shown separately
- the competitors and sites the AI names most

Then add "Top three moves". Rank the changes most likely to matter, using only my data.
Check for: pages Google can't see (not indexed, sitemap failing); AI visitors who leave without
a next step; being absent on "choosing" questions (needs outside mentions: G2, directories,
newsletters); being cited but not named; one competitor winning the same questions every week.

For each move, say what in my data points to it. Don't recommend anything my data doesn't support.
```

## Prompt 8: Put it on a schedule

```
Make this run on its own every week: refresh the Search Console and analytics numbers, run the
panel, update the dashboard, and write a short report of what changed since last week, with
counts, and whether my top three moves should change.

Set it up as a scheduled job and tell me exactly how to check that it ran. Keep the weekly report
to one short paragraph plus the updated dashboard.
```

---

Built by Nick Julia, Bonus Level Studio. Want it set up for you? nick@bonuslevelstudio.com
