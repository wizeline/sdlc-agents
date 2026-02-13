# **The Engineering of Content: Architecting the Modern Technical Writer and the Transition to Agentic Documentation Systems**

The metamorphosis of technical writing from a support function to a core engineering discipline represents one of the most significant shifts in the modern software development lifecycle. In the contemporary landscape, the role of a technical writer is no longer confined to the retrospective documentation of completed features; it has evolved into a proactive, system-oriented practice often referred to as documentation engineering or "Docs-as-Code".1 This transition is underpinned by a radical change in the required technical skill set, where proficiency in linguistic nuance is augmented by expertise in version control, automation frameworks, and the architectural principles of large-scale software systems.3 As organizations move toward 2026, the strategic focus has shifted from mere content creation to the development of agentic systems capable of synthesizing, maintaining, and optimizing documentation with minimal human intervention, effectively birthing the era of the "transhuman writer".1

## **The Technical Writer as a Documentation Engineer**

The modern technical writer operates at the intersection of product management, software engineering, and user advocacy. This positioning requires a deep understanding of the technologies being documented, ranging from API architectures and microservices to complex cloud-native environments.2 The technical writer is increasingly viewed as a "Docs Engineer," a professional who applies software engineering principles to the lifecycle of technical content.1

### **The Technical Skill Matrix**

To function effectively in this modern capacity, the technical writer must possess a diverse and rigorous set of technical skills. These competencies allow the writer to integrate directly into developer workflows, using the same tools and environments as the engineering teams they support.6

| Skill Category | Primary Competency | Technical Context and Tools |
| :---- | :---- | :---- |
| Version Control | Distributed Repository Management | Mastering Git for branching, merging, and peer-reviewed pull requests to ensure document versioning matches code releases.2 |
| Markup Languages | Structured Text Formats | Proficiency in Markdown, reStructuredText, and AsciiDoc to facilitate documentation residing as plain text in code repositories.3 |
| API Proficiency | Interface Documentation | Understanding RESTful principles and utilizing tools like Swagger/OpenAPI, Postman, and cURL to document endpoints and schemas.8 |
| CI/CD Integration | Automation Pipelines | Configuring GitHub Actions, Jenkins, or GitLab CI to automate the linting, building, and deployment of documentation sites.10 |
| Scripting & Tooling | Infrastructure as Code | Utilizing Python, JavaScript, or YAML to create custom automation scripts, manage static site generators, and configure linters.1 |
| Information Architecture | Semantic Structuring | Designing logical content hierarchies and navigation paths that reduce user cognitive load and improve searchability.4 |

This technical foundation enables the writer to participate in the "Docs-as-Code" philosophy, which treats documentation with the same level of rigor as source code. By utilizing issue trackers like Jira to manage documentation tasks and employing automated tests to verify the accuracy of code snippets, the writer ensures that the documentation is as agile and reliable as the software it describes.2

## **Operational Standards and the Science of Style**

The consistency and quality of technical documentation are governed by rigorous standards and style guides. These frameworks are not merely sets of grammar rules; they are comprehensive strategic documents that define the brand’s technical voice, tone, and cognitive approach to user education.8 In the context of creating an AI agent, these style guides serve as the primary "source of truth" for prompt engineering and automated quality enforcement.15

### **The Hierarchy of Industry Standards**

Major technology organizations have published style guides that have become the industry standard for developer-centric documentation. These guides are meticulously curated to balance technical precision with user accessibility.9

| Organization | Style Guide Focus | Unique Structural Characteristics |
| :---- | :---- | :---- |
| Google | Developer Experience | Prioritizes accessibility, localization, and specific technical notation for code comments and syntax.9 |
| Microsoft | General Technical Writing | Offers exhaustive guidelines on UI interactions, bias-free communication, and a "warm yet crisp" brand voice.14 |
| Red Hat | Enterprise Systems | Focuses on complex technical diagrams, product naming conventions, and brand architecture for IT professionals.18 |
| Apple | Product Integration | Concentrates on instructional text for training programs and the linguistic consistency of user interfaces.9 |
| IBM | Global Enterprise Software | Establishes conventions for large-scale documentation projects and word usage for global software deployments.9 |

### **Linguistic Patterns and Technical Consistency**

Style guides codify specific linguistic patterns that reduce ambiguity. For example, most modern guides advocate for sentence case in headings and the use of the Oxford comma to prevent misinterpretation in complex lists.8 The shift toward "inclusive language" is also a critical component of modern standards, where terms that might imply bias are replaced with neutral, descriptive alternatives.14 For an AI agent, these rules are translated into YAML-based linter configurations, allowing for the automated detection of stylistic deviations during the content synthesis phase.15

## **Input Mechanisms: The Data Harvesting Phase**

The creation of technical documentation is a research-heavy endeavor, often cited as being 80% investigation and 20% composition.21 For a technical writer or an AI agent to produce accurate content, they must harvest data from several high-fidelity sources within the development ecosystem.21

### **Core Data Streams**

The "inputs" for the documentation process are diverse, requiring the synthesis of structured and unstructured data.7

1. **Issue Tracking (Jira/Asana):** Tickets serve as the primary trigger for documentation tasks. They contain the "user story," describing the problem being solved, the technical requirements, and the acceptance criteria.7  
2. **Source Code and Comments:** Direct analysis of the codebase allows the writer to extract parameter names, data types, and return values. "In-code" documentation, such as Python DocStrings, provides the raw material for API references.3  
3. **SME Interviews:** Subject Matter Experts (SMEs) provide the "why" behind technical decisions and the nuances of implementation that are not always visible in the code.28  
4. **Product Specifications (PRDs):** High-level documents that outline the architectural vision and the target audience's needs.21

### **The Interview and Research Workflow**

The information gathering process is highly structured. Writers must perform "pre-interview research" to build a foundational understanding of the feature before engaging with engineers.28 This phase involves reviewing recent commits and Jira comments to formulate precise, non-generic questions.29 During the interview, the writer employs active listening and record-keeping tools to ensure that technical details—such as system dependencies and known limitations—are captured accurately.28 For an AI agent, this research phase is simulated through "Agentic Search" and the retrieval of context from internal databases using RAG (Retrieval-Augmented Generation).31

