# Repository instructions

- Store reusable Agent Skills under `skills/<name>/`.
- Every skill directory must contain `SKILL.md`. Keep all referenced files at their expected relative paths.
- Treat imported skills as reviewed snapshots. Do not silently replace them with a newer upstream version.
- Preserve names, frontmatter, commands, scripts, tests, licenses, and attribution from the imported copy.
- Keep generated dependencies, caches, test output, and virtual environments out of the repository.

## Storage policy

- This project is local-only. Its real root is `/Users/lbr/Project/my_skills`; it is not synchronized with iCloud Drive.
- Put regenerable machine-local data under `/Users/lbr/Project/.local/my_skills` when a tool needs persistent local storage.
- Restore the repository with `git clone git@github.com:livingshade/my_skills.git /Users/lbr/Project/my_skills`. The committed skills require no generated dependency directory to be restored.
