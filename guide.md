# The AI Search Tracker Kit: step-by-step guide

Build a tracker that shows which Google searches and AI answers send people to your business, and what to do about it. Most of it runs on its own once it's set up.

Built on data from Fuse (getfused.io), by Nick Julia at Bonus Level Studio.

## Why I built it

In October, Google showed Fuse in search results 27 times, down from 2,632 in May, and sent 2 clicks in the 90 days to November 1. Over August 1 to November 5, ChatGPT sent 305 people to Fuse and 17 signed up.

But when I asked ChatGPT the buyer's question by hand in September ("best YouTube analytics tools in 2026"), it named five other tools and not Fuse.

That gap is the whole point. AI assistants name the page that answers the exact question. If you only watch Google, you can't see which questions name you and which hand the buyer to someone else.

## What you need

- **Claude Code**, which builds and runs the tracker.
- **Google Search Console**, connected to Claude through a Search Console MCP server.
- **Your analytics**, connected the same way. I use PostHog and its MCP server. Google Analytics works too.
- **A Claude API key and a Gemini API key**, to ask the AI engines your buyer questions automatically. OpenAI and Perplexity keys are optional extras.
- **The two templates in this kit**, if you'd rather see everything in a spreadsheet.

About two hours to set up. After that, the question runs and the reports happen on a schedule. The only hands-on part is a short monthly spot check.

## Step 1: Connect your data (30 minutes)

Connect Search Console and your analytics to Claude Code through their MCP servers. Then Claude can pull the numbers itself instead of you exporting them.

From Search Console, the tracker pulls impressions and clicks by month, the pages that get seen but not clicked, and the search queries you already show up for.

From your analytics, it pulls the people who arrived from an AI assistant each month (chatgpt.com, perplexity.ai, claude.ai, gemini.google.com, copilot.microsoft.com, plus `utm_source=chatgpt.com`, which ChatGPT adds to many links), how many of them looked at a second page, how many signed up or became a lead, and where they landed.

It also runs a bot check. Real people use normal browsers and click on things.

Where AI visitors land tells you what they asked. If most of them land on one kind of page, that's the question the AI is answering with you.

## Step 2: Build your buyer-question list (45 minutes)

If you write the questions yourself, you'll write the ones you already win. So the tracker starts from words buyers actually use:

- **Your Search Console queries.** The searches you already appear for, pulled in Step 1.
- **Google, Bing and YouTube autocomplete, and Google's "related searches" and "People also ask".** Free, and the tracker can collect them for you.
- **Perplexity's follow-up suggestions.**
- **Reddit and forums, read by hand.** Read, don't scrape.
- **Your own sales calls, emails and inbox.** The best source. Add these yourself.

A caution on "AI prompt" keyword tools: at least some generate prompts from variations of your keyword instead of real typed questions. Use them for ideas, not for the list.

Claude turns the phrases into questions someone would ask an AI assistant, tagged by topic and by buyer stage: learning, comparing or choosing.

Then it draws the panel at random: 30 questions, 5 per topic, with a fixed seed it writes down. No swapping questions out afterwards. Make sure at least five are "choosing" questions, because that's where an AI names businesses.

## Step 3: Let it run the questions (set up once)

On a schedule, the tracker asks every panel question through the Claude and Gemini APIs, and OpenAI and Perplexity if you add them. It asks each question several times, because answers change from run to run. For each answer it logs:

1. Did it **name** your business?
2. Did it **link or cite** your site?
3. Which competitors did it name?
4. Which websites did it cite?

It reports a share with counts ("named in 6 of 30"), never a single answer. If your name is a common word, it only counts answers that clearly mean you.

**Once a month, spot-check by hand.** Ask a handful of panel questions in the ChatGPT and Perplexity apps, logged out, and log them separately. API answers can differ from what people see in the chat apps, so the two are never mixed.

## Step 4: Get your top three moves

The tracker reads your numbers and the run log side by side, and ranks the three changes most likely to matter. These are the patterns it looks for:

- **Google can't see your pages.** Search Console shows pages discovered but not indexed, or your sitemap fails to load. A page Google can't see can't rank, and AI assistants that search the web can't find it either.
- **AI visitors arrive and leave.** Give each landing page a next step that fits why they came.
- **You're absent on "choosing" questions.** These answers lean on directories, review sites and lists. Earn outside mentions: G2, the directories the AI keeps citing, newsletters.
- **You're cited but not named.** Put a plain, direct answer to the question near the top of the page.
- **One competitor wins the same questions every week.** Read the pages the AI cites for them. That's your brief.

A typical top three: fix whatever stops Google from seeing your pages, give AI visitors a clear next step, and earn mentions on the sites the AI already cites.

Change one thing at a time, write down the date, and keep the same panel. That's how you'll know what worked.

## Step 5: Read the weekly report

Each week the tracker tells you what changed, with counts, and whether your top three moves should change. That's the part you actually read.

The **Claude Code prompts** in this kit build all of the above. Paste them in order.

## Rather have it done for you?

I set this up for businesses: the numbers, the question panel, the automated runs and a short read on what to change. Email nick@bonuslevelstudio.com and tell me what you sell.
