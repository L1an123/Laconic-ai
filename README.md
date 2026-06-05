# Laconic — AI Output Quality Constraint

> One skill. Five guardrails. Zero fluff.

**Laconic** is a single Claude Code skill that eliminates the 5 most common AI output defects in one pass: scope creep, style drift, confirmation echoes, code over-engineering, and hedging filler.

---

## Problem

AI coding assistants suffer from 5 systemic defects:

| Defect | Baseline Rate | After Laconic |
|---|---|---|
| Scope creep ("加个重试按钮" → full error handling refactor) | ~70% | Blocked at Phase 1 |
| Style drift (snake_case → camelCase → no comments, 3 files 3 styles) | >5 turns | Locked at Phase 2 |
| Confirmation echo ("好的，让我帮你...") | ~30% preamble | Removed at Phase 3 |
| Code over-engineering (40% unnecessary abstraction) | ~40% | Trimmed at Phase 4 |
| Hedging filler ("可能/一般来说/我认为...") | Pervasive | Filtered at Phase 3 |

---

## How It Works

5 sequential phases, executed before every code output:

```
Phase 1: Scope Lock     →  What to build (explicit requirements only)
Phase 2: Style Anchor   →  How to format (auto-detect + lock project style)
Phase 3: Language Trim  →  How to express (zero confirmation, zero hedging)
Phase 4: Minimal Code   →  Build it (least lines, zero abstraction creep)
Phase 5: Self-Check     →  Verify nothing extra survived
```

---

## Installation

### Claude Code

Drop `skills/laconic.md` into your project's `.claude/skills/` directory:

```bash
cp skills/laconic.md /your-project/.claude/skills/
```

Or install globally:

```bash
cp skills/laconic.md ~/.claude/skills/
```

### Any LLM

Copy the content of `laconic.md` into your system prompt or custom instructions.

---

## Before / After

### Without Laconic

```
好的，让我帮你创建一个重试按钮。首先我需要了解当前的错误处理架构，
然后我们可以添加一个完整的重试机制，包括指数退避、最大重试次数配置、
以及用户友好的错误提示。让我先查看一下相关代码...

为了更好的用户体验，我建议同时添加：
- 网络状态检测
- 离线队列
- 自动重连机制

以下是实现代码：[200 lines with abstract RetryPolicy class, RetryConfig dataclass, 
RetryManager, RetryButton component...]
```

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

---

## File Structure

```
laconic-ai/
├── skills/
│   └── laconic.md    # The skill (~120 lines)
├── LICENSE            # MIT
└── README.md          # This file
```

---

## Design Principles

| Principle | Implementation |
|---|---|
| Process over prompt | 5 phases with explicit I/O, not "try harder" |
| Block before generate | All checks at Phase 1-3, before a single line of code |
| One file, no conflicts | No cross-skill orchestration needed |
| Works standalone | Single .md file, drop into any project |

---

## Credits

Inspired by the observation that AI outputs are systematically bloated across 5 independent dimensions. Each dimension can be constrained with a tight prompt, but they must work together in a single pass — hence the merged 5-phase pipeline.

Built with the [CLAUDE.md prompt engineering framework](https://github.com/anthropics/claude-code).

---

## License

MIT — see [LICENSE](LICENSE).
