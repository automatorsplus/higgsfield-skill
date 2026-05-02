# Higgsfield Skill for Claude Code

> Part of the **Automators+** skills library -- Claude Code skills shared exclusively with the Automators+ community.

Generate images and videos through Higgsfield's MCP. One connector gives Claude access to 30+ models -- Soul, Flux, Kling, Seedance, Cinema Studio, Nano Banana, GPT Image 2, and more -- all billed against your existing Higgsfield credits.

## What You Get

- **One MCP, 30+ models** -- no per-provider API keys to juggle. One OAuth connection, every model in the catalogue.
- **Cost-approval gate** -- Claude tells you the model + estimated credit cost before every generation. You confirm before any credits burn.
- **Workflow patterns built in** -- one-off images, parallel variants, image-to-video chains, branded asset packs. Trigger with plain English.
- **Failure handling baked in** -- generic "something went wrong" errors get diagnosed (plan tier, auth lapse, content filter) instead of looping forever.

## Prerequisites

### 1. An active Higgsfield subscription

This skill assumes you already pay for Higgsfield credits. If you don't, look at [fal.ai](https://fal.ai) or [kie.ai](https://kie.ai) for pay-as-you-go on the same models.

### 2. Claude Code or Claude Desktop

Either works. Setup is one command in the terminal or a custom connector in Desktop -- both covered in the skill.

## Install the Skill

### Option 1: URL install

```
claude skill add --url https://raw.githubusercontent.com/automatorsplus/higgsfield-skill/main/.claude/skills/higgsfield/SKILL.md
```

### Option 2: Manual copy

Copy `.claude/skills/higgsfield/` into your project's `.claude/skills/` folder.

## Set Up the MCP

After installing the skill, connect Claude to Higgsfield.

**Claude Code (terminal):**

```
claude mcp add --transport http --scope user higgsfield https://mcp.higgsfield.ai/mcp
```

Then `/mcp` to complete OAuth in your browser.

**Claude Desktop:**

Settings → Connectors → Add custom connector → name it `Higgsfield` → paste `https://mcp.higgsfield.ai/mcp` → Connect → sign in.

Restart Claude if MCP tools don't appear after install.

## Try It

Once the MCP is connected and the skill is installed:

- *"Use higgsfield to generate a hero image for my landing page -- modern, minimal, AI-themed."*
- *"Generate 3 thumbnail variants for my video, same visual style, different headline focus."*
- *"Animate that still with Kling via higgsfield."*
- *"Use higgsfield to draft a brand book for this logo -- colour palette, typography, sample lockups."*

The skill activates on any image or video generation request once Higgsfield is connected.

## How It Works

Claude reads `.claude/skills/higgsfield/SKILL.md`, sees the trigger conditions, and pulls the right model from Higgsfield's catalogue based on the brief. Before any generation, it states the model + estimated cost and waits for your go.

For the full skill definition and workflow patterns, see [`.claude/skills/higgsfield/SKILL.md`](.claude/skills/higgsfield/SKILL.md).

## License

MIT

---

*Shared with the Automators+ community*
