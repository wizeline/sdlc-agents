---
name: authoring-user-docs
description: "Use when producing user-facing documentation — tutorials, how-to guides, user guides, getting-started guides, installation guides, or onboarding documentation. Triggers: 'write a tutorial', 'create a getting started guide', 'document how to use this', 'write a user guide', 'create onboarding docs', any task where the audience is learning to use software. Always load authoring-technical-docs first."
---

# Authoring User Docs Action

Produces tutorials, how-to guides, and user guides — the learning-oriented and task-oriented quadrants of the Diátaxis framework.

**Load `authoring-technical-docs` first** for the multi-pass workflow, style rules, and quality framework. This action provides the templates and user-doc-specific rules.

---

## Choosing the right type

| | Tutorial | How-To Guide | User Guide |
|---|---|---|---|
| **Audience** | Beginners | Users who know the basics | All experience levels |
| **Structure** | Linear journey | Task-focused | Chapter-style, comprehensive |
| **Goal** | "I learned how this works" | "I accomplished my task" | "I understand the whole product" |

### Best Practices for Writing Tutorials

**1. Know and Address Your Audience**
Write in approachable, conversational "you" language rather than formal passive voice. Meet the reader where they are, and don't bury context they need to feel oriented.

**2. Get to a Win Early (The "Hello World" Rule)**
Give users a small, tangible success as soon as possible. Don't front-load pages of theory before the first runnable command — early momentum keeps learners engaged.

**3. Explain the "Why"**
Tutorials are action-heavy by nature, but provide enough context at each step so users understand the logic behind what they're doing, not just the mechanics.

**4. Use Consistent Formatting**
- Use **bold** for clickable UI elements (buttons, menus).
- Use `code-style` for strings, file names, and inline commands.
- Use fenced code blocks with syntax highlighting (e.g., ` ```python `) for all multi-line code.
- Separate **Command** (what the user types) from **Output** (what they should see).
- Never use screenshots for code — users can't copy-paste from images.

**5. Write Idempotent Steps**
Where possible, write steps that can be safely re-run without breaking anything. For example, prefer `mkdir -p` over `mkdir` so repeated runs don't throw errors. This is especially important for onboarding and setup flows.

**6. Define Done with a Validation Step**
Every tutorial must include a clear check so users know whether they succeeded. If a learner can't verify their own outcome, the tutorial is incomplete.

**7. Reduce Setup Friction**
If the initial setup is tedious, provide a shell script or a link to a completed GitHub repository so learners can get unblocked quickly and focus on the learning objective.

**8. Add Troubleshooting Tips**
Call out common pitfalls briefly at the end of relevant steps. A single sentence like *"If you see X error, check Y"* can save a learner hours of frustration.

### Best Practices for Writing User Guides

**1. Write for Your Audience**
Use clear, non-technical language suited to end-users. Never assume prior context — the first paragraph should orient anyone arriving cold, such as from a search engine.

**2. Structure Around Tasks, Not UI**
Organize content by what users want to *achieve*, not by how the interface is laid out. This keeps the guide goal-oriented and easier to navigate.

**3. Start with Prerequisites**
Present prerequisites as a checklist so users can verify they're ready before beginning. This prevents frustration mid-procedure.

**4. One Action Per Step**
Don't chain multiple actions in a single step. Each step should be discrete, actionable, and written in plain language.

**5. Show Expected Results**
After every significant step, tell the reader what they should see. This builds confidence and helps users catch errors early.

**6. Progressive Disclosure**
Lead with the most common path. Move edge cases, advanced options, and variations into clearly labeled sections so they don't overwhelm the primary flow.

**7. Use Visual Aids Purposefully**
Include screenshots or diagrams wherever they clarify a major action or reduce ambiguity. Visuals should support the text, not replace it — every image should have a corresponding written step.

**8. Test Every Procedure**
Follow your own instructions before publishing. If you can't complete a step as written, flag it as a gap and fix it.

**9. End with Next Steps**
Always give the reader somewhere to go. A brief "What's next?" section or links to related guides prevents dead ends and encourages continued learning.

### Best Practices for Writing How-to Guides

**1. Lead with the Action ("Do, then Explain")**
Give the instruction first, then explain why if necessary. Don't front-load theory before the command — readers came to accomplish something, not to read background context.

- **Avoid:** *Because the server needs to listen on a specific channel, you should configure the port.*
- **Prefer:** *Set the port listener. This allows the server to accept incoming requests.*

**2. Use Imperative Mood and Active Voice**
Write direct, commanding sentences throughout. "Open the file" is clearer than "The file should be opened." "The system generates a log" is clearer than "A log is generated by the system." These two principles work together to keep every instruction unambiguous and scannable.

**3. Be Language- and Tool-Agnostic Where Possible**
When a guide covers a general workflow, describe the *logic* before the *implementation*. If multi-language examples are needed, present the conceptual step first, then offer language-specific tabs or snippets. Only tie the guide to a specific language or tool when an example is genuinely required for clarity.

**4. Format Code and UI Elements Consistently**
- **Bold** for UI elements: Click **Save**, select **File > Open**.
- `Code blocks` for file names, paths, and inline commands — always in a copy-pasteable format.
- Mark placeholders clearly so users know what to substitute: `<YOUR_API_KEY>`, or *italics* for inline placeholders like `user_id = <your_id>`.

**5. Use Inclusive Language**
Use gender-neutral terms throughout and avoid culturally specific idioms or stereotypes. Accessibility in language broadens your audience and reflects good authorship.

---

## Templates

| User Doc | Asset |
|--------|--------|
| `User Guide` | Read `./assets/user_guide_template.md` |
| `Tutorial` | Read `./assets/tutorial_template.md` |
| `How-to guide` | Read `./assets/how_to_guide_template.md` |

Save tutorials to `docs/guides/tutorial-*.md`. Save how-to guides to `docs/guides/how-to-*.md`. Save user guides to `docs/guides/guide-*.md`.
