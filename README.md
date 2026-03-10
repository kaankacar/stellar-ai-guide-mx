# Stellar Hackathon AI Guide

Mexico has one of the most active SPEI networks in the world, a massive remittance corridor with the US, and millions of people who move money through channels that are slow, expensive, or both. Stellar is built for exactly this, and this hackathon is a chance to build something that actually matters for people here.

This repo is a collection of guides put together by the SDF DevRel team to help you move fast. The goal is simple: every developer at this event should have the same shot at building something real, regardless of whether they have a paid AI subscription, years of Stellar experience, or a high-end laptop.

## Start here

**Building with Mexican peso rails?** The regional starter pack in `Hackathon_Resources.md` is the fastest path. It has Etherfuse (MXN to CETES via SPEI), AlfredPay (MXN to USDC via SPEI), and BlindPay already wired up as a portable TypeScript library you can drop into any Node project. Before you use Etherfuse, read its section in `Dev_Setup_Guide.md` -- there are several non-obvious gotchas that have cost developers hours.

**Don't have a paid AI subscription?** Start with `Free_AI_Setup.md`. Six capable open-source models, step-by-step Ollama setup, and how to connect them to Claude Code for free.

**About to write code?** Read `Dev_Setup_Guide.md` first. API keys, testnet addresses, auth patterns, and the gotchas that have blocked developers for hours. Five minutes here saves a lot of pain later.

## Suggested reading order

1. `Free_AI_Setup.md` if you need a free AI setup
2. `Dev_Setup_Guide.md` before writing any code
3. `Hackathon_Resources.md` to orient yourself in the Stellar ecosystem
4. `Claude_Code_Guide.md` if you're using Claude Code
5. `Recommended_AI_Tools.md` to explore what else is available

## What's in each file

**`Dev_Setup_Guide.md`** is the file to read before you write a single line of code. It covers how to get API keys for Etherfuse and DeFindex (neither has self-service signup, so do this early), verified testnet contract addresses, the exact auth header format for each protocol (they vary in non-obvious ways), and a section of critical gotchas pulled from 60 build runs. If something is broken and you don't know why, check Section 5 before spending more time on it.

**`Hackathon_Resources.md`** is your map of the Stellar ecosystem. It has the regional starter pack for Mexican peso rails, a full-featured DeFi reference app that shows how Blend, Soroswap, Phoenix, Aquarius, SDEX, and Reflector Oracle wire together, links to community-compiled FAQs and real build logs, ecosystem discovery tools, and videos worth watching before or during the event.

**`Claude_Code_Guide.md`** covers the Claude Code features that actually change how fast you can work: plan mode (Claude thinks through a problem before touching any code), parallel agents (multiple tasks running at the same time), and CLAUDE.md (a project file that loads your stack context automatically every session). Also has the full command reference and Stellar-specific plugin setup.

**`Recommended_AI_Tools.md`** maps the broader AI tool ecosystem. Stellar-native tools first: Stella (SDF's official AI assistant), the stellar-dev Claude Code skill, OpenZeppelin's contract generation tools, and x402 (a protocol that lets AI agents make autonomous payments via Soroban). Then coding assistants with free tiers, multi-agent frameworks for AI-native products, and rapid prototyping tools.

**`Free_AI_Setup.md`** is for anyone without a paid AI subscription. Six capable open-source models with hardware requirements, how to run them locally via Ollama, how to connect them to Claude Code for agentic coding at no cost, and how to rent a GPU server for a few dollars if your laptop can't handle local inference.

*SDF DevRel, March 2026*
