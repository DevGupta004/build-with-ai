# Build With AI — Prompt Guide Book

> Real prompts. Real apps. Zero dev team required.

A growing collection of step-by-step guides to build production-ready apps using **Claude AI + Fable 5**. Each guide includes the full tech stack, copy-paste prompts, and a complete feature checklist.

No fluff. Just working code.

---

## Guides

| # | Guide | What You Build | Stack | Model |
|---|-------|---------------|-------|-------|
| 01 | [E-Commerce App — SwiftMart](guides/01-ecommerce-app.md) | React Native customer app + Node.js REST API + Next.js admin panel | RN 0.85 · Node 20 · Next.js 15 · MySQL | Fable 5 |
| 02 | [Looping in Claude Code](guides/02-looping-in-claude-code.md) | How to use `/goal`, `/loop`, `/schedule` — self-correcting AI agent systems | Claude Code | Any |

> More guides coming. Star the repo to get notified.

---

## How to Use

1. Pick a guide from the table above
2. Follow the steps **in order** — setup first, prompt second
3. Paste the prompts into **Claude Code** (`claude` in terminal or VS Code extension)
4. Say `continue` whenever Claude stops mid-file
5. That's it — Claude writes all the code

---

## What is Fable 5?

Fable 5 is Anthropic's most capable code model. It writes 500+ line files in one pass, understands full-stack architecture, and rarely stops mid-file. Always set it before pasting prompts:

```
/model → select claude-fable-5
```

Or add to `.claude/settings.json`:
```json
{ "model": "claude-fable-5" }
```

---

## About

Built and maintained by [@devgupta007](https://github.com/devgupta007).

New guides added regularly — cover more app types, more stacks.

---

*Start with Guide 01 → [Build a Full E-Commerce App](guides/01-ecommerce-app.md)*
