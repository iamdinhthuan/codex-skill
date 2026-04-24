---
name: documentation-lookup
description: Use up-to-date official library and framework docs instead of training data. Activates for setup questions, API references, code examples, or when the user names a framework (e.g. React, Next.js, Prisma).
origin: upstream
---

# Documentation Lookup

When the user asks about libraries, frameworks, or APIs, fetch current official documentation using the available docs MCP, local docs, or web browsing instead of relying on training data.

## Core Concepts

- **Docs MCPs**: Use configured documentation MCPs when available.
- **Official docs**: Prefer primary project documentation, release notes, RFCs, and API references.
- **Fallback browsing**: If no docs MCP exists, browse official sources and cite them in the final answer when links matter.

## When to use

Activate when the user:

- Asks setup or configuration questions (e.g. "How do I configure Next.js middleware?")
- Requests code that depends on a library ("Write a Prisma query for...")
- Needs API or reference information ("What are the Supabase auth methods?")
- Mentions specific frameworks or libraries (React, Vue, Svelte, Express, Tailwind, Prisma, Supabase, etc.)

Use this skill whenever the request depends on accurate, up-to-date behavior of a library, framework, or API.

## How it works

### Step 1: Identify the Primary Source

Identify the library, framework, product, and version from the user's question and the local project files. Prefer `package.json`, lockfiles, imports, config files, or existing docs before browsing.

### Step 2: Select the Best Match

Choose documentation using:

- **Name match**: Prefer exact or closest match to what the user asked for.
- **Source reputation**: Prefer official or primary sources over blogs and examples.
- **Version**: If the user specified a version (e.g. "React 19", "Next.js 15"), prefer a version-specific library ID if listed (e.g. `/org/project/v1.2.0`).

### Step 3: Fetch the Documentation

Use the best available source: official docs MCP, local docs, official website, release notes, or source repository. Keep queries specific. If the answer remains unclear after checking primary sources, state the uncertainty instead of guessing.

### Step 4: Use the Documentation

- Answer the user's question using the fetched, current information.
- Include relevant code examples from the docs when helpful.
- Cite the library or version when it matters (e.g. "In Next.js 15...").

## Examples

### Example: Next.js middleware

1. Identify the installed Next.js version from project files if available.
2. Fetch the official Next.js middleware docs or release notes for that version.
3. Use the current docs to answer; include a minimal `middleware.ts` example if relevant.

### Example: Prisma query

1. Identify the installed Prisma version from project files if available.
2. Fetch official Prisma Client docs for relation queries.
3. Return the Prisma Client pattern (e.g. `include` or `select`) with a short code snippet.

### Example: Supabase auth methods

1. Identify the Supabase SDK version from project files if available.
2. Fetch official Supabase auth docs.
3. Summarize the auth methods and show minimal examples from the fetched docs.

## Best Practices

- **Be specific**: Use the user's full question as the query where possible for better relevance.
- **Version awareness**: When users mention versions, use version-specific library IDs from the resolve step when available.
- **Prefer official sources**: When multiple matches exist, prefer official or primary packages over community forks.
- **No sensitive data**: Redact API keys, passwords, tokens, and other secrets from any external query. Treat the user's question and local files as potentially containing secrets before sending content to external docs tools or browsing.
