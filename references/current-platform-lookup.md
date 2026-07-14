# Current Roblox Platform Lookup

Use this reference when an API, property, MCP capability, beta feature, limit, deprecation, or platform policy may have changed.

## Source Priority

1. Roblox Creator documentation
2. Roblox Engine API reference
3. Luau documentation
4. Official Roblox release notes
5. Maintainer documentation for a named third-party library

## Rules

- Do not invent class names, properties, methods, events, or enum values.
- Look up uncertain APIs before writing implementation code.
- Separate Roblox Engine APIs from Open Cloud APIs.
- Check deprecation status when modifying older projects.
- Treat hardcoded limits, platform statistics, policy dates, and rollout status as time-sensitive.
- When documentation and an existing project disagree, report the conflict before changing working code.
- Prefer official per-page markdown or API reference pages when available.

## Lookup Targets

- Creator docs index: `https://create.roblox.com/docs/llms.txt`
- Engine API index: `https://create.roblox.com/docs/reference/engine/llms.txt`
- Open Cloud API index: `https://create.roblox.com/docs/cloud/llms.txt`
- Studio MCP docs: `https://create.roblox.com/docs/studio/mcp`
- Luau docs: `https://luau.org`

## Implementation Behavior

1. Search or read the relevant official page.
2. Confirm exact spelling and availability before coding.
3. If a feature is beta-gated, document the required Studio setting and provide a fallback.
4. If an API changed, include the migration impact in plain language.
5. If no authoritative answer is found, ask the user before writing fragile code.
