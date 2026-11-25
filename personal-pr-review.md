---
name: personal-pr-review
description: "Use when reviewing code changes for Declan, including requests like 'review my changes', 'review this PR', or 'take another look after the fixes'. Covers local staged or unstaged changes, branches, commits, pasted diffs, pull requests, re-reviews, and drafting inline review comments. Implementation-only and policy-only questions fall outside this workflow."
---

# Personal PR Review

Review thoroughly, with particular attention to unintended consequences and how easily the next engineer can reason about the code. Report what would help Declan and the author make a decision. A clean review is a valid outcome.

## Establish what is being reviewed

- For a PR, identify the current base and head. Read the description, full diff, existing review discussions, and linked ticket or spec. For a branch or commit, identify the comparison the requester intended. For local changes, respect a specified scope such as staged only. An unqualified working-tree review includes staged edits, unstaged edits, and relevant new files. For a pasted diff, use the surrounding source when available and state any material context gaps.
- Identify the problem, stated requirements, and decisions the team has already accepted. Surface contradictions as questions. Read applicable repository instructions and relevant domain guidance, then trace changed behavior through callers, consumers, shared helpers, configuration, and error handling.
- Inspect the tests and available CI results. Identify which checks exercise the changed behavior and whether any failures come from the application, runner, or CI tooling. Use the repository's runner for focused checks. Use temporary files or an isolated worktree when reproducing a finding would disturb Declan's checkout. Reserve expensive full suites for reviews that require them or where Declan has authorized them.
- For an author update, compare the previously reviewed snapshot with the current one and read the author's replies. Recheck affected behavior in the full current context, including risks introduced by the fix. Account for each earlier finding as addressed, outstanding, superseded, or no longer applicable. If the earlier snapshot is unavailable, say what the current code establishes.

## Examine the change

Apply these questions where the diff gives them a concrete bearing on the review.

| Lens | Questions that guide the review |
| --- | --- |
| Correctness and value | Does the change solve the stated problem through the normal entry points? What user or engineering benefit justifies the complexity and ownership cost? Could an existing capability or a simpler design meet the same requirement? |
| Edge cases and unintended consequences | Which callers see different behavior for empty, default, invalid, or boundary inputs? What happens under concurrency, retries, duplicate delivery, partial failure, or different permissions and configurations? Trace reachable paths to silent loss, incomplete results, or changed failure semantics. |
| Maintainability and readability | Do the patterns and abstractions fit their callers? Examine ownership, coupling, duplication, and API contracts. For refactors, check that behavior is preserved and the result is easier to follow. Raise style issues when an explicit convention or a concrete readability improvement supports them. |
| Compatibility, rollout, and recovery | Can old and new instances coexist? Consider existing clients and stored data, migration order, feature flags, rollback, and any customer communication a breaking change requires. Identify which outcomes depend on deployment, rollout, or product acceptance. |
| Performance and cost | At representative and worst supported workloads, trace query counts and bounds, index use, hotspots, fan-out, retry amplification, blocking I/O, caching, payload size, memory, CPU, and service cost as relevant. State the workload and scale behind a concern. |
| Security and privacy | Trace authorization, tenant isolation, input handling, and sensitive data across the trust boundaries the diff changes. Use source inspection, caller tracing, and existing or ordinary functional regression tests. Identify the specific security guarantee at stake and any unverified assumptions. |
| Observability and verification | Which test would fail if the change regressed? Does it preserve the production assumptions that matter? Review the tests themselves for correctness, reliability, and clarity. Suggest telemetry only for a concrete question about failure, rollout, or outcome, accounting for existing signals, privacy, cardinality, and cost. |

## Decide what merits a finding

- Establish the triggering conditions, expected behavior, actual behavior, and consequence. Trace surrounding safeguards and cite the code, requirement, focused reproduction, or authoritative documentation that supports the claim. Assess the proposal on its evidence and avoid attributing motives to the author.
- Explain what a synthetic or boundary test establishes and which assumptions remain about inputs, configuration, reachability, frequency, or production impact. A supported edge case can warrant a finding before an incident occurs.
- Assess confidence in the observed mechanism and in the intended contract separately. When the behavior is clear but the contract is unsettled, ask a design question. Prioritize confirmed defects by impact and likelihood. Label optional clarity or documentation improvements as nits and omit low-value preferences.
- Verify claims about standards against a primary source. Identify whether the basis is a normative requirement, vendor guidance, repository policy, or engineering judgment.
- If Declan challenges a finding, revisit the code and its assumptions. Explain which facts, expectations, or priorities changed, and retain or revise the finding accordingly.

## Deliver the review

**Start every completed review with a brief, plain-language summary.** Explain the problem, the resulting behavior, and any material work the change defers.

1. Lead the findings with the most consequential defect. For each one, give the triggering scenario, consequence, and a practical direction for repair or clarification. Anchor it to the smallest useful changed line or range in the reviewed snapshot. Link supporting callers or tests separately when needed. If the review found no actionable defects, say so directly.
2. Group worthwhile non-blocking questions and nits separately. On a re-review, make the disposition of earlier findings easy to scan.
3. Identify the reviewed commit or local working-tree scope. State which checks actually ran, their results, and any material limitations. Give an overall judgment that the evidence supports. Verify approval status, CI completion, and merge readiness individually whenever making claims about them.

When asked for inline comments, draft concise, friendly text with an exact anchor for each comment. State a confirmed defect directly, explain its effect, and suggest a possible fix. Ask a focused question when intent is unresolved. For a tone-only rewrite, revise the existing comment directly.

## Keep review actions within scope

A review request covers inspection and appropriate checks. Editing source, posting comments, submitting an approval, changing tickets, pushing, merging, or deploying each requires authorization for that action. Honor authorization already given. Immediately before an authorized approval, confirm the live PR head still matches the reviewed commit and use any message Declan specified exactly.

For organization approval or merge-readiness advice, consult the current repository review policy and PR requirements, including required human reviews. An agent review does not satisfy an independent human-review requirement.
