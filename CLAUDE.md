# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## About This Repository

This is the documentation repository for Mechanic, a Shopify development and automation platform. The documentation is structured in GitBook format and covers:

- **Core Concepts**: Events, Tasks, Actions, Runs, and Shopify integration
- **Platform**: Liquid templating, GraphQL, integrations, caching, email
- **Resources**: Task library, tutorials, video walkthroughs
- **Techniques**: Best practices for task development
- **FAQ**: Common questions and troubleshooting

The documentation is published and rendered through GitBook, with markdown files containing GitBook-specific syntax like `{% hint %}`, `{% tabs %}`, and `{% endtabs %}`.

## Commands

### Git Operations

```bash
git status                          # Check repository status
git add .                          # Stage all changes
git commit -m "message"            # Commit with message
git push                           # Push to remote
```

### Viewing Documentation

The documentation is served through GitBook. Files are written in markdown with GitBook extensions.

## Architecture Overview

### Content Structure

```text
├── core/                          # Core concepts documentation
│   ├── actions/                   # Action types and integrations
│   ├── events/                    # Event system
│   ├── runs/                      # Run queue and execution
│   ├── shopify/                   # Shopify API integration
│   └── tasks/                     # Task system
├── platform/                      # Platform features
│   ├── cache/                     # Caching system
│   ├── email/                     # Email functionality
│   ├── graphql/                   # GraphQL documentation
│   ├── liquid/                    # Liquid templating
│   └── integrations/              # Third-party integrations
├── techniques/                    # Development patterns
├── resources/                     # Task library, tutorials
├── faq/                          # Frequently asked questions
├── .gitbook/assets/              # Images and media files
├── SUMMARY.md                    # GitBook table of contents
└── README.md                     # Introduction page
```

### Key Concepts

#### Execution Model

Mechanic follows a hierarchical execution model:

```text
Trigger → Event → EventRun → TaskRun(s) → ActionRun(s)
```

* **Events**: Represent anything that happens (Shopify webhooks, schedules, custom events)
* **Tasks**: Liquid scripts that respond to events via subscriptions
* **Actions**: Side effects produced by tasks (Shopify API calls, emails, HTTP requests, files)
* **Runs**: Execution units processed through queues (event runs → task runs → action runs)

#### Liquid Templating

Tasks are written in Liquid with Mechanic-specific extensions:

* Custom tags: `{% action %}`, `{% log %}`, `{% error %}`, `{% permissions %}`
* Custom filters: `shopify`, `graphql_arguments`, `parse_json`, `csv`, `hmac_sha256`
* Keyword literals: `hash`, `array`, `newline`
* Mechanic objects: `event`, `task`, `options`, `cache`, `action`

#### Action System

Actions are defined using JSON or the `{% action %}` tag:

```liquid
{% action "shopify" %}
  mutation {
    customerCreate(input: { email: "test@example.com" }) {
      customer { id }
      userErrors { field message }
    }
  }
{% endaction %}
```

Common action types:

* `shopify`: Interact with Shopify Admin API (GraphQL or REST)
* `http`: Call external APIs
* `email`: Send emails with templates and attachments
* `files`: Generate PDFs, CSVs, ZIPs
* `ftp`: Upload files to FTP/SFTP servers
* `cache`: Store/retrieve data across task runs
* `event`: Trigger custom events

#### Responding to Action Results

Tasks can subscribe to `mechanic/actions/perform` to handle action results asynchronously. Use `__perform_event: false` to skip generating follow-up events for specific actions.

## GitBook Formatting Conventions

### Hints

```markdown
{% hint style="info" %}
Informational message
{% endhint %}

{% hint style="warning" %}
Warning message
{% endhint %}
```

### Tabs

```markdown
{% tabs %}
{% tab title="Liquid" %}
Code example
{% endtab %}

{% tab title="JSON" %}
Output example
{% endtab %}
{% endtabs %}
```

### Code Blocks

Always add spacing around code blocks:

```markdown
{% tab title="Example" %}
(blank line)
```liquid
code here
```
(blank line)
{% endtab %}
```

This spacing is critical for GitBook rendering. Recent commits show ongoing maintenance to fix formatting issues.

## Documentation Patterns

### File Naming

* Use kebab-case: `working-with-external-apis.md`
* READMEs serve as index pages for directories
* Keep URLs stable (files are referenced from external sources)

### Content Organization

* Start with high-level concepts before details
* Use examples extensively (Liquid + JSON output in tabs)
* Cross-reference related documentation with relative links
* Include "Learn more" sections with links to deeper content

### Common Sections

* **Description**: What the feature/concept is
* **Usage**: How to use it (with examples)
* **Options/Parameters**: Available configuration
* **Examples**: Real-world use cases
* **Related**: Links to related documentation

## Important Constraints

1. **No Build Process**: This is a pure documentation repository with no build tools or dependencies. Changes are published directly through GitBook.

2. **GitBook Integration**: The repository is connected to GitBook, which manages publishing. Do not modify `.gitbook/` directory contents.

3. **SUMMARY.md**: This file defines the table of contents for GitBook. Update it when adding new documentation files.

4. **External References**: This documentation is referenced by:
   - The Mechanic web application (docs.mechanic.dev)
   - The task library (tasks.mechanic.dev)
   - Community resources
   - API documentation

   Avoid breaking links or moving files without redirects.

5. **Shopify API Versions**: Document Shopify API patterns that work across versions. The platform supports multiple API versions simultaneously.

## Writing Guidelines

### Git Commits

When creating commits in this repository:

* Use clear, descriptive commit messages
* Do not include Claude Code attribution or co-author tags
* Focus on what changed and why

### For New Documentation

1. Follow existing file structure and naming patterns
2. Use GitBook formatting (hints, tabs) consistently
3. Include practical examples with both input and output
4. Cross-reference related concepts
5. Update SUMMARY.md to include new files
6. Add spacing around code blocks for GitBook compatibility

### For Updates

1. Preserve existing URLs and file paths
2. Maintain GitBook formatting
3. Test that tabs and hints render correctly
4. Check for broken internal links
5. Verify code examples are accurate

### Code Examples

* Use Liquid for task code examples
* Show both code and expected output in tabs
* Include error cases and edge cases
* Demonstrate Mechanic-specific filters and tags
* Reference Shopify GraphQL API patterns

## Common Tasks

### Adding New Documentation

1. Create markdown file in appropriate directory
2. Add entry to SUMMARY.md in correct section
3. Use GitBook formatting conventions
4. Include cross-references to related docs
5. Test rendering by checking spacing around code blocks

### Updating Shopify API Examples

1. Verify API version compatibility
2. Test GraphQL queries in Shopify GraphiQL explorer
3. Show both query and response in tabs
4. Document any required permissions
5. Link to core/shopify/api-versions.md if version-specific

### Fixing Formatting Issues

Recent commits show common formatting fixes:

* Add spacing around code blocks in tabs
* Normalize tab structures for GitBook
* Fix Liquid syntax highlighting
* Ensure code fences are properly closed
