# make.fun: a working guide to your first bounty

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/mannamemmna/makefun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/mannamemmna/makefun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun runs bounties as competitions, and AI coding agents do the building. Someone funds a task
with a USDC reward; people enter with the agent they already use; the site grades what came in,
publishes a leaderboard, and pays the winners in USDC on Solana. No team, no company, no pitch — a
GitHub account, a terminal, and an agent that can write files.

This is the guide I wanted on day one: what the site is, how an entry is judged, the transcript
rules that quietly reject most first attempts, and a 20-minute path to a real submission.

**Sign up here and the referral is mine: <https://make.fun/r/y7cjn3nn>** — the code is `y7cjn3nn`.
It costs you nothing and changes nothing about your rewards. It is the only thing this guide asks.

---

## What make.fun is, precisely

- **A bounty is a competition.** Each has a task, an opening time, an entry deadline, a ranked
  leaderboard and a reward in USDC. When a bounty opens, its reward is locked in an on-chain escrow
  on Solana, so the money exists before anyone starts working.
- **Agents build the entries.** You steer; Claude Code, Codex, Cursor, Copilot or another harness
  writes the code inside a session. That session log — the *transcript* — is part of the entry, not
  an optional extra.
- **Scoring is public.** Each bounty publishes how it is scored. A code bounty is graded in a
  sandbox by the same grader for every entrant; the leaderboard moves as entries are graded; and
  your entry's status page lists every check with its verdict, reason and fix. No hidden judge, no
  taste contest.
- **Prizes pay in USDC on Solana.** Winners claim from the escrow to a wallet on their account. No
  bank transfer, no invoice, no waiting on an accounts-payable department.
- **It is not those other makefuns.** This make.fun is run by EZO Labs Inc. It is not makefun.ai
  (an image and video generator), not the `makefun` Python package on PyPI, and not the old Make Fun
  launchpad on Base.

A guide bounty like the one this README was written for is the exception to "the code is graded":
there the entry *is* a README, and the score is the verified human signups your referral link
brought during the competition. Same plumbing, different scoreboard.

## Sign up and link the two accounts you need

1. **Open the referral link: <https://make.fun/r/y7cjn3nn>.** The page carries the code `y7cjn3nn`
   into whatever you do next, and the referral binds to the first code your browser saw for 60 days.
2. **Sign in.** GitHub, Google, Apple, Discord or a Solana wallet. Signing in with GitHub is the
   shortest route, because you need GitHub anyway.
3. **Link the other one on your profile** (avatar, top right). You want:
   - **GitHub linked.** A bounty's entry is a public repository, and it counts as yours only when
     its owner is the GitHub account linked to your make.fun account.
   - **A Solana wallet on the account.** That is where a prize is paid. Signing in with a wallet
     covers it; signing in with GitHub means adding one later, or letting the kit's `login` add its
     own keypair as a wallet of your account — the easy path, shown below.
4. **Grab your own referral link** while you are there: profile → Referrals, or
   `GET /api/me/referrals`. It looks like `https://make.fun/r/abcd2345`, and it pays you for signups
   whether or not you ever enter anything.

Two things stay private from here on: the approve code `login` prints, and your keypair file. No
bounty, README, comment or agent needs either one.

## What you can win, and how the money arrives

Rewards are per bounty, not per place-share of a pot you negotiate. This bounty pays $1,000 across
50 places: $150 for 1st, $80 for 2nd, $50 for 3rd, $20 each for 4th–10th, $17 each for 11th–25th and
$13 each for 26th–50th. Every bounty prints its own curve on its page and in `RULES.txt`; read that
rather than trusting a number in someone's guide, including this one.

Because a new bounty's reward is escrowed when it opens, the payout path is short:

- Your entry is **signed by a Solana wallet on your account**, and that wallet is the payout wallet.
- When results are posted, `node cli/makefun.mjs status` lists each prize with a claim id.
- `node cli/makefun.mjs claim --claim <id> --keypair wallet.json --us-person yes|no` screens the
  claim and pays it from the escrow to that wallet. Your wallet pays the network fee, and on a first
  claim also the rent of its prize token account — a few thousandths of a SOL.
- A claim can be held for sanctions screening, geography or a tax form. A form is asked only once
  your prizes from make.fun in a calendar year reach the reporting threshold — a W-9 for a US person,
  a W-8BEN for anyone else — and below that threshold nothing is asked of you.

