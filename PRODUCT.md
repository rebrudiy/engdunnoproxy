# PromptProxy

## What We Want

### 1. Every prompt goes through the proxy
- Every prompt in every Claude Code session intercepted automatically
- No manual activation, no per-project setup — it just works
- Implementation: **Claude Code Plugin** with a global `UserPromptSubmit` hook
- Users install with one command: `claude plugin add github:USER/promptproxy`
- Plugin auto-registers the hook — zero manual configuration

### 2. Two features, one product, one model call

**Feature A: Grammar fixer**
- Corrects grammar, injects clean version as additionalContext
- Every correction logged to `~/.promptproxy/history.json` (original, fix, explanation)
- User reviews on their schedule with `/promptproxy:lessons`

**Feature B: Prompt rewriter (the killer feature)**
- Turns messy prompts into clean, specific, best-practice prompts
- "fix bug button not work" → "Fix the click handler in Button.tsx — onClick doesn't fire"
- This is what people share and what differentiates us from a grammar checker

Both features are toggleable in config independently:
- `grammar: on, rewriter: on` — do both (default)
- `grammar: on, rewriter: off` — just fix my English
- `grammar: off, rewriter: on` — my English is fine, improve my prompts
- `grammar: off, rewriter: off` — proxy disabled

One model call handles both. One hook. One plugin.

### 3. Rewriter modes

| Mode | What happens |
|------|-------------|
| **off** | Rewriter disabled, grammar only |
| **suggest** | Show rewritten prompt in conversation, user can ignore it |
| **questions** | Proxy asks user to clarify vague parts before sending to Claude |
| **automatic** | Proxy rewrites the prompt and injects it as context, user sees nothing |

Default: **suggest**
Names TBD — might rename these later

### 4. Grammar modes

| Mode | What happens |
|------|-------------|
| **off** | Grammar disabled |
| **silent** | Fix and inject as context, user sees nothing |
| **visible** | Show what was fixed in the conversation |
| **strict** | Block prompt until user fixes it themselves (with red-pen guidance) |

Default: **silent**

### 5. Vocabulary leveling (needs design)
- Proxy upgrades your vocabulary to push you one level up
- Config param: english level — exact scale TBD (CEFR A1-C2 vs simple low/medium/high)
- Replaces simple words with level-appropriate alternatives
- New words get logged to learning journal so you actually learn them
- Open questions: how granular? how aggressive? which modes?

## Nice to Have (Later)

- Filtering: turn proxy on/off for specific sessions or projects (TBD)
- Onboarding: first launch asks user their English level, maybe auto-detect
- Quality scoring: rate prompt quality with actionable suggestions
