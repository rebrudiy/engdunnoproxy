# Architecture

## Open Questions (need research/testing)

### 1. How grammar checking works — two options to test

**Option A: Let the model decide (simple)**
- Just tell the model: "fix grammar in this text and explain what you fixed"
- Model knows grammar better than any rules we write
- We don't define what to check — model catches everything
- We only ask it to categorize fixes when reporting back (spelling, preposition, etc.)
- Pros: simpler to build, probably more accurate
- Cons: black box, less control, harder to configure per-category

**Option B: We define rules and context (structured)**
- We give the model a strong system prompt with specific categories and rules
- We tell it exactly what to look for: spelling, prepositions, verb forms, false friends, etc.
- User can toggle categories on/off in config
- Pros: full control, configurable, predictable behavior
- Cons: more work, we might limit the model by giving it bad instructions

Need to test both and compare. Maybe the answer is a mix — Option A for fixing, Option B for reporting/explaining.

### 2. Which model runs the proxy
- TBD — needs real testing, not guessing
- Candidates: Claude (Opus/Sonnet/Haiku), ChatGPT (GPT-4o/mini), local models (Ollama/llama), local rules, external APIs
- We build a benchmark: same 50 broken prompts → run through each option → compare accuracy, speed, cost
- Pick the winner based on data

### 3. English level control — how to teach at the right level

**Option A: Just prompting**
- Tell the model in the prompt: "user is level B1, explain fixes at that level, suggest vocabulary appropriate for B2"
- Pros: simple, no extra infrastructure
- Cons: model might not consistently follow the level, hard to verify

**Option B: Criteria in MCP server**
- Build an MCP server that holds structured data: vocabulary lists per level, grammar rules per level, explanation complexity per level
- Model calls the MCP tool to get the right criteria for the user's level
- Pros: structured, consistent, updatable without changing prompts
- Cons: more infrastructure, need to build and maintain vocabulary/rule sets

Need to test: does prompting alone give consistent level-appropriate results, or do we need structured data to keep it accurate?

### 4. How prompt rewriter decides "good vs bad"

Anthropic official guide says a good prompt should have:
- Specificity — clear what to do, not vague
- Output format — what the result should look like
- Context — file names, error messages, background
- Motivation — why this needs to happen
- Positive framing — "do X" not "don't do X"
- Structure — steps for complex requests
- Examples — if task is subjective

For Claude Code context, most of this is overkill. Coding prompts are short commands.
We probably only need to check: **is it specific? does it mention what file/function? is the scope clear?**

**TODO: research myself** — google prompt engineering best practices, community guides, maybe OpenAI/Google docs too. Build a real checklist of what matters for CODING prompts specifically, not general prompts.

Sources to check:
- Anthropic official: https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices
- OpenAI prompt guide
- Community best practices
- Real examples of good vs bad coding prompts
