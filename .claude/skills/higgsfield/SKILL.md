---
name: higgsfield
description: Generate images and videos through Higgsfield's MCP — one connector that gives Claude access to 30+ models (Soul, Flux, Kling, Seedance, Cinema Studio, Nano Banana, GPT Image 2, etc.). Use this skill when the user wants to generate visual content via Higgsfield. Triggers on "higgsfield", "use higgsfield to ___", or any image/video generation request when the user has an active Higgsfield subscription.
---

# Higgsfield

One MCP connection, 30+ image and video models, all billed against the user's existing Higgsfield credits. No per-provider API keys.

## When to use this skill

Use this skill any time the user wants to generate images or videos through Higgsfield, or asks Claude to use a model in Higgsfield's catalogue. The skill assumes the user has an active Higgsfield subscription — if their auth fails, prompt them to check their account.

## Setup

**Claude Code (terminal):**

```
claude mcp add --transport http --scope user higgsfield https://mcp.higgsfield.ai/mcp
```

Then run `/mcp` and complete OAuth in the browser tab that opens.

**Claude Desktop:**

Settings → Connectors → Add custom connector → name it `Higgsfield` → paste `https://mcp.higgsfield.ai/mcp` → Connect → sign in with the account that owns the subscription.

If MCP tools don't appear after install, restart your Claude session.

If generation calls fail silently after a clean auth, check whether your Higgsfield workspace needs to be selected — refer to Higgsfield's own docs at higgsfield.ai for the latest setup notes.

## The cost approval rule

Higgsfield bills in credits. Credits cost real money. Before any image or video generation, do this every time:

1. State the model + key settings (resolution, duration, count).
2. Tell the user the rough credit cost — verify in their dashboard if you're unsure.
3. Wait for explicit confirmation before firing.

Auto mode does NOT override this. Burning credits silently is the fastest way to lose user trust on a paid MCP.

## Common workflows

### One-off image

> "Use higgsfield to generate a hero image for my landing page — modern, minimal, AI-themed."

Pick the model that fits the brief. For UI / marketing visuals, GPT Image 2 or Nano Banana are usually the right call. Confirm cost first.

### Multiple variants in parallel

> "Generate 3 thumbnail variants for my video — same visual style, different headline focus."

Fire the calls in parallel. Each returns a job that you poll until ready. Confirm total credit cost upfront.

### Image to video

> "Animate the still I just generated with Kling via higgsfield."

Pass the previous generation as a reference rather than re-uploading. Saves time and keeps the visual anchor consistent.

### Branded asset pack

> "Use higgsfield to draft a brand book for [logo] — colour palette, typography, sample lockups."

Multi-step: generate, review, iterate. Confirm credit cost on each pass — don't compound surprises.

## Failure handling

- **Generic "something went wrong"** — usually means the model isn't on the user's plan tier. Check the plan in the dashboard before debugging the prompt.
- **OAuth lapsed / subscription expired** — re-run `/mcp` and reconnect. Auth breaks when subscriptions lapse.
- **Content filter rejection** — change the prompt or model. Don't loop the same input.
- **Don't fall back from reference-based to text-based generation silently** — if the reference fails, surface it. Losing the visual anchor changes the deliverable.

## Models exposed

The live catalogue is on Higgsfield's site. Snapshot of what's typically available:

- **Image**: Nano Banana, GPT Image 2, Flux, Seedream
- **Video**: Seedance, Sora, Veo, Kling, Hailuo
- **Higgsfield-exclusive**: Soul (character consistency across generations), Cinema Studio (cinematic output)

Run `models_explore` from within Claude during a session to see what's live for the connected account.

## Heads-up on costs

The MCP currently doesn't return per-call credit cost in its response, so the cost-approval rule above relies on the user verifying in their Higgsfield dashboard after a few test runs to learn what each model costs them on their plan. Costs and plans change — verify before locking workflows in.
