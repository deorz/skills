# Workflow skills

A collection of skills for project work. Each skill must be explicitly invoked by name; transitions between skills never happen automatically.

| Skill | Purpose |
| --- | --- |
| [design-interview](design-interview/SKILL.md) | Refine an idea through a bounded interview and document the decisions. |
| [investigate](investigate/SKILL.md) | Explore code, diagnose a problem or test a hypothesis. |
| [write-spec](write-spec/SKILL.md) | Create a project vision or change specification. |
| [implement-change](implement-change/SKILL.md) | Implement a change with deliberate test selection and project quality gates. |
| [review-code](review-code/SKILL.md) | Review requirements, correctness and adherence to project conventions. |

## Structure

Each top-level directory containing a `SKILL.md` is a standalone skill in the [Agent Skills](https://agentskills.io/specification) format. Supply the entire skill directory to the client, including any resources.

```text
<skill-name>/
└── SKILL.md             # YAML frontmatter and core instructions
```

Client-specific metadata and icons are not included. Each current skill consists solely of a `SKILL.md` containing metadata and instructions, including the explicit invocation requirement.

### Organization guidelines

- `SKILL.md` starts with YAML frontmatter. Required fields: `name` and `description`.
- `name` matches the directory name: 1–64 characters, lowercase letters `a-z`, digits and hyphens; no leading or trailing hyphens and no `--`.
- `description` contains 1–1024 characters and explains what the skill does and when to use it.
- Optional frontmatter fields defined by the specification: `license`, `compatibility`, `metadata`, `allowed-tools`. Custom metadata belongs under `metadata` as string key-value pairs. Include `compatibility` only for specific environment requirements, using 1–500 characters.
- Keep core instructions under 500 lines; the recommended size is fewer than 5000 tokens. The current skills are compact and do not need further splitting.
- Add `references/` for detailed reference material, `scripts/` for executable code and `assets/` for templates and static resources as needed. Do not create empty directories in advance.
- Resource links are relative to the skill root. Link directly from `SKILL.md` to the required materials and explain when to read them, avoiding nested reference chains. This allows details to be loaded only when needed.

## Validation

From the repository root, with [uv](https://docs.astral.sh/uv/) installed and network access for the initial download, run the official reference validator, [skills-ref](https://github.com/agentskills/agentskills/tree/main/skills-ref):

```bash
for skill in design-interview implement-change investigate review-code write-spec; do
  uvx --from 'git+https://github.com/agentskills/agentskills.git#subdirectory=skills-ref' skills-ref validate "$skill" || exit 1
done
```

The validator checks the skill format, frontmatter and naming conventions; it does not assess instruction quality.
