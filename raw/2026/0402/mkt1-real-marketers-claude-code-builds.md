# What 4 Gen Marketers Are Building with Claude Code
URL: https://newsletter.mkt1.co/p/real-marketers-claude-code-builds
Source: MKT1 Newsletter by Emily Kramer

## Claude Decoded for Marketers

Three main Claude products:
- **Claude Chat**: Standard chat interface
- **Claude Cowork**: Desktop app with file access and tool integrations
- **Claude Code**: CLI tool (despite the name, NOT just for coding — marketers can build without writing code)

## What Are Skills?

Reusable sets of instructions stored as Markdown files that Claude pulls into context when triggered. Think of them as structured playbooks Claude can reference automatically.

## Skills Built by Marketers

### 1. Homepage Positioning Checker (Emily Kramer)
- Evaluates B2B startup homepage against MKT1 positioning framework
- Scores hero and full page separately with letter grades
- Checks core positioning questions, gives concrete fixes including rewritten headlines
- Built in 4 iterations of testing on real homepages

### 2. Marketing Advantages Skill (Emily Kramer)
- Two phases: "Identify" (questions to surface top 2-4 advantages) and "Review" (pressure-test stated advantages)
- Forces clarity on whether something is actually a catalyst for growth or just a tactic

### 3. Customer Lookalike Outbound Agent (Elaine Zelby, Tofu)
- Weekly agent looks at closed-won deals in HubSpot
- Pulls structured call data → identifies 10 lookalike companies
- Finds 3-5 contacts per company using Clay
- Drafts 4-email sequence + LinkedIn DMs for each contact
- Sends drafts to Slack for team review
- Built in Claude Cowork, connecting to HubSpot + Clay + Slack + email

### 4. Humanizer Skill (Aditya Vempaty)
- Reviews AI-generated copy for "AI-ness"
- Uses Claude Code + Google's Antigravity

### 5. LinkedIn Ad Competitive Intel Agent (Kamil Rextin, 42 Agency)
- Uses Claude Code
- Deployed via GitHub + Railway + Vercel

## Key Learnings

- Skills are the most powerful starting point for marketers in Claude
- "I asked Claude to build a skill, share a framework I like, test it on real-world examples"
- Build incrementally: start with phase 1, validate, then build phase 2
- Feed real examples (past reviews, feedback) to train the skill
- Turn review processes (copy review, strategy review) into structured skills
- Upgrade skills to full web apps when needed (requires other tools, API access)

## Getting Started Advice

- Learn tech stack basics (HTML, CSS, how JS works, your stack, SEO fundamentals, deployment)
- Get engineer review for customer-facing changes
- Automate the soul-crushing weekly busywork first
- Human-in-the-loop beats fully autonomous
