---
name: pr-description
description: Use when creating a pull request or drafting/updating its description. Do not use for ordinary coding, commits, or code review.
---

# Pull request descriptions

1. Read `assets/pull_request_template.md` bundled with this skill.
2. Review the full branch diff against the target branch, along with relevant requirements, issue context, and decisions from the current coding session.
3. Write an explanatory description using the template: cover **why** the change is needed, **what** changed, and **how** the important implementation works. Group related changes instead of listing files or commits.
4. Match the level of detail to the PR's complexity. Remove template comments, placeholders, and inapplicable optional sections.
5. State only supported facts. Never invent root causes, design decisions, test results, or performance claims. Clearly distinguish tests added from tests actually run and mention meaningful verification gaps.
6. Use the completed description as the PR body. When updating an existing PR, preserve unrelated author-written content unless explicitly asked to replace it.
