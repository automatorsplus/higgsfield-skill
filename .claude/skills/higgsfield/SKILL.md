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

## The cost approval rule (NON-NEGOTIABLE)

Higgsfield bills in credits. Credits cost real money. Every single image or video generation must pass through this gate. No exceptions.

**The user's original request is a REQUEST, not approval.** A prompt like "use higgsfield to generate a thumbnail" is the user asking you to start the flow — it is **not** permission to fire the API call. Approval is a separate, explicit confirmation that comes AFTER you quote the cost.

### Mandatory pre-generation flow

For every `generate_image` or `generate_video` call, in this order:

1. **Call `balance` first** to fetch current credits. Always. Do not skip even if you "know" the balance from earlier in the session — it changes.
2. **Quote the cost block** in this exact shape:
   ```
   Model: <model_id>
   Settings: <aspect_ratio, resolution, count, duration if video>
   Estimated cost: ~<X> credits (range Y-Z if uncertain)
   Current balance: <from balance call>
   Balance after: <current - estimated>

   Reply "go" to fire, or tell me what to adjust.
   ```
3. **STOP. Wait.** Do not call `generate_image` / `generate_video` until the user replies with an explicit approval token: `go`, `yes`, `fire`, `proceed`, or equivalent. Anything else (silence, a follow-up question, a tweak to the prompt) is NOT approval.
4. **One approval = one generation batch.** If the user says "go" once, that authorises the single batch you quoted. A second generation needs a fresh quote and a fresh "go". Never roll multiple batches into one approval.

### Calibrated cost reference (verify in dashboard, update as you learn)

| Model | Settings | Approx credits |
|---|---|---|
| `gpt_image_2` | 2K medium | ~3 |
| `gpt_image_2` | 4K high | ~10-12 |
| `nano_banana_2` | 2K | ~10-12 |
| `nano_banana_2` | 4K | ~20-25 |
| `seedance_1_5` | 480p / 4s | ~2-3 |
| `seedance_1_5` | 720p / 4s | ~6-8 |
| `seedance_1_5` | 720p / 12s | ~18-25 |
| `seedance_2_0` (any tier) | first call | measure first |
| Higher-end video (Veo, Sora, Cinema Studio) | first call | measure first |

If you don't have a measured number for the exact model + settings, quote a range AND tell the user this is a first-run calibration — the actual cost will land in the dashboard after the call.

### Why this matters

- Silent credit burn is the #1 way to break user trust on a paid MCP.
- This skill ships to a community. If a community member's first experience is "Claude burned 50 credits without asking," they'll uninstall and tell their network. The gate is a feature, not friction.
- Auto mode does NOT override this rule. Even when the user is hands-off, the cost gate fires.

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
