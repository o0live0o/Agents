---
name: git-commit
description: Conventional Commits 1.0.0 commit message specification covering format, types, breaking changes, and semantic version mapping. Use when writing or reviewing commit messages or deciding a version bump (PATCH/MINOR/MAJOR).
---

# Conventional Commits

## Message structure

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

- Start with a type, a noun such as `feat` or `fix`, then the optional scope, the optional `!`, and the required colon plus space.
- The description immediately follows the colon and space and gives a short summary of the change.
- Separate the body from the description with one blank line; it is free-form and may contain any number of paragraphs.
- Put one or more footers one blank line after the body. Each footer is a word token followed by a `: ` or ` #` separator and a value; replace whitespace in a token with `-` (`Reviewed-by`), except for `BREAKING CHANGE`.

## Types and version mapping

- `feat`: a new feature, maps to MINOR.
- `fix`: a bug fix, maps to PATCH.
- Any type carrying `!` or a `BREAKING CHANGE:` footer maps to MAJOR.
- Other types such as `build`, `chore`, `ci`, `docs`, `style`, `refactor`, `perf`, and `test` may be used; they do not change the semantic version on their own.
- When a commit conforms to more than one type, make multiple commits whenever possible.

## Breaking changes

- Every breaking change MUST be marked in the type/scope prefix or in a footer, never left implicit.
- When marking it in the prefix, put `!` immediately before the colon: `feat!:`, `feat(api)!:`.
- When marking it in a footer, use `BREAKING CHANGE: <description>`, all uppercase.
- Both forms may be used together. When `!` is used, the `BREAKING CHANGE:` footer may be omitted, and the description must convey what breaks.
- `BREAKING-CHANGE` and `BREAKING CHANGE` are equivalent footer tokens.

## Other mandatory rules

- A scope is a noun describing a section of the codebase, in parentheses, e.g. `fix(parser):`.
- Casing is insignificant except for `BREAKING CHANGE`, which must be uppercase; pick one style and stay consistent.
- A footer value may contain spaces and newlines; a new valid token plus separator marks the start of the next footer.
- Revert commits use the `revert` type and list the reverted SHAs in a `Refs:` footer.

## Examples

### Commit message with description and breaking change footer

```
feat: allow provided config object to extend other configs

BREAKING CHANGE: `extends` key in config file is now used for extending other config files
```

### Commit message with `!` to draw attention to breaking change

```
feat!: send an email to the customer when a product is shipped
```

### Commit message with scope and `!` to draw attention to breaking change

```
feat(api)!: send an email to the customer when a product is shipped
```

### Commit message with both `!` and BREAKING CHANGE footer

```
feat!: drop support for Node 6

BREAKING CHANGE: use JavaScript features not available in Node 6.
```

### Commit message with no body

```
docs: correct spelling of CHANGELOG
```

### Commit message with scope

```
feat(lang): add Polish language
```

### Commit message with multi-paragraph body and multiple footers

```
fix: prevent racing of requests

Introduce a request id and a reference to latest request. Dismiss
incoming responses other than from latest request.

Remove timeouts which were used to mitigate the racing issue but are
obsolete now.

Reviewed-by: Z
Refs: #123
```

### Revert commit

```
revert: let us never again speak of the noodle incident

Refs: 676104e, a215868
```

## Fixing mistaken commits

- Before merging or releasing a mistaken commit, edit the history with `git rebase -i`. After release, cleanup depends on the tools and processes in use.

## Defaults (not part of the specification)

The following are project preferences, not Conventional Commits rules. User or team conventions take precedence.

- Record breaking changes in a `BREAKING CHANGE:` footer by default; use `!` only when the user asks for it.
- Use these types by default: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`.