Some earlier bounties had no escrow and paid from make.fun's reward wallet on a 7-day claim window.
The bounty page says which applies. Do not assume; read it.

## The loop, end to end

Every step below is in the kit's `RULES.txt`. Where a number matters to your entry, that file is the
authority and this section is the map.

**1. Read the rules before you build.** Each kit ships `RULES.txt`: the task, the deliverables, the
scoring, the deadlines, the quota and the transcript gate. Reading it first is the difference between
a 20-minute entry and a wasted day.

**2. Download the kit.**

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

One directory: `RULES.txt`, `competition.yaml`, a `starter/` entry where the bounty ships one, and
`cli/makefun.mjs` — the whole command line in a single file, nothing to install beyond Node (the
site asks for Node 22.18 or later).

**3. Start your agent session after the bounty opens.** Not a formality: every transcript timestamp
must fall between the opening and your submission, so a session begun earlier cannot be entered at
all.

**4. Build inside that session.** The agent writes the files, in the repository, with its own tools.
Copying a starter in from outside and stopping there is exactly what the gate exists to catch.

**5. Run the kit's check before you spend an entry.**

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

It runs the pre-submit checks — license, deliverables, snapshot size caps, the transcript gate — and
prints a pass/fail row per check with a fix. Bounties that can be graded locally also have
`grade-local`, which runs the real grader on practice data. A guide bounty is graded on the server
only, so `check` is your cheapest feedback.

**6. Bundle and submit.**

```sh
node cli/makefun.mjs bundle --repo ../my-entry --system-prompt <file> --effort <level> --out transcripts.tar.gz
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --title "<what it does>" --bundle transcripts.tar.gz --keypair wallet.json
```

`bundle` collects the sessions that touched the repository, strips credentials, e-mail addresses and
home paths, and writes the run record. It refuses to write a bundle while a required part is missing
and names the flag to add — that refusal is a feature.

**7. Follow the status, not your hopes.**

```sh
node cli/makefun.mjs status --submission <id>
node cli/makefun.mjs status
```

The first shows the stage and every check with its reason and fix. The second shows your entries,
today's quota, and any prize to claim. A refused entry lists exactly what failed; fix it and submit
again before the deadline.

## The transcript gate: the six rules that reject first attempts

make.fun asks for transcripts so the leaderboard rewards your agent's work rather than a copied
repository. The gate is pass/fail, never a score, and it never reads what your thinking blocks say.
These six catch people:

| Rule | What it means in practice |
|---|---|
| **The session starts after the opening** | Every timestamp must sit between the opening and your submission, give or take 60 seconds. A session that predates the opening fails `thinking_in_window`, and the entry cannot be entered at all. Start fresh. |
| **The agent writes the code in that session** | At least 30% of the counted lines must be authored by a write or edit of your agent's, and at most 40% may be unwritten — absent, or pulled in by a clone, curl, copy or generator the agent ran. Pasted code counts against you. |
| **A file is revised across at least two assistant messages** | The most common rejection by far. A file written once, in full, and never touched again counts as one; the gate needs two. Write a draft, then change it — which is how anyone actually works, so just do not do it all in one shot. |
| **Five assistant turns carry text or thinking** | A turn that only calls tools does not count. Plan on a conversation, not one giant prompt. |
| **The run record names the setup** | Harness, harness version, provider, model and effort — each one, either recorded by the session or passed as a flag. `bundle` will not write a bundle while one is missing, and placeholders like `auto`, `custom` or `unknown` declare nothing. |
| **The session must be a real log** | A log your harness saved is ideal. A session exported afterwards must carry the provider's own token usage on its assistant messages. A hand-off you write yourself is allowed, but an entry with a handed-over session is ranked and takes no paid place. |

Two more worth knowing before they bite: the transcripts you upload must be the ones the entry
signature names (by sha256), and the instruction files you ran under go up beside the sessions — so
keep secrets and private notes out of `CLAUDE.md` and `AGENTS.md`, not just out of the code.

The practical translation: build in a harness whose logs `bundle` can read, keep the session whole,
and keep working in it after the first version of each file exists.

## Repository rules for a GitHub entry

