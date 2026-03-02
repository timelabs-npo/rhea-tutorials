# Lesson 01: Your First Tribunal

## The problem

Ask Claude: "Does aspirin inhibit COX-2?"
Ask GPT: "Does aspirin inhibit COX-2?"
Ask Gemini: "Does aspirin inhibit COX-2?"

You'll get three confident answers. They might agree. They might not. **You have no way to know which confidence is warranted without asking all of them.**

That's what a tribunal is — forcing the question across multiple models and measuring what happens.

## What you'll build

A 30-line Python script that:
1. Sends the same claim to 3 models
2. Collects their responses
3. Prints them side by side

No scoring yet. No consensus. Just: **look at the disagreement.**

## Code

```python
"""tribunal_basic.py — Your first multi-model query."""
import os, json, httpx

CLAIM = "Aspirin selectively inhibits COX-2 at therapeutic doses"

MODELS = [
    {"name": "gemini-2.0-flash", "provider": "google",
     "url": "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent",
     "key_env": "GEMINI_API_KEY"},
]

def ask_model(model: dict, claim: str) -> str:
    key = os.environ.get(model["key_env"], "")
    if not key:
        return f"[{model['name']}] No API key set ({model['key_env']})"

    if model["provider"] == "google":
        r = httpx.post(
            f"{model['url']}?key={key}",
            json={"contents": [{"parts": [{"text": f"Evaluate this scientific claim. Be specific about what's correct and what's misleading:\n\n{claim}"}]}]},
            timeout=30,
        )
        data = r.json()
        return data["candidates"][0]["content"]["parts"][0]["text"]

    return f"[{model['name']}] Provider not implemented yet"


if __name__ == "__main__":
    print(f"CLAIM: {CLAIM}\n{'='*60}\n")
    for model in MODELS:
        print(f"--- {model['name']} ---")
        response = ask_model(model, CLAIM)
        print(response[:500])
        print()
```

## Run it

```bash
export GEMINI_API_KEY="your-key-here"  # Free at aistudio.google.com
python tribunal_basic.py
```

## What you'll notice

Gemini will likely tell you aspirin is **non-selective** — it inhibits both COX-1 and COX-2. The claim is deliberately misleading. A single model catches it because this is well-documented biochemistry.

But what happens when the claim is *almost* right? Or when it's in a domain where models disagree? That's lesson 02.

## Key insight

> A tribunal isn't about getting the right answer. It's about **seeing the disagreement surface** so you can decide what to investigate further.

The value isn't in the consensus. It's in the divergence.

---

Next: [Lesson 02 — Consensus Scoring](02-consensus.md)
