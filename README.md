# Laconic — AI Output Quality Constraint

<p align="center">
  <b>One skill. Five guardrails. Zero fluff.</b>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/license-MIT-blue" alt="MIT License">
  <img src="https://img.shields.io/badge/version-1.0.0-black" alt="v1.0.0">
  <img src="https://img.shields.io/badge/platform-Claude%20Code-orange" alt="Claude Code">
  <img src="https://img.shields.io/badge/lines-114-lightgrey" alt="114 lines">
</p>

---

**Laconic** eliminates the 5 most common AI output defects in a single pass:

| Defect | Example | Fixed |
|---|---|---|
| **Scope Creep** | "加个重试按钮" → full error framework | ✅ Phase 1 |
| **Style Drift** | 3 files, 3 naming conventions | ✅ Phase 2 |
| **Confirmation Echo** | "好的，让我帮你..." every reply | ✅ Phase 3 |
| **Code Bloat** | 40% unnecessary abstraction | ✅ Phase 4 |
| **Hedging Filler** | "可能/一般来说/我认为..." | ✅ Phase 3 |

---

## Quick Install

```bash
cp skills/laconic.md ~/.claude/skills/
```

Done. Next AI reply — zero fluff.

---

## How It Works

```
Input → Phase 1: Scope Lock   → What to build
     → Phase 2: Style Anchor  → How to format
     → Phase 3: Language Trim → How to express
     → Phase 4: Minimal Code  → Build it
     → Phase 5: Self-Check    → Verify
     → Output
```

All checks before a single line of code. Not "try harder" — **hard gates**.

---

## Before / After

<details>
<summary><b>Click to expand — real comparison</b></summary>

### Without Laconic

> "好的，让我帮你创建一个重试按钮。首先我需要了解当前的错误处理架构，然后我们可以添加一个完整的重试机制，包括指数退避、最大重试次数配置、以及用户友好的错误提示。让我先查看一下相关代码..."
>
> "为了更好的用户体验，我建议同时添加网络状态检测、离线队列、自动重连机制。"
>
> Output: 200 lines, RetryPolicy ABC, RetryConfig dataclass, RetryManager, RetryButton component...

### With Laconic

```python
# src/retry.py — 重试按钮 | Style: STYLE_ANCHOR.md | 行数: 12 | 函数: 1

def retry(fn, max_attempts=3):
    for attempt in range(1, max_attempts + 1):
        try:
            return fn()
        except ConnectionError:
            if attempt == max_attempts:
                raise
```

</details>

---

## File

```
laconic-ai/
├── skills/laconic.md   ← The skill. Copy this.
├── LICENSE
└── README.md
```

That's it. One file to fix five problems.

---

## vs Superpowers

| | Superpowers | Laconic |
|---|---|---|
| What it does | Engineering **process** (TDD, review, worktrees) | Output **quality** (no bloat, no drift, no fluff) |
| Skills count | 14 | 1 |
| Best for | Teams, large projects, PR workflow | **Every** AI conversation |
| They stack? | Yes — complementary, zero overlap | |

---

## Design

| Rule | Mechanism |
|---|---|
| Block before generate | Phase 1-3 gates. Fail → no code written |
| Process over prompt | 5 steps with explicit I/O. Not "be better" |
| One file, zero conflicts | Merged pipeline. No cross-skill orchestration |
| Works anywhere | Single `.md`, drop into Claude Code / Cursor / system prompt |

---

## License

MIT — [LICENSE](LICENSE)