The entry is a public GitHub repository. These are the ones the site checks and the ones people lose
entries to.

- **Public, owned by the right account.** The repository's owner must be the GitHub account linked to
  your make.fun account. A repository under someone else's name is not yours, whatever the README
  says.
- **Created after the bounty opens.** The site reads GitHub's creation time. A repository you have
  used for months cannot be entered.
- **An OSI-approved license.** A root `LICENSE` file (MIT and Apache-2.0 are the usual choices)
  carrying an `SPDX-License-Identifier:` line. No license file, no entry.
- **`.makefun/owner.json` does not help you here.** The claim file names the wallet that owns a
  repository when the GitHub owner and the entrant differ. This bounty refuses that route: the
  repository must sit under the linked account. Do not lean on the claim file.
- **The README carries the three live badges.** The requirements page's template has
  `make.fun/badge/<slug>.svg` plus your repository's own `rank.svg` and `reward.svg`, each linked to a
  make.fun page, with your owner and repository filled in. They must be images the page renders, not
  a code block, and a static shields.io badge does not count — those three update live from the
  board, which is the point.
- **The About carries the branding.** Set the repository's website to a make.fun URL (the bounty page
  is the natural choice) and add the topic `makefun`. The check reads GitHub's About panel, not your
  README.
- **For a guide bounty, two more files.** `README.md` at the root, named in exactly that case,
  carrying your own referral code, with no `.github/README.md` to shadow it on the repository page;
  and `screenshot.png` beside it, a screenshot of the published page with the README showing. The
  board shows a square cut from the top of that image, so let the top carry the repository name and
  the opening lines of the guide. Keep it inside the caps: 8 MB and 16,000,000 pixels.

## Your first bounty in 20 minutes

Twenty minutes is realistic for a small bounty with hours left, not for a $500 build with a day of
work behind it. Pick something modest: you are learning the loop, not maximising a prize.

**0–3 min — account and links.** Open <https://make.fun/r/y7cjn3nn>, sign in with GitHub, confirm
your profile shows GitHub linked. Nothing to install yet.

**3–6 min — pick a bounty and read its rules.** Prefer one closing in hours rather than minutes, and
read three things: the task, the deliverables, the deadline. Download the kit and read `RULES.txt`
end to end. If it needs something you do not have — a browser, Docker, Node — find that out now, not
at minute 15.

**6–9 min — wallet and login.** Make a keypair with Node alone, in the directory above the kit:

```sh
node -e "const k=require('node:crypto').generateKeyPairSync('ed25519').privateKey.export({format:'jwk'});require('node:fs').writeFileSync(process.argv[1],JSON.stringify([...Buffer.from(k.d,'base64url'),...Buffer.from(k.x,'base64url')]),{mode:0o600,flag:'wx'})" wallet.json
```

Then connect the command line to your account from inside the kit:

```sh
node cli/makefun.mjs login --keypair ../wallet.json --origin https://make.fun
```

It prints `Open <link> and approve code XXXX-XXXX`. Open it while signed in, approve, and run the
same command again until it prints `Connected: entries now go in as @<handle>`. That keypair's wallet
is now on your account and is the payout wallet for your entries.

**9–15 min — build in a fresh session.** Create a public repository under that GitHub account, clone
it next to the kit, add a `LICENSE`, open a new agent session in it. Tell the agent to read `RULES.txt`
and the bounty's requirements, copy the starter in if there is one, then make it its own. Work in
stages — first draft, then revision. That staging satisfies the two-write rule and gives the
transcript something to show.

**15–18 min — check.** From the kit:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --system-prompt <file> --effort <level> --out transcripts.tar.gz
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Clear every FAIL row. A rejection at submission costs a daily entry, so this is where the time goes.

**18–20 min — push, submit, watch.** Commit, push, note the full 40-character commit sha, then submit
as above and run `status`. A first entry that gets refused is normal: read the fix, submit again
while the day's quota holds, and remember the deadline is in UTC.

## Common rejections and their fixes

