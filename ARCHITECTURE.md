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
