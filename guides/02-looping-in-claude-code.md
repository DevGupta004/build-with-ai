# Looping in Claude Code — A Short & Simple Guide

*Based on the Prime AI Field Guide by Joe Stolte*

## What's a loop?

A loop is a small team of Claude agents that finishes a job for you. It works, grades itself against your standard, fixes what's weak, and only hands you the finished thing.

You set the goal once. Then you step out of the review seat.

## Why plain prompting falls short

- **You grade the homework.** You read every answer and check it yourself. Claude won't tell you when it's wrong.
- **Answers are one and done.** Claude stops the second it replies, good or bad.
- **You turn the hand crank.** Every round of improvement is you, typing, again.

You didn't remove the work — you just moved it into a chat window.

## How a loop flips this

- **Claude grades the homework.** A separate agent checks the work against your goal and sends weak work back.
- **Claude fixes itself.** If the work fails the check, it writes its own next instruction and retries.
- **Claude runs the machine.** It keeps going until the goal is hit or a stop rule kicks in.

For bigger jobs, the work splits across agents like an assembly line: a **boss** (orchestrates), a **builder** (does the work), a **checker** (reviews and sends back problems), and a **publisher** (final review, ships it).

## The 3 commands

| Command | Use it for | Behavior |
|---|---|---|
| `/goal` | "Get this done for me" | Works until the outcome is hit, then stops on its own |
| `/loop` | "Watch this for me" | Repeats on an interval (e.g. `/loop 30m`) while your session is open |
| `/schedule` | "Run while I'm gone" | Runs in the cloud, even with your laptop closed |

To stop early: `/goal clear` for goals, or just say "stop" for loops.

## The 4 guardrails

**1. Make "done" a yes/no test.**
A loop does exactly what you said, not what you meant.
- Weak: "make it good"
- Strong: "every claim has 3 working sources" / "Lighthouse SEO score 90+"

**2. Never let the worker grade its own homework.**
An agent checking its own work will always say it passed. Use a separate checker agent with fresh eyes.

**3. Put up stop signs.**
Always cap the loop — "max 3 tries" or "stop once the goal is met." A loop with no stop condition is a faucet you left running, burning your token budget.

**4. Rulebook vs scorecard.**
Keep instructions (the skill file) separate from progress tracking (the state file). The loop reloads a clean rulebook every run — any progress notes scribbled inside it get wiped, and the loop restarts from zero. The rulebook says *how to play*; the scorecard remembers *where you are*.

## The 7 things to define before building any loop

1. **Goal** — the finish line, as a yes/no test
2. **Trigger** — what starts each run: a time, an event, or run-until-done
3. **Discovery** — where the loop looks to find the work each round
4. **Action** — what it's allowed to do, with which tools
5. **Verification** — who checks the result, and against what (never the worker itself)
6. **State** — where "done vs left" gets written down, outside the chat
7. **Human gate** — which actions must pause and ask you first (anything irreversible: sending emails to real people, spending money, going live)

## The one rule everyone gets wrong

With `/goal`, the checker agent **only reads the chat**. It can't open files or click links.

So the worker must paste its proof directly into the chat — working links, all sources, screenshots, scores. No proof in the chat = the job doesn't count as done. Build this into every loop prompt.

## How to start (1 minute)

1. Update Claude Code to the latest version
2. Open it in any folder
3. Run a safe, zero-risk `/goal` — like a one-page brief on a topic you know well, with source verification and a "max 3 revision rounds" stop
4. Watch it write, check its own work, catch its own mistakes, and fix them
5. Read the result. Notice where it was strict and where it let things slide

That one run shows you the whole machine: plan → do → check → fix → stop. Start small, then graduate to bigger builds.

## The takeaway

Prompting is asking for an answer. Looping is designing a system with a clear goal, an independent checker, a stop sign, and a human gate — so the AI does the job, grades itself, and only hands you the finished thing.

Your new job isn't writing prompts. It's writing loops.
