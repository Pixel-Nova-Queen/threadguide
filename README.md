# ThreadGuideBot

ThreadGuideBot is a Reddit assistant that helps users and moderators find relevant past discussions before repetitive questions get asked and answered again. It is designed to improve discovery, reduce duplicate posts, and surface useful older Reddit threads inside approved communities.

## Overview

Many subreddits receive the same beginner, support, or FAQ-style questions repeatedly. Valuable answers already exist on Reddit, but they are often hard to find quickly through normal browsing.

ThreadGuideBot uses the Reddit API to read posts and comments from approved subreddits, detect similar historical discussions, and return a small set of relevant prior Reddit threads. Depending on subreddit approval and configuration, it can either:

- Power a private moderator-facing dashboard.
- Respond to a command trigger in comments.
- Post a single approved reply on duplicate-style questions.

The goal is to help Redditors get better answers faster while reducing repetitive work for moderators and frequent contributors. Reddit’s OAuth model is scope-based, so the app will request only the minimum scopes required for the approved functionality.

## Problem

Communities often lose useful knowledge because:
- The same questions are posted repeatedly.
- New users do not know the right keywords to search.
- Good answers are buried in older threads.
- Moderators spend time redirecting users to existing resources manually.

This project addresses that by making relevant Reddit discussions easier to discover at the moment they are needed.

## Benefits to Redditors

- Helps users find better answers faster.
- Reduces duplicate questions.
- Surfaces older high-quality Reddit discussions.
- Supports moderators with lightweight tooling.
- Keeps the experience centered on Reddit content rather than redirecting users away from the platform.

## How It Works

1. The app reads posts, comments, titles, timestamps, subreddit metadata, and permalinks from approved subreddits using Reddit’s API.
2. A backend service indexes relevant thread metadata and text signals.
3. When a new submission is created, or when triggered by a moderator or command, the app compares the content against prior discussions.
4. The app ranks similar threads and returns a short list of the most relevant Reddit links.
5. If public replies are enabled for that subreddit, the bot posts one concise response. Otherwise, results are shown only in an internal dashboard or moderator workflow.

## Example Use Cases

### Example 1: Beginner programming question

A user posts:

> "How do I start learning Python with no experience?"

ThreadGuideBot detects that very similar questions have already been answered many times in the same subreddit. It finds several strong historical discussions and returns links to those threads so the user can immediately access high-quality Reddit answers.

### Example 2: Fitness FAQ

A user asks:

> "What is the best beginner strength routine?"

The bot checks similar posts in an approved fitness subreddit and identifies previous threads covering beginner routines, progression, and common mistakes. If enabled in that community, it posts one helpful reply with a few relevant thread links.

### Example 3: Moderator workflow

A moderator wants to reduce repetitive posting in a support subreddit. The app highlights clusters of duplicate topics, helping the mod team improve pinned resources, wiki pages, weekly megathreads, or automod messaging.

## Platform Scope

This project is intended for use only in:
- A private testing subreddit during development.
- Subreddits where the bot owner has moderator approval or explicit permission to operate.
- Narrowly scoped pilots rather than broad sitewide automation.

The bot is not intended for mass-posting, unsolicited engagement, or broad spam-like activity.

## Why Not Devvit

This project relies on an external backend for indexing, ranking, semantic similarity, and storage of historical discussion metadata. Reddit’s Devvit platform is useful for in-platform experiences, but this project requires a more flexible external service architecture and backend workflow than a Devvit-only implementation currently supports. Devvit Web documentation notes restrictions around certain client-side external request patterns, and Reddit developer discussions also reflect cases where a custom external API is needed.

## Planned Features

- Search and rank similar historical Reddit threads.
- Optional bot replies in approved subreddits.
- Moderator-facing dashboard for duplicate-topic review.
- Configurable per-subreddit rules and thresholds.
- Logging and rate limiting for safe operation.
- Opt-in behavior only where allowed.

## API Access and OAuth

The project will use Reddit OAuth for authenticated API access. Reddit’s OAuth documentation states that API access is limited by explicitly requested scopes, so this project will request only the minimum permissions needed for its approved behavior.

### Planned scopes

These scopes may be adjusted depending on the final approved workflow:

- `read` — Read posts, comments, and subreddit content.
- `history` — Review account interaction history if needed for bot operations.
- `submit` — Post a bot reply in subreddits where this is explicitly approved.
- `privatemessages` — Optional, only if moderator communication or command handling requires it.

If moderator-only features are added later, additional moderator scopes would only be requested if required and approved. Reddit’s documentation emphasizes requesting only the scopes necessary for the intended endpoints.

## Safety and Guardrails

- Operates only in approved subreddits.
- Uses narrow OAuth scopes.
- Does not mass-message users.
- Does not scrape private data.
- Does not repost copyrighted content.
- Avoids spam-like repetitive posting.
- Supports rate limiting and manual disable controls.
- Public commenting can be turned off entirely for dashboard-only use.

## Architecture

High-level flow:

1. Reddit user creates a post or invokes a command.
2. Backend service receives the event or checks the new content.
3. Reddit data is fetched using OAuth-authenticated API calls.
4. Similar past threads are ranked.
5. Results are returned either to:
   - a moderation dashboard, or
   - a single public bot reply in approved communities.

### Components

- Backend API service
- Reddit OAuth integration
- Similarity/ranking engine
- Optional admin or moderator dashboard
- Logging and safety controls

## Repository Structure

```text
.
├── README.md
├── src/
│   ├── api/
│   ├── reddit/
│   ├── ranking/
│   ├── moderation/
│   └── config/
├── docs/
│   ├── architecture.md
│   └── scopes.md
├── .env.example
└── package.json
```

## Setup

### Prerequisites

- Node.js or Python backend environment
- A registered Reddit application
- Reddit client ID and client secret
- Configured redirect URI
- Approved subreddit(s) for testing

### Environment variables

```bash
REDDIT_CLIENT_ID=
REDDIT_CLIENT_SECRET=
REDDIT_REDIRECT_URI=
REDDIT_USER_AGENT=threadguidebot/0.1 by u/YourBotUsername
APP_BASE_URL=
```

### Installation

```bash
git clone https://github.com/yourusername/threadguidebot.git
cd threadguidebot
npm install
```

### Run locally

```bash
npm run dev
```

## Intended Subreddits

Initial rollout is planned for:
- A private development/testing subreddit.
- A small number of communities where moderator approval has been obtained.

This project is intentionally designed for controlled rollout rather than broad deployment.

## Current Status

This repository is an early-stage implementation and documentation hub for a Reddit API access request. Initial work focuses on:

- Defining allowed behaviors.
- Documenting required scopes.
- Building the indexing and ranking backend.
- Preparing a safe pilot for approved subreddits only.

## Roadmap

- [ ] Implement OAuth authentication flow
- [ ] Add subreddit configuration support
- [ ] Build duplicate-thread ranking logic
- [ ] Create dashboard for moderator review
- [ ] Add optional public reply mode
- [ ] Add logging, throttling, and moderation controls
- [ ] Test in private subreddit
- [ ] Request approval for limited live rollout

## Compliance Notes

This project is intended to comply with Reddit API policies, subreddit-specific rules, and approval requirements for API access. It is designed to provide user-facing value on Reddit itself, with narrow access scopes and limited deployment. Reddit’s API approval process emphasizes responsible use of OAuth tokens and controlled access.

## License

Private / All Rights Reserved
