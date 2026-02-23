---
name: authoring-api-docs
description: "Use when producing API reference documentation — REST endpoints, SDK/library references, CLI command references, or documentation generated from OpenAPI/Swagger specs. Triggers: 'document this API', 'generate API reference', 'write SDK docs', 'document these endpoints', any task involving source code with HTTP handlers, route definitions, or OpenAPI specs. Always load authoring-technical-docs first."
---

# Authoring API Docs Action

Produces precise, exhaustive API reference documentation — the information-oriented quadrant of the Diátaxis framework.

**Load `authoring-technical-docs` first** for the multi-pass workflow, style rules, and quality framework. This action provides the templates and API-specific rules.

---

## Templates

Templates are located in the `assets/` folder alongside this skill:

- **`assets/rest-endpoint-template.md`** — Use for documenting individual REST API endpoints. Covers authentication, path/query/body parameters, request and response examples, error codes, and rate limits.
- **`assets/sdk-function-template.md`** — Use for documenting SDK or library functions. Covers parameters, options, return values, errors, and a usage example.

Copy the relevant template and fill in all placeholder fields before publishing.

---

## API-specific rules

1. **Every parameter must have a type.** No exceptions. Complex types link to schema definitions.
2. **Every endpoint must have a working example.** Copy-pasteable with clearly marked placeholders (`YOUR_API_KEY`).
3. **Document all error codes.** Include the code string, description, and resolution.
4. **Show both request and response.** Always pair them.
5. **Use consistent field naming.** Match the API's convention exactly — never translate.
6. **Document authentication once at the top**, then reference from individual endpoints.
7. **Include rate limit information** for every rate-limited endpoint.
8. **Version your docs** if the API has versions.

---

## Generating from OpenAPI specs

When an OpenAPI/Swagger spec is the input:

1. Parse all paths, methods, parameters, and schemas
2. Group endpoints by tag or resource
3. Populate the endpoint template for each
4. Create a "Data types" section for schemas referenced by `$ref`
5. Use `description` fields as starting points — rewrite for clarity
6. Use `example` fields in code examples
7. Document `security` section as auth requirements at the top

Save output to `docs/api/`.
