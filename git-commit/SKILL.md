---
name: git-commit
description: Create clean, atomic Git commits with clear Conventional Commit messages. Always use when making commits.
---

When creating commits:

1. **Atomic commits**: Commit one logical change at a time. Inspect changes and stage related files selectively. Never use `git add .` blindly.
2. **Conventional Commits**: Use:
   `<type>(<scope>): <imperative description>`
   Allowed types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `ci`.
   Omit the scope when unnecessary.
3. **Subject**:
   - Use imperative mood: "Add", "Fix", "Remove", not "Added" or "Fixes".
   - Capitalize the description and omit the final period.
   - Target <50 characters; never exceed 72.
4. **Body**: Add one only when useful. Separate it with a blank line, wrap at 72 characters, and explain **what changed and why**, including important side effects. Do not narrate the diff.
5. **Clear English**: Follow STE and Google style principles. Use simple, precise words, active voice, present tense, consistent terminology, and short sentences. Avoid jargon, idioms, filler, and unnecessary complexity.
6. **Safety**: Never amend, push, or rewrite history unless explicitly requested. Never commit secrets, `.env` files, temporary logs, or generated/build artifacts unless intentionally tracked.