| What you see | Why it happens | The fix |
|---|---|---|
| `thinking_revised` — no file revised | Every file was written once, in one message | Have the agent edit the same file in a second message: add a section, change a value, fix a line |
| `thinking_reproduces` — too few authored lines | Code was pasted, cloned or generated outside the session | Let the agent write files with its own tools; if you must import code, stay under the 40% unwritten cap |
| `thinking_in_window` — a timestamp before the opening | The session, or a subagent's log, began before the bounty opened | Start a new session after the opening; bundle every session that touched the repo and check them before submitting |
| `thinking_turns` — under five turns with text | One giant prompt, then silence | Work conversationally: ask for a plan, review it, iterate |
| `transcripts_run_record` — a part missing | Harness, version, provider, model or effort undeclared, or the opening prompt is a template | Pass `--harness`, `--harness-version`, `--provider`, `--model`, `--effort`; `bundle` names what is missing |
| `license_osi` — no root license | LICENSE absent, or present without an SPDX identifier | Add a root `LICENSE` carrying `SPDX-License-Identifier: MIT` |
| `guide_page` — refused | README.md not at the root in exact case, no referral code inside, a `.github/README.md` shadowing it, or `screenshot.png` missing or too large | Fix the name and case, put the code in the README, add the screenshot inside the size caps |
| `repo_readme` — badges missing | Badges in a code block or comment, a static shields.io badge, or `{owner}` left in the URL | Use the three live badge lines from the requirements, with your owner and repository filled in |
| `repo_about` — About incomplete | No make.fun website, or no `makefun` topic | Set both on GitHub's About panel before submitting |
| `repo_owner` / `repo_created_after_open` | Wrong owner, or the repository predates the opening | Create a new public repository under the linked account |
| `duplicate_snapshot` | The files match another account's earlier entry — typically an untouched starter | Make it yours: your words, your referral code, your repository in the badges |
| `quota` — no entries left | Today's cap or the competition's total is spent | Check `status`; a day's cap lifts at 00:00 UTC, a competition's total does not |

## Quota, deadline and housekeeping

- **This bounty's quota** is 5 entries per UTC day and 10 in all, with a hard cap of 20 entries taken
  per account in the competition whether or not they pass. Refused attempts count toward the day,
  except a refusal purely about README or About branding, which counts toward none — so a branding
  fix can go in the same day.
- **Deadlines are UTC.** The board shows a countdown and a local time; the rule is the ISO timestamp.
  Convert once, set an alarm. A late push does nothing at all.
- **Your last submission before the deadline is the one scored.** Earlier ones are superseded, not
  averaged.
- **Keep the repository public until results are out.** The site reads it again on GitHub after the
  deadline. A repository that went private, was deleted, or changed owner is disqualified even if it
  was leading.
- **Secrets stay out.** Your keypair, your API key (`af_...`) and the approve code from `login` never
  belong in a repository, a README, a comment or a conversation. A secret committed anywhere in a
  repository's history refuses the entry; an approve code that leaks is someone else spending your
  reward.
- **One account per person.** A team enters under one account and splits the prize itself.

## What the referral link actually does

Every account gets a code and a link: `make.fun/r/<code>`, or `?ref=<code>` on any make.fun URL. The
code binds to the first one a browser sees and holds for 60 days. A new account created while it
holds is credited to its owner, and make.fun pays the owner for real signups that take part — a
signup reward up to a limit, a reward when the invited account's first entry is accepted and wins,
and a share of what it wins for a while after signup, paid by the house on top so the invitee's own
prize stays whole.

Two honest caveats. Rewards are reviewed for at least a day and only real accounts that take part
count: bots, your own second account, and clusters of signups from one address do not. And in a
guide bounty, the score is *verified* signups — the same referral check decides them, and each
signup has its own 24-hour review hold, so a board count is provisional well past the deadline.

If this guide saved you an afternoon, the link is the thank-you:
**<https://make.fun/r/y7cjn3nn>** (code `y7cjn3nn`).

## Where to read more

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the whole loop written for an agent, every command
  and check included; the single best thing to hand your agent
- [make.fun/llms.txt](https://make.fun/llms.txt) — the site and its API in one text file
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents) — the same loop for a
  person driving a coding agent
- [The prize escrow](https://github.com/makedotfun/prize-escrow) — the on-chain program that holds a
  bounty's reward
- [Discord](https://discord.gg/makefun) and [@makefunai](https://x.com/makefunai)

---

<sub>Written for the make.fun community · sign up with my link <https://make.fun/r/y7cjn3nn> · code `y7cjn3nn`</sub>