## **The Architecture of Documentation Output**

The "outputs" of the technical writing process are categorized based on their intent and the user’s stage in the learning journey. Modern documentation systems often follow the Diátaxis framework, which divides content into four distinct quadrants: Tutorials, How-To Guides, Explanations, and References.26

### **Content Categorization and Format Standards**

The choice of output format is critical for interoperability and machine readability. Markdown has emerged as the de facto standard due to its simplicity and its ability to be rendered into HTML, PDF, or integrated into headless CMS platforms.2

| Output Type | Primary Purpose | Structural Requirements |
| :---- | :---- | :---- |
| Tutorials | Learning-Oriented | Step-by-step, hands-on instructions designed to guide a novice through a new feature.26 |
| How-To Guides | Problem-Oriented | Task-based instructions for users who already have some familiarity with the product.26 |
| Explanations | Understanding-Oriented | High-level overviews that provide context and the "why" behind technical architectures.26 |
| References | Information-Oriented | Precise technical data on API endpoints, SDK functions, and configuration parameters.26 |

For an AI agent, the "output format" is not just the final text but also the metadata required for discovery and SEO. This includes YAML frontmatter for Jekyll or Hugo, meta descriptions, and semantic tags that help search engines and other AI models parse the content effectively.16

## **Quality Assurance: The Automated Governance Layer**

In a Docs-as-Code environment, quality assurance is an automated, continuous process. The technical writer employs "prose linters" and "health check" scripts to enforce standards before content is published.15

### **Automated Linting with Vale**

Vale is the primary tool used for documentation linting in modern engineering teams. Unlike standard spell-checkers, Vale is syntax-aware and can be configured to enforce complex style guide rules.15 Organizations like DataDog and GitLab use Vale to catch everything from passive voice and gendered pronouns to the correct use of the Oxford comma.12

YAML

\# Example Vale Rule for Oxford Comma Enforcement  
extends: existence  
message: "Use the Oxford comma in '%s'."  
link: "https://styleguide.example.com/commas"  
level: error  
tokens:  
  \- '(?:\[^,\]+,){1,}\\s\\w+\\s(?:and|or)'

This automated check ensures that all contributors, whether human or AI, adhere to the same linguistic standards, reducing the review burden on senior editors.15

### **Documentation Health Metrics**

Quality is also measured through "Document Health Checks," which assess the structural integrity of the documentation repository.35

| Health Check Metric | Description | Automation Mechanism |
| :---- | :---- | :---- |
| Link Integrity | Verification of all internal and external URLs to prevent 404 errors.35 | Automated scripts run during CI/CD to crawl the documentation site. |
| Metadata Validation | Ensuring every file has the required tags, titles, and descriptions for the build system.16 | Linter rules that check for the presence and format of YAML frontmatter. |
| Snippet Verification | Testing that the code examples in the documentation actually compile and run.6 | Integration of "DocTests" that execute code blocks as part of the test suite. |
| Translation Parity | Tracking whether translated pages are in sync with the latest version of the source language.35 | CCMS workflows that flag outdated translations based on commit hashes. |

## **Engineering the AI Agent as a Technical Writer**

Creating an AI agent capable of performing the role of a technical writer requires moving beyond simple prompt engineering toward a multi-agent orchestration framework. This system must be designed to handle the complexity of technical research, linguistic synthesis, and automated verification.5

### **The Multi-Agent Role Framework**

A documentation agent is not a single entity but a "pod" of specialized units working toward a common goal.5

1. **The Orchestrator:** This agent manages the overall project flow, identifying which features need documentation and sequencing the tasks between other agents.5  
2. **The Research Specialist (Automated Specialist):** Tasked with harvesting data from Jira, source code, and internal Wikis. It uses RAG to maintain a "living source of truth".5  
3. **The Content Creator (Task Performer):** This unit synthesizes the raw data into drafts, following the rules established in the style guides.5  
4. **The Critic (Collaborator Agent):** Responsible for the "Agentic Debate." It reviews the drafts for inaccuracies, hallucinations, and stylistic deviations, forcing the creator to iterate until the quality thresholds are met.5

### **Context Engineering and System Prompts**

The success of the agent pod depends on "Context Engineering"—optimizing the tokens provided to the LLM to ensure accuracy while remaining within the model’s operational limits.31 System prompts must define a clear persona and provide a step-by-step logic for the agent to follow.38

#### **Logic Sequence for a Documentation Agent**

* **Step 1: Ingestion:** Scan the repository for new commit messages and Jira tickets marked as "Done".40  
* **Step 2: Analysis:** Extract business logic and technical specifications from the code using static analysis and LLM summarization.42  
* **Step 3: Drafting:** Generate a Markdown file following the organization’s specific template (e.g., Prerequisites \-\> Body \-\> Examples \-\> Troubleshooting).26  
* **Step 4: Verification:** Run the draft through the Vale linter and the build system to catch errors.15  
* **Step 5: Human-in-the-loop:** Present the finalized PR to a human writer for a "judgment call" on narrative coherence and user value.16

## **Workflows, Funnels, and the AI Marketing Shift**

The ultimate goal of the technical writing workflow is to serve the "marketing funnel." In 2026, documentation is increasingly recognized as the primary tool for user discovery and pre-sale validation.46

### **The Documentation Funnel**

Documentation serves different stages of the user journey, and the AI agent must be prompted to address these specific goals.46

| Funnel Stage | Documentation Goal | Key AI Metrics |
| :---- | :---- | :---- |
| **Awareness** | Discovery of features through AI search tools and LLMs.46 | SEO tags, AI-readiness, semantic clarity. |
| **Consideration** | Detailed comparison of technical specs and integration requirements.46 | API reference accuracy, "why" explanations. |
| **Decision** | Final validation of implementation time and pricing at scale.46 | How-To guides, pricing tables, case studies. |
| **Retention** | Successful onboarding and troubleshooting to reduce churn.47 | Tutorial completion rate, support ticket reduction. |

### **AI-Readiness and the New Top-of-Funnel**

