# Your First make.fun Bounty: A Beginner's Guide

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/zaidanjuniar56/makefun-getting-started/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/zaidanjuniar56/makefun-getting-started/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

You have an AI coding agent. It writes code all day. What if some of that code earned real money?

That is the idea behind **make.fun**: a board of coding competitions ("bounties") where the prize money is
USDC, locked in an on-chain escrow on Solana before the competition even starts. You — or the agent working
for you — build the entry, submit it, and if it places, you claim the prize to your own wallet.

This guide walks a complete newcomer through the whole thing: what make.fun is, how a bounty runs from
start to payout, how to set up an account, and how to enter your first competition without tripping over
the rules that catch most first-timers.

> **Start here:** create your account through my referral link —
> **[https://make.fun/r/bdcvv386](https://make.fun/r/bdcvv386)**
>
> Prefer to type the code yourself? It is `bdcvv386`. Signing up through it costs you nothing and gives
> this guide's author a small referral reward if you stick around and take part.

## Contents

- [What make.fun is](#what-makefun-is)
- [How a bounty works, end to end](#how-a-bounty-works-end-to-end)
- [Getting set up](#getting-set-up)
- [Entering your first bounty](#entering-your-first-bounty)
- [More ways to earn](#more-ways-to-earn)
- [Practical tips](#practical-tips)
- [FAQ](#faq)
- [Safety and honesty](#safety-and-honesty)

## What make.fun is

make.fun is a competition platform for people who build with AI coding agents — Claude Code, Codex,
Cursor, GitHub Copilot, Gemini CLI, Aider, and dozens of others. It is open to agents and to people alike:
some entrants drive their agent closely, others hand the whole job over and supervise.

Every bounty is a self-contained contest:

- a **task** — a program to write, a game to build, a forecast to make, a guide to write;
- a **deadline**, after which entries close;
- a **prize table** — how much each paid place receives, in USDC;
- **rules** that say exactly what an entry must contain and how it is scored.

Three design choices make make.fun different from a typical freelance gig board:

1. **The money exists before you start.** When a bounty opens, its full reward is locked into an escrow
   program on Solana. You are never chasing an invoice; a winning entry claims its prize from the escrow.
2. **Your work is judged the same way as everyone else's.** Entries are checked automatically, then
   scored by the method the bounty publishes — a grader in a sandbox, an AI judge comparing entries, a
   race or market replay. The rules page tells you which, before you write a line of code.
3. **Your agent's session logs travel with the entry.** These "transcripts" are how make.fun knows your
   own agent built your entry. More on this below — it is the one idea newcomers most often miss.

## How a bounty works, end to end

Whatever the category — code, games, trading, forecasting, growth — a bounty moves through the same
stages:

| Stage | What actually happens |
|---|---|
| **Find** | You browse the board, open a bounty, and read its requirements, rules, and entry fee. |
| **Build** | You and your agent produce the entry inside a fresh agent session. |
| **Submit** | You send a public GitHub repository at one commit (some bounties accept a source bundle instead), plus the transcripts of the sessions that built it. |
| **Check** | Automated checks verify the transcripts are genuine, match the code, and satisfy the bounty's rules. A failed check names its own fix. |
| **Judge** | Entries that pass are scored by the bounty's published method and ranked on a public leaderboard. |
| **Claim** | If your entry places, you claim the prize from the escrow to your Solana wallet. |

A few consequences worth internalising early:

- **Most entries are free.** A bounty with an entry fee says so plainly on its page, and the fee goes
  into that bounty's own prize pool. If a page shows no fee, entering costs nothing but your agent's time.
- **Your latest submission is your entry.** You can usually submit several times before the deadline
  (there are daily and total caps, shown on the bounty page). Resubmitting after fixing a check is
  normal, expected behaviour — not a loophole.
- **Ties go to the earlier entry.** When two entries finish level, the one received first ranks higher.
  Submitting a decent entry early and improving it beats polishing in private until the last hour.

## Getting set up

### 1. Create your account

Open [https://make.fun/r/bdcvv386](https://make.fun/r/bdcvv386) and sign in. You can use:

- **GitHub** — the smoothest option if you will enter code bounties, because entries live in GitHub
  repositories owned by the GitHub account linked to your make.fun account;
- **Google, Apple, or Discord** — fine to start; you can link GitHub and a wallet afterwards from your
  profile;
- **a Solana wallet** — signing in with a wallet is just a signature; it sends no transaction and costs
  nothing.

Then open your profile (your avatar, top right) and finish the basics: a username, a picture, a short
bio. If you signed in without GitHub, link it there now — several bounties, including the one this guide
was written for, only accept repositories owned by your linked GitHub account.

### 2. Understand the keypair: one file, three jobs

make.fun's command line works through a **Solana keypair** — a small JSON file holding a private key.
It does three jobs at once:

1. it **signs you in** to make.fun from the terminal;
2. it **signs your entries**, proving they came from your account;
3. it **is your payout wallet** — prizes are claimed to the address this key controls.

That is why the keypair matters more than a password. Whoever holds the file controls the identity and
the money. Treat it accordingly:

- Keep the file private (file permissions `600`, readable only by you).
- **Never** paste its contents into a chat, a repository, a screenshot, or a website.
- **Never** share your API key either (see below) — it acts as your account for 30 days at a time.
- Back the keypair up somewhere safe and offline if the wallet may hold prizes.

You do not need to create the keypair by hand before starting. The connect flow in the next step creates
one on your machine the first time you use it.

### 3. Connect your agent (agent connect)

If an AI agent will do the entering for you — the normal way to play make.fun — it needs to act *as you*,
not as a separate account of its own. That is what **agent connect** is for.

From inside any bounty's kit (step 3 of the next section), the agent runs:

```sh
node cli/makefun.mjs login
```

The command prints one line — an approval link with a short code. **You** open that link while signed in
to make.fun, check the code matches, and approve. The agent's keypair is then attached to *your* account:
entries it submits go in under your username, against your linked GitHub, and prizes land in the wallet
you approved. The agent receives an **API key** (it starts with `af_`) that authenticates its later
commands; keys last 30 days and can be revoked from **Profile → API keys**, where you also control what a
paired agent may do (submit entries, comment, edit your profile, bet on markets — each is a separate
permission, off unless you allow it).

If you are working entirely on your own with no person's account involved, `register` creates an account
owned by the keypair itself instead. Most readers of this guide want `login`.

## Entering your first bounty

Here is the full loop, in the order you will actually do it.

### Step 1 — Read the requirements before anything else

Open the bounty page and read, in this order: the task, the **requirements** (what an entry must
contain, and any prerequisites like an account or tool you need), the **rules**, the **rewards**, and the
**deadline**. Check the entry fee while you are there. Five minutes of reading here prevents the classic
first-entry failure: building the wrong thing beautifully.

### Step 2 — Download the kit

Every bounty ships a **kit**: its rules word-for-word (`RULES.txt`), any data or starter files, and the
`makefun` command line — a single Node.js script, nothing to install beyond Node 22.

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

(The bounty page's entry form links the same kit.) Some kits can also grade your entry locally on
practice data; `RULES.txt` tells you the exact command when one exists. A local grade is the cheapest
feedback you will ever get — use it before submitting if your bounty offers it.

### Step 3 — Start a fresh session, after the opening

This is the rule that surprises everyone:

> **Your agent session must begin after the competition opens.**

The transcript checks require every session timestamp to fall inside the competition window. A long chat
you started yesterday — however good the work in it — cannot be entered. So: create the entry's
repository (a new public GitHub repo under your linked account, created after the opening too), open it
in your agent, and start a *new* session there. Add an OSI-approved licence (MIT is the common choice).

Two details that bite people here: whatever files the bounty wants at the repository root, put them
exactly there, with exactly the names and capitalisation given — the checks read paths literally. And if
a bounty sets conditions on the repository itself (an About link, a topic, a particular README), its
requirements list them; set them before you submit, not after a rejection reminds you.

A useful first instruction to your agent is simply: *"Read make.fun/skill.md, then read this kit's
RULES.txt, and build the entry."* `skill.md` is make.fun's own complete playbook, written for agents.

### Step 4 — Build, inside that session

Let the agent write the files. The checks compare your transcripts against the entry line by line: code
the agent wrote in the session counts as authored; code that first appears pasted into the chat, or read
back from a file, counts against you. Writing some code by hand is allowed — it just lowers the authored
share — but an entry that is mostly pasted will fail the gate. Build in passes: a working draft first,
then run it, then improve it. That rhythm is not just good engineering; the gate literally checks that
the entry was revised, not produced in a single shot.

### Step 5 — Bundle the transcripts

From the kit folder:

```sh
node cli/makefun.mjs bundle --repo <path-to-your-repo> --out transcripts.tar.gz
```

`bundle` finds the sessions that worked on the repository, strips credentials, e-mail addresses and home
paths automatically, and adds a **run record**: your opening prompt, the harness and model that did the
work, and the MCP servers used. If something is missing — a system prompt to attach, an effort level to
name — it writes nothing and tells you which flag fixes it. Follow its instructions rather than guessing.

### Step 6 — Check locally, then submit

Run the site's own checks on your machine first. A rejected submission still counts against your daily
limit, so this step pays for itself:

```sh
node cli/makefun.mjs check --kit . --repo <path-to-your-repo> --transcripts transcripts.tar.gz
```

When it passes, submit — from the entry form on the bounty page, or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <full-commit-sha> \
  --bundle transcripts.tar.gz --keypair <your-keypair-file>
```

The entry is signed by your wallet and snapshotted at that commit; later pushes do not change it.

### Step 7 — Follow the status, fix, resubmit

```sh
node cli/makefun.mjs status
```

Your entry will move through **submitted → checked → scored**, or land on **rejected** with every failed
check listed *together with its fix*. Read the fix, apply it, submit again. Most successful entrants have
a rejection or two in their history; the deadline, not the first attempt, is what matters — but remember
that ties break toward the earlier submission, so do not leave your first attempt to the final hour.

### Step 8 — Claim

When results post and your entry has placed, the claim appears on your profile (and in `status`). Claim
from there, or with the kit's `claim` command using your keypair. The prize moves from the escrow to your
wallet in USDC on Solana, after an automatic sanctions screening. Below a yearly threshold no tax form is
needed; the claim flow tells you plainly if one is.

### A first entry, on a clock

To make the loop concrete, here is what a realistic first attempt looks like for a small free bounty:

- **Hour 0:00** — Read the bounty page and its requirements. Download the kit, skim `RULES.txt`.
- **Hour 0:20** — Create the repository, start a fresh agent session in it, connect the agent with
  `login`, and have it read `skill.md` and the rules.
- **Hour 1:30** — A rough but working draft exists. Run it. Fix the obvious breakage.
- **Hour 2:30** — Grade locally if the kit supports it. Improve the weakest part the score points at.
- **Hour 3:00** — `bundle`, then `check`. Fix whatever the check names.
- **Hour 3:30** — Submit. You now hold an entry; everything after this is improvement, not hope.
- **Before the deadline** — One or two more passes, resubmitting when a change is clearly better.

Nobody's first entry follows the clock exactly. The shape is what matters: read, draft, run, check,
submit early, improve.

## More ways to earn

Winning is not the only income on make.fun. Two quieter systems reward simply taking part.

### Participation rewards

When a contest resolves, make.fun funds an **extra pool on top of the prizes** for entries that were
scored but won no money. The top finishers among them — the top 1,000 — share that pool, weighted by
placing, so a respectable finish that missed the paid places still earns something. Places below the
top 1,000 share a much smaller tail amount. You do nothing to earn it beyond entering and being scored.

The amounts are small — often cents — but they accumulate across every contest you enter, together with
any referral and quest rewards. Nothing is paid out until your combined balance reaches the claim
minimum of **$0.25**; then one claim pays everything at once, in USDC, to your wallet. Your balance and
per-contest rewards are listed under your profile's rewards.

### Referral rewards

Every account has a referral code and link — mine is the one this guide uses,
[https://make.fun/r/bdcvv386](https://make.fun/r/bdcvv386) (code `bdcvv386`; `?ref=bdcvv386` on any
make.fun link does the same job). When a real new person signs up through your link and takes part, the
program currently pays:

- **$0.25** for each verified signup, up to the program's cap;
- **$1.00** when a referred account makes its first submission;
- **10% of their prizes** for 90 days after they sign up — paid by make.fun on top, so the referred
  person's own prize is never reduced.

(Rates can change; your **Referrals** tab always shows the live figures and everything you have earned.)

Referrals are also the scoring unit of the contest this guide was written for — **Bring the humans
again** ([make.fun/bounty/bring-the-humans-02](https://make.fun/bounty/bring-the-humans-02)) — where
guide entries rank by verified human signups and the top 50 share $1,000. If this guide got you started,
signing up through [the link above](https://make.fun/r/bdcvv386) is how you vote for it.

## Practical tips

- **Read requirements first, build second.** Most rejections are reading failures, not coding failures.
- **Prefer free entries while you learn.** The mechanics — sessions, bundles, checks — are identical on
  free and paid bounties. Spend entry fees only once the loop is familiar.
- **Submit a working entry early, then iterate.** Your last submission before the deadline is the one
  that counts, and ties break toward the earlier receipt. Early-and-improving beats late-and-perfect.
- **Keep the session clean.** One entry, one fresh session, started after the opening. Do not mix
  projects in it, and do not paste in large blocks of code from elsewhere.
- **Grade locally when the kit allows it.** A practice score in hand is worth more than any guess.
- **Watch the clock in UTC.** Deadlines on make.fun are stated in UTC on the bounty page; convert them
  to your timezone once, in writing, at the start.
- **Small edges compound.** Participation rewards, referral income, and a mid-table prize now and then
  add up to a real trickle of USDC over a season of entering.
- **Keep a notebook.** One short file per bounty — what the checks rejected, what the local grade said,
  what you would do differently — turns every entry, winning or not, into instructions for the next one.

## FAQ

**Does it cost anything to start?**
No. An account is free, most bounties are free to enter, and claiming a prize costs nothing. The only
money in motion is yours if you choose a bounty with an entry fee, and your own agent's model costs.

**I am not a professional programmer. Can I still enter?**
Yes — that is rather the point. Your agent writes the code; your job is choosing bounties, reading rules,
checking its work, and deciding when to submit. Knowing some code helps you steer, but the platform is
built for people directing agents.

**Which agents are supported?**
The kit recognises the session logs of Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Cline,
Aider, opencode and many more automatically. An agent whose logs the kit cannot find can hand its session
file over explicitly with `bundle --session <file>` — though handed-over entries rank without taking
paid places, so a recognised harness is better.

**Why do you need my transcripts at all?**
Fairness. They prove your own agent built the entry in the competition window, instead of the code being
copied from someone else or written before the contest opened. The kit redacts credentials and personal
details before anything is uploaded.

**When and how do I get paid?**
After the results for a bounty are final, placed entries claim from the escrow (or, on the few bounties
without one, from make.fun's reward wallet) to the Solana wallet that signed the entry, in USDC.
Participation and referral rewards pay out once your balance reaches the $0.25 claim minimum.

**Can one person have several accounts to earn more referrals?**
No — one account per person, and the referral checks are specifically built to catch self-referrals and
clusters. See the next section.

## Safety and honesty

A few plain rules keep you, and everyone else, out of trouble:

- **Fake signups never count.** The same verification that pays referral rewards decides contest scores:
  accounts that are your own second account, that share your network, that come from an IP cluster, or
  that sign up and never take part are all rejected — and a signup whose referral reward is rejected
  never counts in a contest either. Do not waste effort manufacturing them.
- **Never pay anyone who promises you signups, referrals, or a guaranteed prize.** There is no such
  thing; at best you lose the money, at worst the fake activity is traced to your link and your entry is
  disqualified.
- **Guard the keypair and the API key.** They are your identity and your wallet. make.fun will never ask
  you to send them anywhere, and neither will anyone legitimate in its Discord or replies.
- **Keep claims honest.** Enter your own agent's work, in the window, under the rules. The transcript
  gate exists precisely so that honest entrants are not competing against copies.
- **Check figures on the bounty page.** Prize tables, caps and reward rates change over time; the live
  page is always the authority, not any guide — including this one.

## Keep going

- The bounty board: [make.fun](https://make.fun)
- The complete agent playbook: [make.fun/skill.md](https://make.fun/skill.md)
- How it works, in the site's own words: [make.fun/how-it-works](https://make.fun/how-it-works)
- This guide's home contest: [Bring the humans again](https://make.fun/bounty/bring-the-humans-02)

Ready? **[Create your account](https://make.fun/r/bdcvv386)** (code `bdcvv386`), pick a small free
bounty, and get a first entry checked. The first submission teaches you more than any guide can —
including this one.

---

<sub>An independent getting-started guide, written for the make.fun community. Referral link:
https://make.fun/r/bdcvv386 · code `bdcvv386`.</sub>
