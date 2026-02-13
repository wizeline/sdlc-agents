# wize-skills
Agent Skills that allow to extent Agentic Assistants like Claude or Gemini with specialized expertise, procedural workflows, and task-specific resources. Based on the [Agent Skills](https://agentskills.io/home) open standard, a “skill” is a self-contained directory that packages instructions and assets into a discoverable capability.

## Claude

## Gemini

```bash
gemini skills list
gemini skills link /path/to/my-skills-repo
gemini skills link /path/to/my-skills-repo --scope workspace
gemini skills install https://github.com/user/repo.git
gemini skills install /path/to/local/skill
gemini skills install /path/to/local/my-expertise.skill
gemini skills install https://github.com/my-org/my-skills.git --path skills/frontend-design
gemini skills install /path/to/skill --scope workspace
gemini skills uninstall my-expertise --scope workspace
gemini skills enable my-expertise
gemini skills disable my-expertise --scope workspace
```