AI-driven page views for documentation sites are seeing exponential growth, as users move away from traditional search engines toward AI assistants like ChatGPT for technical queries.46 If a product’s documentation is not "AI-readable"—meaning it is thin, outdated, or lacks a structured index—the AI assistant may misrepresent the product, potentially directing users to competitors.46 To counter this, technical writers are now focusing on "AI-Native" capabilities, such as creating MCP (Model Context Protocol) servers that allow their documentation to power external AI tools directly.46

## **Synthesis: The Future of the Transhuman Writer**

The role of the technical writer is shifting from a manual crafter of prose to a strategist who designs the systems that generate that prose.1 This "transhuman" approach does not eliminate the need for human writers but rather elevates them to a role focused on judgment, empathy, and high-level architecture.1

The agentic technical writer of the future is a composite of human expertise and machine speed. While the AI agent can trace thousands of dependencies in legacy code in minutes—a task that would take a human weeks—the human writer remains the "strategist," defining the "why" and ensuring that the content aligns with human needs and market nuances.5 This synergy ensures that documentation is not just a reactive record of the past but a strategic asset that drives growth, innovation, and user success in the digital age.46

### **Mathematical Foundations of Document Reliability**

To quantify the efficiency of an agentic documentation system, we can analyze the relationship between Accuracy ![][image1], Volume ![][image2], and Time ![][image3]. For an AI agent pod, the throughput ![][image4] is significantly higher than that of a human, but it is constrained by a "Hallucination Variable" ![][image5].

The reliability of a documentation set ![][image6] can be modeled as:

![][image7]  
Where human intervention ![][image8] serves to minimize ![][image5]. In a multi-agent "debate" system, ![][image5] is reduced through iterative adversarial checking:

![][image9]  
Where ![][image10] is the number of debate cycles and ![][image11] is the efficiency coefficient of the critic agent. This model suggests that as the number of agentic reviews increases, the reliability of the output approaches near-human levels while maintaining machine-like speed.

### **Conclusion**

The evolution of the technical writer into a documentation engineer is an essential adaptation to the complexities of modern software development. By leveraging "Docs-as-Code" methodologies, automated quality gates, and agentic multi-agent systems, writing teams can now deliver documentation that is as dynamic and robust as the code it serves.2 The future of the discipline lies in the seamless integration of human linguistic artistry with agentic system precision, ensuring that the documentation funnel remains a powerful driver for product adoption and user loyalty.1

#### **Works cited**

