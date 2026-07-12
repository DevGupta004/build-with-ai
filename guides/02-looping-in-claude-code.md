# Prompting vs Looping with AI — A Short & Simple Guide

*Based on the Prime AI Field Guide by Joe Stolte*

## The difference in one line

**Prompting** = you ask, AI answers, you review, you ask again.
**Looping** = you define the goal, AI works, checks itself, fixes itself, delivers.

---

## Why plain prompting falls short

- **You grade the homework.** You read every answer and check it yourself. AI won't tell you when it's wrong.
- **Answers are one and done.** AI stops the second it replies, good or bad.
- **You turn the hand crank.** Every round of improvement is you, typing, again.

You didn't remove the work — you just moved it into a chat window.

---

## How a loop flips this

- **AI grades the homework.** A separate agent checks the work against your goal and sends weak work back.
- **AI fixes itself.** If the work fails the check, it writes its own next instruction and retries.
- **AI runs the machine.** It keeps going until the goal is hit or a stop rule kicks in.

For bigger jobs, the work splits across agents like an assembly line:
- **Boss** — orchestrates
- **Builder** — does the work
- **Checker** — reviews and sends back problems
- **Publisher** — final review, ships it

---

## The 3 modes

| Mode | Use it for | Behavior |
|---|---|---|
| **One-shot prompt** | Quick answers, drafts, lookups | Stops after one reply |
| **Goal / run-until-done** | "Get this done for me" | Works until the outcome is hit, then stops |
| **Scheduled loop** | "Watch this for me" / "Run while I'm gone" | Repeats on an interval, runs in the cloud even with laptop closed |

---

## The 4 guardrails

**1. Make "done" a yes/no test.**
A loop does exactly what you said, not what you meant.
- Weak: "make it good"
- Strong: "every claim has 3 working sources" / "all tests pass" / "Lighthouse SEO score 90+"

**2. Never let the worker grade its own homework.**
An agent checking its own work will always say it passed. Use a separate checker agent with fresh eyes.

**3. Put up stop signs.**
Always cap the loop — "max 3 tries" or "stop once the goal is met." A loop with no stop condition is a faucet you left running, burning your budget.

**4. Rulebook vs scorecard.**
Keep instructions separate from progress tracking. The loop reloads a clean rulebook every run — any progress notes scribbled inside it get wiped, and the loop restarts from zero. The rulebook says *how to play*; the scorecard remembers *where you are*.

---

## The 7 things to define before building any loop

1. **Goal** — the finish line, as a yes/no test
2. **Trigger** — what starts each run: a time, an event, or run-until-done
3. **Discovery** — where the loop looks to find the work each round
4. **Action** — what it's allowed to do, with which tools
5. **Verification** — who checks the result, and against what (never the worker itself)
6. **State** — where "done vs left" gets written down, outside the chat
7. **Human gate** — which actions must pause and ask you first (anything irreversible: sending emails to real people, spending money, going live)

---

## The one rule everyone gets wrong

The checker agent **only reads the chat**. It can't open files or click links.

So the worker must paste its proof directly into the chat — working links, all sources, screenshots, scores. No proof in the chat = the job doesn't count as done. Build this into every loop prompt.

---

## Works with any AI tool

This pattern isn't locked to one tool. The concept applies across:

| Tool | How to loop |
|---|---|
| **Claude Code** | `/goal`, `/loop`, `/schedule` commands |
| **ChatGPT** | Custom GPTs + memory + scheduled tasks |
| **Cursor / Windsurf** | Agent mode with multi-step tasks |
| **n8n / Make** | AI nodes chained with condition + loop back |
| **LangChain / CrewAI** | Multi-agent pipelines in code |
| **Zapier AI** | Actions → check → retry flows |

The tools change. The structure doesn't: **goal → build → check → fix → stop**.

---

## How to start (1 minute)

1. Open any AI tool you already use
2. Run a safe, zero-risk goal-mode task — like a one-page brief on a topic you know well, with source verification and a "max 3 revision rounds" stop
3. Watch it write, check its own work, catch its own mistakes, and fix them
4. Read the result — notice where it was strict and where it let things slide

That one run shows you the whole machine: **plan → do → check → fix → stop**.

Start small, then graduate to bigger builds.

---

## The takeaway

Prompting is asking for an answer.
Looping is designing a system — clear goal, independent checker, stop sign, human gate — so AI does the job, grades itself, and only hands you the finished thing.

**Your new job isn't writing prompts. It's writing loops.**
