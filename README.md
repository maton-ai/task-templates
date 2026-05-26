# Maton Task Templates

Community-contributed task templates for [Maton](https://maton.ai). Each
template is a small, self-contained folder with a `SKILL.md` describing what
the task does and how to invoke it.

These templates power the Templates gallery in the Maton app. Adding a
template here makes it instantly available to every Maton user.

## Repository layout

```
<category>/<template-slug>/SKILL.md
```

For example:

```
calendly/meeting-reminder/SKILL.md
```

A template folder can also contain `references/`, `scripts/`, `assets/`, and
`tests/` alongside the `SKILL.md`.

## SKILL.md format

```markdown
---
name: my-template
description: "One-sentence summary of what this template does."
version: 1.0.0
author: Your Name
license: MIT
metadata:
  maton:
    tags: [Tag, Another-Tag]
---

# My Template

Long-form instructions, examples, prerequisites, and usage notes.
```

The frontmatter is YAML; the body is Markdown. The Maton app surfaces
`name`, `description`, `tags`, `author`, and `version` on the template card
and detail page, and renders the Markdown body inline.

## Contributing

The easiest path is the in-app "Submit a template" button, which forks this
repo to your GitHub account and opens a pull request on your behalf. You
can also contribute the old-fashioned way:

1. Fork this repo.
2. Create a new folder under the appropriate category (or propose a new
   category by adding a folder for it).
3. Write your `SKILL.md`.
4. Open a pull request.

If a similar template already exists, consider improving it instead of
adding a near-duplicate.

## License

[MIT](./LICENSE).