1. My technical writing predictions for 2025 \- passo.uno, accessed February 11, 2026, [https://passo.uno/tech-writing-predictions-2025/](https://passo.uno/tech-writing-predictions-2025/)  
2. What is Docs as Code? Guide to Modern Technical Documentation | Kong Inc., accessed February 11, 2026, [https://konghq.com/blog/learning-center/what-is-docs-as-code](https://konghq.com/blog/learning-center/what-is-docs-as-code)  
3. What Are the Essential Skills Every Aspiring Technical Writer in Tech Should Master?, accessed February 11, 2026, [https://www.womentech.net/how-to/what-are-essential-skills-every-aspiring-technical-writer-in-tech-should-master](https://www.womentech.net/how-to/what-are-essential-skills-every-aspiring-technical-writer-in-tech-should-master)  
4. Technical Writer Skills in 2025 (Top \+ Most Underrated Skills) \- Teal, accessed February 11, 2026, [https://www.tealhq.com/skills/technical-writer](https://www.tealhq.com/skills/technical-writer)  
5. Building With Multi-Agent AI Systems | Wizeline, accessed February 11, 2026, [https://www.wizeline.ai/building-with-multi-agent-ai-systems/](https://www.wizeline.ai/building-with-multi-agent-ai-systems/)  
6. Docs as Code — Write the Docs, accessed February 11, 2026, [https://www.writethedocs.org/guide/docs-as-code.html](https://www.writethedocs.org/guide/docs-as-code.html)  
7. Why Technical Writers Should Use Jira | by Breanna Fitzgerald \- Medium, accessed February 11, 2026, [https://annafitz-ux.medium.com/why-technical-writers-should-use-jira-3348813e8915](https://annafitz-ux.medium.com/why-technical-writers-should-use-jira-3348813e8915)  
8. Style Guides \- Write the Docs, accessed February 11, 2026, [https://www.writethedocs.org/guide/writing/style-guides.html](https://www.writethedocs.org/guide/writing/style-guides.html)  
9. Style Guides \- Iris IX Studio, accessed February 11, 2026, [https://www.iris-ixstudio.com/style](https://www.iris-ixstudio.com/style)  
10. vale-lint · Actions · GitHub Marketplace, accessed February 11, 2026, [https://github.com/marketplace/actions/vale-lint](https://github.com/marketplace/actions/vale-lint)  
11. elastic/vale-rules: Elastic Docs' style guide rules for the Vale linter \- GitHub, accessed February 11, 2026, [https://github.com/elastic/vale-rules](https://github.com/elastic/vale-rules)  
12. Vale documentation tests \- GitLab Docs, accessed February 11, 2026, [https://docs.gitlab.com/development/documentation/testing/vale/](https://docs.gitlab.com/development/documentation/testing/vale/)  
13. I Built An AI Tool To Document Python Code — Then Used It On Itself. \- Medium, accessed February 11, 2026, [https://medium.com/@mikejpabon/i-built-an-ai-tool-to-document-python-code-then-used-it-on-itself-16e1b4f87014](https://medium.com/@mikejpabon/i-built-an-ai-tool-to-document-python-code-then-used-it-on-itself-16e1b4f87014)  
14. The Best Technical Writer Style Guides I'm Using for Inspiration in 2026, accessed February 11, 2026, [https://technicalwriterhq.com/writing/technical-writing/technical-writer-style-guide/](https://technicalwriterhq.com/writing/technical-writing/technical-writer-style-guide/)  
15. How we use Vale to improve our documentation editing process \- Datadog, accessed February 11, 2026, [https://www.datadoghq.com/blog/engineering/how-we-use-vale-to-improve-our-documentation-editing-process/](https://www.datadoghq.com/blog/engineering/how-we-use-vale-to-improve-our-documentation-editing-process/)  
16. Technical writing with AI agents: Devin, Cursor, and Claude Code in January 2026 \- Fern, accessed February 11, 2026, [https://buildwithfern.com/post/technical-writing-ai-agents-devin-cursor-claude-code](https://buildwithfern.com/post/technical-writing-ai-agents-devin-cursor-claude-code)  
17. Welcome \- Microsoft Writing Style Guide, accessed February 11, 2026, [https://learn.microsoft.com/en-us/style-guide/welcome/](https://learn.microsoft.com/en-us/style-guide/welcome/)  
18. Resources \- Red Hat brand standards, accessed February 11, 2026, [https://www.redhat.com/en/about/brand/standards/resources](https://www.redhat.com/en/about/brand/standards/resources)  
19. Style guides, linters, and Vale: Why do tech writers need this? | Documentation Portal, accessed February 11, 2026, [https://tw-docs.com/docs/vale/vale-styleguides/](https://tw-docs.com/docs/vale/vale-styleguides/)  
20. Harnessing Vale. Vale is a syntax-aware linter | by Patrick Rachford | Medium, accessed February 11, 2026, [https://patford12.medium.com/harnessing-vale-82d8c3f016e9](https://patford12.medium.com/harnessing-vale-82d8c3f016e9)  
21. Technical Writing: A Comprehensive Guide (2025) \- adoc Studio, accessed February 11, 2026, [https://www.adoc-studio.app/blog/technical-writing-guide](https://www.adoc-studio.app/blog/technical-writing-guide)  
22. Technical Writer Interview Questions and Answers I'd Prepare For in 2026, accessed February 11, 2026, [https://technicalwriterhq.com/career/technical-writer/technical-writer-interview-questions/](https://technicalwriterhq.com/career/technical-writer/technical-writer-interview-questions/)  
23. How to write a useful Jira ticket \- Atlassian Community, accessed February 11, 2026, [https://community.atlassian.com/forums/Jira-articles/How-to-write-a-useful-Jira-ticket/ba-p/2147004](https://community.atlassian.com/forums/Jira-articles/How-to-write-a-useful-Jira-ticket/ba-p/2147004)  
24. 70 Most Common Jira Interview Questions with Expert Answers, accessed February 11, 2026, [https://www.interviewcoder.co/blog/jira-interview-questions](https://www.interviewcoder.co/blog/jira-interview-questions)  
25. Create Jira User Story Templates. Stop wasting 10 minutes per ticket —… | by BOUTERBIAT Oualid | Agile Insider | Medium, accessed February 11, 2026, [https://medium.com/agileinsider/create-jira-user-story-templates-246b3dadc398](https://medium.com/agileinsider/create-jira-user-story-templates-246b3dadc398)  
26. How to document and comment code using AI (With examples) \- Pluralsight, accessed February 11, 2026, [https://www.pluralsight.com/resources/blog/software-development/documenting-commenting-code-with-AI](https://www.pluralsight.com/resources/blog/software-development/documenting-commenting-code-with-AI)  
27. Write less with this AI-powered code documentation tool \- DEV Community, accessed February 11, 2026, [https://dev.to/hackmamba/write-less-with-this-ai-powered-code-documentation-tool-h27](https://dev.to/hackmamba/write-less-with-this-ai-powered-code-documentation-tool-h27)  
28. Technical Writing Process: Preparing for an Interview with the SME \- StoryAZ Studio, accessed February 11, 2026, [https://storyaz.studio/technical-writing-process-preparing-for-sme-interview/](https://storyaz.studio/technical-writing-process-preparing-for-sme-interview/)  
29. How do technical writers interview SME 's? : r/technicalwriting \- Reddit, accessed February 11, 2026, [https://www.reddit.com/r/technicalwriting/comments/11ujnck/how\_do\_technical\_writers\_interview\_sme\_s/](https://www.reddit.com/r/technicalwriting/comments/11ujnck/how_do_technical_writers_interview_sme_s/)  
30. How to write a good spec for AI agents \- Addy Osmani, accessed February 11, 2026, [https://addyosmani.com/blog/good-spec/](https://addyosmani.com/blog/good-spec/)  
31. Effective context engineering for AI agents \- Anthropic, accessed February 11, 2026, [https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)  
32. A Practical Guide to Choosing Prompts, Workflows, or Agents | by Sean Falconer | Medium, accessed February 11, 2026, [https://seanfalconer.medium.com/a-practical-guide-to-choosing-prompts-workflows-or-agents-6d5fb88f9e69](https://seanfalconer.medium.com/a-practical-guide-to-choosing-prompts-workflows-or-agents-6d5fb88f9e69)  
33. Introducing AI into your product documentation workflow | Guides \- GitBook, accessed February 11, 2026, [https://gitbook.com/docs/guides/docs-workflow-optimization/introducing-ai-into-your-product-documentation-workflow](https://gitbook.com/docs/guides/docs-workflow-optimization/introducing-ai-into-your-product-documentation-workflow)  
34. Best 7 AI Tools for Technical Writing \- Document360, accessed February 11, 2026, [https://document360.com/blog/ai-tools-for-technical-writing/](https://document360.com/blog/ai-tools-for-technical-writing/)  
35. Technical documentation at WAGO \- Quanos, accessed February 11, 2026, [https://quanos.com/en/references/details/wago-5-examples-of-automated-technical-documentation/](https://quanos.com/en/references/details/wago-5-examples-of-automated-technical-documentation/)  
36. Introduction \- Vale CLI, accessed February 11, 2026, [https://vale.sh/docs](https://vale.sh/docs)  
37. Markdown Optimization AI Agents \- Relevance AI, accessed February 11, 2026, [https://relevanceai.com/agent-templates-tasks/markdown-optimization-ai-agents](https://relevanceai.com/agent-templates-tasks/markdown-optimization-ai-agents)  
38. Mastering System Prompts for AI Agents | by Patric \- Medium, accessed February 11, 2026, [https://pguso.medium.com/mastering-system-prompts-for-ai-agents-3492bf4a986b](https://pguso.medium.com/mastering-system-prompts-for-ai-agents-3492bf4a986b)  
39. Agents \- Prompts \- UiPath Documentation, accessed February 11, 2026, [https://docs.uipath.com/agents/automation-cloud/latest/user-guide/agent-prompts](https://docs.uipath.com/agents/automation-cloud/latest/user-guide/agent-prompts)  
40. Generate professional changelogs from Git commits with GPT-4 and GitHub | n8n workflow template, accessed February 11, 2026, [https://n8n.io/workflows/8137-generate-professional-changelogs-from-git-commits-with-gpt-4-and-github/](https://n8n.io/workflows/8137-generate-professional-changelogs-from-git-commits-with-gpt-4-and-github/)  
41. How to Automate Release Notes with AI: Complete GitHub Actions Tutorial \- Ascend.io, accessed February 11, 2026, [https://www.ascend.io/blog/how-we-built-an-ai-powered-release-notes-pipeline](https://www.ascend.io/blog/how-we-built-an-ai-powered-release-notes-pipeline)  
42. context-labs/autodoc: Experimental toolkit for auto-generating codebase documentation using LLMs \- GitHub, accessed February 11, 2026, [https://github.com/context-labs/autodoc](https://github.com/context-labs/autodoc)  
43. automated-documentation · GitHub Topics, accessed February 11, 2026, [https://github.com/topics/automated-documentation](https://github.com/topics/automated-documentation)  
44. Simple and effective AI prompts for tech writers \- tcworld magazine, accessed February 11, 2026, [https://www.tcworld.info/e-magazine/technical-writing/simple-and-effective-ai-prompts-for-tech-writers](https://www.tcworld.info/e-magazine/technical-writing/simple-and-effective-ai-prompts-for-tech-writers)  
45. Enterprise agentic AI use cases: The complete guide to workflow automation \- Writer, accessed February 11, 2026, [https://writer.com/guides/ai-use-cases/](https://writer.com/guides/ai-use-cases/)  
46. AI is the new marketing funnel — and your docs decide who wins – GitBook Blog, accessed February 11, 2026, [https://www.gitbook.com/blog/ai-docs-as-marketing](https://www.gitbook.com/blog/ai-docs-as-marketing)  
47. 11 AI-powered sales automation workflows that work for every funnel stage \- HubSpot Blog, accessed February 11, 2026, [https://blog.hubspot.com/sales/ai-sales-automation-examples](https://blog.hubspot.com/sales/ai-sales-automation-examples)  
48. The Importance of Technical Writing in 2025 \- TransCurators, accessed February 11, 2026, [https://www.transcurators.com/blog/what-is-the-importance-of-technical-writing-in-2025](https://www.transcurators.com/blog/what-is-the-importance-of-technical-writing-in-2025)

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAYCAYAAADtaU2/AAACs0lEQVR4Xp1WP2tXMRS9AQUHFamoIC2tiyioiCjYyVKECg4q6KguDrZUHOzi4KI4KbQ/HEpbEOqmRbtUUFDsItRBHcRB6OBHEPwA9Zzc/H1JlHrKyU3OvTc3yXt5v4oEGPxZ40b1fopCKoR/IS2QSx01Ra4X8a00j3ox2Q5uK/VGdB12jnpUTTVyFO00XDu7LqKSsgXqDVjmRRgZQDuP+IH2euNgP/gCPBQUj2ayDIM/wfNeSNz0PRW7ifYEHN1HO5WpHi62k87jXEJvA/3JxOUDDcxDsb7OBNbq4DC4qjZfUYGYdB18w8KwdzO366F/Cp0P4GBr1kk4FiV5qZrQwv1oFsCz4DqkXroLD4x2wbxG77IXUrAYi97O5RIuD8bcE32ux8F1cF70RQuxSfRjtE+YlDgt9oFrUMdKVxUs9gjcKjF3Cbl85op8Hj7/V+AOHUYnJjJfYU97IbqKo+PpzKB3zEm+MI6Tx1pd+Ri4JhqbwNgdfDK6EyukptO/CN6h4iQWY9EwcaU0C38Bh4LigjqFc2cy0R7wHQS+xbxC1oq+1d+gH4yhGVj4B+KPdB2xcGW5DvTcQnOljDE94QumJ1c7KBQ21aMeQvtZ9Gq0wGc6Y+rXrQf9N+ax+bFwODJepY/gXpVMcO0G34PXassVPeLnEC5EZ7azKaPHzp1FNYLXdFHCon2M/hRPgw+c4l194Eu3G078S7i4GHEG/C7xedP/Fv0DGmOjcLftHddParYuOzCXYFbQ55emvu6Gmut5H6NB0U8mP51BT8HdLUMbzV3uP5NNIM0VfRlnRT82TZwD5yT7Ad9E2TKUm3kG/URw6ukWO2MzATPhQ2qz/V0KaezwJ/Zm5vYoEvRIxsGTwfV/GAGvpnsrq1ukz7IR0tHtKDap2oZ318Mqamtu1y8yWgty5g9QFlJANEXpUwAAAABJRU5ErkJggg==>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB8AAAAYCAYAAAACqyaBAAACpElEQVR4XpVWu4oUQRS9BStsoIusyOKCMiaigqJioIksIoxgIMKabuzbRAMx00gQ1MBgxUQD8REooqFgIqwsamZk5BcIfoCe+6jpW3Wr2/HAmao+99V1u7p6iJpItTCBt/R7dZjCv0fuRcM/VGn4CAo9tdw2grO12EKoWc43gHNe6HwaVYF94D3KQf9CWGWRlIvfgnTKiwGWYxG/L8HdTr0J/gT/GH8RJ5yYBWcxZz37PAM3mW0ejk8xHupZrACmxEmvTS47LOBqjZQL3iBQ19PgC3Bro6XHSW+IH6eh9NkDfrSxBge9Ar+Co9LESHNItUqTjrFUDHiE6Q3GM/G+FJfAJ9TeaDPgI/AHeKCyMVbAc6Xkq8imvkGag3MV4IJc+KpclXH55y7J80zjyrwTw2OSZzuIE+Aa/BcnOW3Ec0z8PMettpgPd4Y3E68yg3fzHXBJrkKsCTpwx9YxP9jZFWz4Bh6pDR3SMmlx6Y7d0BLpzrdWltW7pskPb1RZYH2PXPwzj95Qzbltv8EHJnGbH4I7ygWWMyfl4rwIEfKI4kmKmxIhPrLhnpPu/suk7/e0yK9rtTEp6cq1QL3iLPABtA6+B4+B9ym8GSHSz23laVybR+AX0tY2IJ5bwA/g96Sn4P5GgVIqMaK6hvnmxCsDwfmg4U13hXLoYP3CyLv8E7jLiWoh/Zjc5mnI1wm82d6B814P/gGScxk/uPnkjthcLOHo08SbWxlM2ovZdi94DNzEDAS8Gcm+GxG8mtdw4o+AIhRoLNUQJN8aPQXf2tiZK5wk/UDMDtQJGPTRxl4HL8pVp9dILF0wqtmvwAfEYIX5u7CjpB+Uqf6c4LxO5xF3WC/LwoM1I7aRfs2kcI9PH0p334SA0KFG7H+jEdWQBuBf3bJ9fwEwFE4dw+HLgQAAAABJRU5ErkJggg==>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB0AAAAYCAYAAAAGXva8AAACi0lEQVR4Xq1WO2tWQRCdwQhBokiC+CCBz8oEFBQsIhaKCIZoE9BObDUiprH5uqAICkKCkMIy2PgoRNBSTCMkSOLPsBXyA/TMzt67M7t79RI8cO7OzszuzL4v0X8Hd8hZzZvqSD7cyz8hevdoNAaO5soC3R3tAw9TnmE51NZ+BvIK5EPWpUFXnEwv1Xvg3WTKRhxjCk6A78DpaBIcAd+CWz25HFoR7QdfgjdCzQ9Mwap+jOKR0xJdQfEV8kXWjkR/FZ9d8GnjBBwjTW4p1gUzsH6Ey6TR2VHyDD4bJI7evAxedpkyDUmDzrWeigfg9SBpKiMo1iCHgbhRRqABr7NsoGSdgPwM5ZhpcAB8A+6Ag6QOCT3E51zSBdyE/jOFjeWndxSVdQpT4/I5Bc6r2OoHkCXgewq73PrzbdY9YHEa3IQ+T4aOosEmymtadR3lcrOeQ5eeoKLg0DdJ3zrtBmfBH3CaDbWisVPperJZz2hMPm4a5czLrMh6O0hQ2e5S/g1NB9l6NqjeVk0bSZZsaimozbqYWR6QBozrmVC4JjRBX+R9SrA+I8WU8i63WRcBIlwKsvOz6VX7ANyGLJvEITUP0hBFOp/F8CopcDgqcmTkWnSGCXy/gHe8wYDNerIkWUSsxiS5Wpm+obyUG8R9BXzS1hRyUaxCsQ35F/g78if4nYrsE8zAZ1kHdNzaGyyAn+ApT9IeURk9hStwDRwpTcTj+HwgueA70TWNFb3K4yyPANOFTO8ayAZ5BTl7wO35KyJGVPW3wOcsr1PNnELzfZTCzK19YWIt6TtwEnwNTtWmIIe8mYvg+V4B6pkcJD3L8lPQomzdlUzhWSg8bBIFnN5m2tXgH7kUsMth+yf6AzCQUCfORdKgAAAAAElFTkSuQmCC>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAYCAYAAADtaU2/AAACqElEQVR4Xo1VO2tWQRDdLQQN8YGCoiB+Nj4KRURBbQQJGLCysBNrjQEbQb5WK9OIpLKSgGhQURETbEQLRUXUQrAWmxQp8wPiOTu7e2d2935+B86d3Xns7OzrOjcGfKPVj+ijXccJ+19AQ+X6tBZ6Qr43zyTaG5NTu2JbWRqvaG8BN4i2BTvhI+A9J0EVdIJOodtmQsfBebSbY+ngPeBT8FDWdNgBPgdXwPXIv4j9hgGoW0Gb9oMqhrgE3oWPqtxU6tm7Dd7sVEp0nymINchhUf1mcAH8DR7NWm6Zcw/heEHpIiT4MPghyh6E/b6BBhL7KTNvwSy4Dv0d6WaPi+Arl7fPm6IZxBlzhkqvD1ewLUDxA3LQ8Bk62YKhDgIG8PkIeUq6XWIZUKpJpgzV3g1+BhehnSh8WM1r8A94MmmDj3cTLsSk8bvIXeAX8HwyxID0STgLrrpQmdGzcz3arsZ+ifueJ7ywHQN/ei6FTlaHhz0EH4NzMM9RwvEt2stonwHDKRWYAWbRewY5qZVM/DXKPqTt4Kk97WSVErcqvxqSn5NeArdpU5e4rjIB++u5vy/gw6szGnrRYmKIT5A7kwvRrthOIu1vvCqluVDURlbcLXWc0QCf746PQz/C/YX/tHTtyHUeIwhetUUnJzyDz+E78Ip0q3XiD+MR5C/wgBgtqsQWNM97rpZxlNvDH4NdRu+24/vEy5vcvc9yLTYl3zHAA/XG5dWy4LNGozl1RFVNQ1GpArKeD8p7tPcZcwSrewmea22QUTWyNFxTi59bkfplMpiG7oGL77WgdhJY/Qiv/U6e2L1J0fLmjGYgZ3yeXcuthq5S+fMfzLMTfok94+QsdL4GnigsBbS2d4aXXdi6EoVjzyoIKn0jWdWugkagz7cxUOVaKTTsRP8BXSVUZFS0FMAAAAAASUVORK5CYII=>

[image5]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABoAAAAYCAYAAADkgu3FAAACnUlEQVR4XoVVvWvUQRCdRQMajiBKDApitIqFEMSAoCCIYCBVIHZirwRshHBgo1aCIGIh2gTBIsRCCwUb0UJRERV7hXQW/hH63s5+zNzuxQezH+/N7M7N7m9PggCxsWiI/6If4Vg7CdsHNGLy90tsjxF9AGLX+A0S1fBhCtREV+xwx2H3YFM6deIANg1qR2F87AnYAymxCc5HJwdhmxjPWQniGchbGPyFbWA8mTNsf5RcRHun/rIq2NEtdNctb3yZxBcQw+4GCeBRclmHLbX7KHMM9i717UJBzqL5jdFiDjKx2Sd3y+heiC9h8VzF+IloRiOIPtDlG2y2k0Rs6sZhFs172KnkkYW4ODe5VgUH3kDqz2D7MD6N/gJsr3crmIRtSGe9Gdgn0eAeDsA+wl6L1n8Fm91Gjt8xnvP1K7P7ojfQifOiQfpTraSlwPnIH4zvYpxLy5ifwuQa/wiWmhUYWJlBn+E0P5pdAoN+wI6a82BSW7Dzxs8gMOYVbI9OldWN2KeFjJjPr2an/GpIlyPOCkqivFwf0O+3UtoosM/lyiH5fIY6jTJfCGwcHqPfmfmKGJlKF+ibuZjVV+mXIZUoLFYqLCDsl+j3Mg27Cdtd5dgOg9483sACXtk36C9nohQglyi4EpVvSvQGrhiNsQzHjYs3My8WS8SGD6kKFXw8H8KeCs+qinw8eaYs3Q3wEyTNorwAL4WvSAfLcKKot6SCNa6vRd2MHD7YMJJbxAL4t+gPjwoEv/LnsHOFsUuYdA3RmQXuvAZibUwSRDzwR9K8d8k/bdZEe+KI6CU45Fi3hnZXk5kky8ij4QPPime9FGderEgCna/ATjp+bBRRxEsYa+l7/pbr6R7eY1wSthw67jhFJN4GVPg/vWaJhhD5B6WASqqe21XLAAAAAElFTkSuQmCC>

[image6]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA8AAAAYCAYAAAAlBadpAAABl0lEQVR4XoVUu0pDQRCdwStYaKMQ/0Cw005BMJ0pUoqNaB1UxMLG2kbQD5D8giCCtVgFUgkSsNJGhLSCH6Bndjc7Mzd78cA89sxjd+4ul0jAWWVTRi1Yzm1qxG4fg0KgyS8TNdgDlA6jgbDzFpwXyA/kF4TYV8gn/DHsI3K68Gcm9b51JG6w+oKzYSJScAAZgzgOtJZkfwFyDxmAaNVylqGGIAbwW4nLSswK9AirPvzKF9M65APyBH5JaUUHAZn1UPtNwOcs34Op58eUUDSXZOZN/CzUPtwR5IhknaFN5uHfkXxdogcIfH6muNsV7KL9RsaG2cK88PuwVYqLPqVwVdy2+RHaDvOGu+1lhkJ4DeodrlyhC+RilnnZzKvBXZJHQ3wRMxPMDPF+3R1myEeU4hN3YoNVcG8IYF6uTFxe1i2FYorFRGfI2ZYmm1BDLL5TAmZmec8dc+wuybMkliZ7oK5h5+yxnVE/8zLKDlZtksI6NNe/eQu/quOfYodyqOmvUsr2s6mfCHuKUrli6tgWdoMSpuJTRMYfJ084J4gZLbUAAAAASUVORK5CYII=>

[image7]: <data:image/png;base64,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>

[image8]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACEAAAAYCAYAAAB0kZQKAAAC2UlEQVR4Xr1WO4tVMRCeARURuYgiF62ujS8QVrHQShBBwWrB2toX2KggdorNgqyNhbe7CsvigopotwvbiCuiVjZiJYKFjeAP0G8yeWdyFy384DuTMzNJJpnJySEmwD3+L4op00svklzv26X493ZQNFOrYiu4udU33gZMn43goFZOC+IQnvNkdeqC2xjLgREE34Y8207YYjf4BNzv3nwHP8EOPJ+CP8Hfnl/B+6S7dgv8DEe1MX2HXAYPSndgO/gIPCIvvXRIU6K9Vs6ew634FBq/wDu11Qe6Ap83aO+qzcBJcIE03SYOgKtetkgB3SQN4kzUJMzA8QvkGP4bRFGtYwDFc8hZ99bkkOgKOIEuFWTupEJsE/ADOErdY+scaZrON5bQZreIMeiCzBEGv1poi/kdRqQBLFGxpTFaFDR/gzyWbMEcgXTyGmn9Kbx9CK6hfToaorEY4QT4A7wLDln7BUoxr5LWhNQGWasglzJ6B9Xh2iiGj5StIO+XtbUemB5DzkE/J9JzAXqkgscct9of3RysC6a44DKIt14WhgxIGU/I10NlEzT1IDBGGvp0iH8BFwTHIEyM0FkCWAS3iCJNwNKUD5yrh2Zir3DC7wTaFzIPBzla2U6YkIKSoykpoWqN28BX4AqFeuhHktJR+YxgfU/6Ieqh+j4UI8zgTb8PxtGrMALNucKXzuezKSg5jnIse/Ug/aQe3BY3m1BCTsVrcG/UZOmaZ/kUlyPsJL0v5I4I9wXuB7oHbgJvwP+T6FltuFd4Ge19YQADUpBLLAsLczmpj1mIl6T5zYzrtHuwfSRVD8jdT5R8UhDulntGeslkaFJTdOoYTRXEHogXpNJydZNJ0T2k6ocmn7PB1IAKyDm+DnlZ2rUxhxgvefYdjTlTLGW39MbHWU/PQLQ9/wD5FbsIHp3mVGMdL/m3kCP+F39r7Ro7bxYMD0OVYBktXYZ8c6ZuVO5TtWrjHyi4ZOczjMP9AAAAAElFTkSuQmCC>

[image9]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAmwAAAAiCAYAAADiWIUQAAAEzUlEQVR4Xu3c2+s1UxzH8e+TQ07JIUTKk5wPhSgkXLgQuVByI+6UIpRDEUkOxY3kLOfE86AULlyIG0KKa0U9N/Jn8P3stcZv7Xnm2fbsPYe1Zt6v+vabPeu392/NzJo1399aM9sMyNqu+gqgQ7SvOeKoo3M0KpSAdorh0NrmgKOMwZXe6EqvPwBMCX0yUCrOXuCAVp0eq8oAdIwTDgDmjKsACkcT3sDUd9rUt2+uOK7zxHEHNsO5gz7RvnrCjgWwBroKAKhb7hnpJwEAAABgHvj/D8BACu9uCq9+/tjBABboDIBZowvoGDsUAAAgAyRlaIcWAwyDcw1A3uilADS5yuMfjyPqBZk52+Mnjx/qBQVRP3ytx/fx9Ys7RQuXeBzrcbDHzR6nLRcDAIA5+7u+IlNnedxTX1mYFzxu8TjX47O47laPQzy+rX7J/ZosD4T/63PC0RjaJPZ4jxvR40djXBzaooyQHGzkeo/L6ysL87HHRRYSz0c8rkzK9safOn3qo2+d4vwEsCW6EWBgJ3t8bWHU5ymPc5aLs3GUx6ceH3rc7/HHcvHovrAwjam6HVYrSx2eLDdNQ2s75dClteuiCwUAjI+rUQ800nN6XNbo1XVJmbzh8WXyWknJOh73+Ku+cgsaXdsXl3fb8qjg0TGaaKrxlLicJkFXJ8upVfXW33intk7J2bsW7jc70eNZo6GupcSdVGKdZ6W4A1RchVHHIcRAlGy8bzujOkredv9XGrxsYdSocnuyvMrxHp/UV7ZROw80+vdjXE6TN7nAdrahTsnaSXH5lWS9piObrKq3/s5vDet+9/jZ4zEL053AKKpzhmtI/jhGANrQiNq+uHyZx58eJ9jOtN0zHh953OVxpMdDFkaZlLQ953Gfhace5SCPtyzcQK+nHO+wkGQ10WfdtCKaaESteuDgF48HPZ70uMHjg7heyaXe/5ot6rnoEqsRsdssJFvnW0jgHo7r29S7ycUW9ltKnzl9XHFa6Hdn9fvp2BgHph32F3BASmx0T5joKyWqUaKKkpFj4rKmTS+Nr8/w+Dyu13SgRuq+iq+rZEejVHqqsyvf7QpTjvKNn9nvWUgeT7WQsFXJlp7C1GjbNRa+IkOvReU3xp/altctJKld1PtCC1PHuo9NSSSADXHNRv5m0UpnsZElUWKT3iCv5fQg6UGE9LVGtUTTfnraUWVKkuQlC59XjXbpKyuqZK8L6ZSn7kvT1KUo6VJdJP3KjKq8Siz1ukrEqu8504hdV/XWyOQ8RtZQNHph9Iwmhpkat+m/nSwf57HH4wqP5y0kPLo/7AEL93G9aWGaVL+jxEjTk3cv3tmp/XbIEx5PxwIlXRp10xTunRYSu1ctJKJ6+vXe+J4zLUyJnmeD1Rsl26/VYS3st5yUfDRKrjvQLyVn+qqPR+sFALrDZQgARjGZ7ldThppa1MMBAIAlk+nrAQDAurj8A8D/oKOcqC0P7JZvB4DpooPEEGhnXWAvAgDQyviXzpY1aPnrwCrNzal5LTAqmiVmZ9tGv+37R5f3BuRdu/yx/wDMGX0gAAAYD5kIMIBJn2iT3jigG5wmwMRxkk8OhxToznTOp+lsCQDMV2Z9eWbVGQX7AEOgnQHr4VwBMCg6nQHltrNzqw+ASfgXPbZp2Ypa3mIAAAAASUVORK5CYII=>

[image10]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAwAAAAXCAYAAAA/ZK6/AAABL0lEQVR4XoWPIU8DQRCF50KbVBQEJNSQFIVuHYIEQkgq6lFQjSAEQQ0/AQ0Gg8CgEEg0ggRZUQOuP6Tf7c3dTXe2xySz7817M7O7Inlk4SwhHaVZNyWmUhvsgBtu5Kl1TooE52sEfeUP9vkJvib+6fSS/bQNu6jGTdgZfE/VNnwIGZG78aVdigdwSv7Az8EX8pK8Jef0HhetxdQpOSEH5C/aG7hVNEiPpm/wWuswcwEcwMbwBfzImLk+Q7+yA3nwB3knnxFaarXgT+CH1DdWA24TvC/5n0TuyA7CPep26Y8pFjQdFhvCMQL+4EOqE+qb0Kk+W7JPmG4I6j75BX8FH8U+C7/D0a2F6qlt2A64Yb3KddxJWVTEoe9t7FmJZl9dt9VGpLqmlGA1XR6Ode9eAv2vGhfYS4KgAAAAAElFTkSuQmCC>

[image11]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAwAAAAZCAYAAAAFbs/PAAABQElEQVR4XpWQu0oEQRBFq9EFI4P1heIiGJibCWKkYKpgbLCRgYH7G6JsZKAgguwXiKGBIJgYGgmmfoEfoKf6MdPb3TMwBXfq1q3ntEhXMymzLuaJZQ1ZVGqOp9ZCpWfNjdc0Ds+E6W21lGzoaIX9mVTesAwOkXbxc2ky7l7hcwfuwTG4AB/o275gFWyGfxnweYFfEvV8wSz6DX5idJORETU7LiFyJXaabLjaau85+CLaRxqjz3dvINhC/CTQ+7XZNviWE/g3foxw5CReBPyBU/8/1jwnZzT3INGLHYBfcU853eCG/Rh94nAlfh33BoZeUlXTe+AVbodRuAZfCBWafAe3JK7xT3Sc4RfBI/wZP6FhUL+HyAx8Cd8XvyLoQCf3qkOdxUETD2H8oy21ZWstSk6qw8LKkGy6oHVRcVLKvfAPaEoiYXnbkRwAAAAASUVORK5CYII=>