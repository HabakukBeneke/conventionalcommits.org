# Conventional Commits 1.0.0 Specification - LLM Reference

## Overview

Conventional Commits is a lightweight convention for commit messages that creates an explicit commit history, enables automated tooling, and integrates with Semantic Versioning (SemVer).

## Commit Message Structure

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

## Core Types and Their SemVer Impact

### Required Types

- **`fix:`** - Bug fix (correlates to PATCH in SemVer)
- **`feat:`** - New feature (correlates to MINOR in SemVer)

### Common Additional Types

- `build:` - Build system or dependency changes
- `chore:` - Maintenance tasks
- `ci:` - CI/CD changes
- `docs:` - Documentation changes
- `style:` - Code style changes (formatting, no logic changes)
- `refactor:` - Code refactoring (no bug fixes or features)
- `perf:` - Performance improvements
- `test:` - Test additions or modifications

## Breaking Changes

Breaking changes (correlate to MAJOR in SemVer) can be indicated in two ways:

1. **Footer notation**: `BREAKING CHANGE: description`
2. **Exclamation mark**: Add `!` after type/scope (e.g., `feat!:` or `feat(scope)!:`)

Both methods can be used together. `BREAKING-CHANGE` is synonymous with `BREAKING CHANGE`.

## Scopes

- Optional contextual information in parentheses
- Must be a noun describing a codebase section
- Example: `feat(parser):`, `fix(api):`

## Specification Rules (RFC 2119 Keywords)

### MUST Requirements

- Commits MUST be prefixed with a type + colon + space
- `feat` MUST be used for new features
- `fix` MUST be used for bug fixes
- Description MUST immediately follow the type/scope prefix
- Body MUST begin one blank line after description
- Footer tokens MUST use `-` instead of spaces (except `BREAKING CHANGE`)
- Breaking changes MUST be indicated in type/scope prefix OR footer
- Footer breaking changes MUST use uppercase `BREAKING CHANGE:`
- `BREAKING CHANGE` MUST be uppercase
- Case sensitivity MUST NOT be enforced (except `BREAKING CHANGE`)

### MAY/OPTIONAL Rules

- Scope MAY be provided after type
- Longer body MAY be provided after description
- One or more footers MAY be provided
- Footer values MAY contain spaces and newlines
- `BREAKING CHANGE:` MAY be omitted from footer if `!` is used
- Types other than `feat` and `fix` MAY be used

## Example Patterns

### Basic Examples

```
docs: correct spelling of CHANGELOG
feat(lang): add Polish language
fix: prevent racing of requests
```

### Breaking Changes

```
feat!: send email when product shipped
feat(api)!: send email when product shipped
chore!: drop Node 6 support

BREAKING CHANGE: use JavaScript features not available in Node 6.
```

### With Body and Footers

```
fix: prevent racing of requests

Introduce request id and reference to latest request. Dismiss
incoming responses other than from latest request.

Remove timeouts which were used to mitigate racing but are
obsolete now.

Reviewed-by: Z
Refs: #123
```

## Footer Format

- Must consist of: `<token><separator><value>`
- Separators: `:<space>` or `<space>#`
- Inspired by git trailer format
- Example: `Reviewed-by: Jane Doe`, `Refs: #123`

## Key Benefits

- Automatic CHANGELOG generation
- Automatic semantic version determination
- Clear communication of change nature
- Triggers for build/publish processes
- Structured commit history for contributors

## SemVer Integration

| Commit Type | SemVer Bump | Description |
|-------------|-------------|-------------|
| `fix:` | PATCH | Bug fixes |
| `feat:` | MINOR | New features |
| `BREAKING CHANGE` | MAJOR | Breaking changes (any type) |

## Implementation Notes for Tools

- Case insensitive parsing (except `BREAKING CHANGE`)
- Footer parsing terminates at next valid token/separator pair
- Body is free-form, can contain multiple paragraphs
- Scope format: noun in parentheses
- Breaking change can be in type prefix OR footer (or both)

## Revert Commits

No explicit specification provided. Recommended approach:
```
revert: let us never again speak of the noodle incident

Refs: 676104e, a215868
```

## Workflow Integration

- Compatible with squash-merge workflows
- Lead maintainers can clean up commit messages during merge
- Not all contributors need to use the specification
- Tools can process commits that follow the specification

## Validation Rules Summary

1. Type is required (noun + colon + space)
2. Description is required (after colon + space)
3. Scope is optional (noun in parentheses)
4. Body is optional (blank line separator required)
5. Footers are optional (blank line separator required)
6. Breaking changes require `!` in type OR `BREAKING CHANGE:` footer
7. Footer tokens use hyphens instead of spaces
8. Only `BREAKING CHANGE` is case-sensitive (must be uppercase)