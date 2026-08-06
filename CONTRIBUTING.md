# Contributing

Thanks for helping improve this collection. Corrections, deeper answers, and new questions are all welcome.

## What makes a good contribution

The bar here is **an answer that would land well in a real interview** — not a definition copied from documentation. Concretely:

- Explain the mechanism, not just the name. "Kubernetes reconciles desired state" beats "Kubernetes is an orchestrator."
- Include a trade-off or a limitation. Answers with no downsides read as rehearsed.
- Show a real example — a command, a manifest, a snippet you have actually run.
- Say what the interviewer is likely to ask next.

## Adding or editing a question

### 1. File location and name

```
topic-slug/question-title-slug.md
```

- **No numeric prefixes** on directories or filenames — they are pure slugs.
- The slug **must** match the title: lowercase, non-alphanumerics collapsed to `-`, `&` becomes `and`. `validate_content.py` enforces this, and will tell you the expected slug if you get it wrong.
- Ordering lives in metadata, not filenames: `id` in each question's frontmatter, and `order` in `scripts/topic_meta.json` for topics.

### 2. Frontmatter

Every question file starts with:

```yaml
---
title: "What is Kubernetes?"
id: 11
category: "Kubernetes"
difficulty: "Beginner"
tags:
  - devops
  - kubernetes
  - interview-questions
---
```

- `id` must be unique across the whole repository, and must match the number in the `#` heading.
- `category` must exactly match the topic README's `title`.
- `difficulty` is one of `Beginner`, `Intermediate`, `Advanced`.
- `tags` must include `devops` and `interview-questions`, plus the topic slug.

### 3. Body structure

````markdown
# 11. What is Kubernetes?

**Short answer:** two or three sentences.

## Detail

The substance — mechanisms, trade-offs, vocabulary.

## Example

```yaml
# real, runnable, minimal
```

## Interview tips

- The follow-up questions and common traps.

---

[⬅ Back to Kubernetes](./README.md) · [All topics](../README.md)
````

The `# <id>. <title>` heading must match the frontmatter exactly.

**Do not indent prose by four spaces.** Markdown renders it as a code block; the validator rejects it.

### 4. Regenerate indexes and validate

Topic READMEs and the root README's table of contents are **generated**. Never edit them by hand — your change will be overwritten and CI will fail.

```bash
python3 scripts/generate_indexes.py     # rewrite all indexes from the question files
python3 scripts/validate_content.py     # frontmatter, naming, links, index freshness
```

Both are stdlib-only Python 3.11+. Optionally run the formatter CI also runs:

```bash
npx prettier --check "**/*.md"
```

### 5. Open the pull request

Fill in the [pull request template](./.github/pull_request_template.md). CI runs the same two scripts plus Prettier on every pull request.

## Adding a new topic

1. Create `topic-slug/` (no number).
2. Add an entry to `scripts/topic_meta.json` with an `order` (where it appears in the indexes), a `description`, and a few `study_notes` (the "What interviewers probe here" bullets). A directory that is not registered here fails validation.
3. Create `topic-slug/README.md` with frontmatter containing the topic `title` — the generator reads the display name from there, then rewrites the rest of the file.
4. Add your question files, then run the generator and validator.

## Style

- British or American spelling is fine; be consistent within a file.
- Prefer tables for comparisons, bullets for enumerations, prose for reasoning.
- Keep code examples minimal and correct. If it would not run, do not ship it.
- Pin versions in examples where the version matters.
