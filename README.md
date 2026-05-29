# Contribution 1: "Undefined" displayed as metric value for custom-labeled metrics in dashboard table view

**Contribution Number:** 1
**Student:** Krishi Shah
**Issue:** [apache/superset#40432 — "Undefined" is displayed as the metric value for custom-labeled metrics when using the "View as Table" option from a chart in the dashboard view](https://github.com/apache/superset/issues/40432)
**Status:** Phase I Complete

---

## Why I Chose This Issue

I chose this issue because it is a clearly scoped, reproducible frontend bug with a specific trigger condition — custom-labeled metrics rendering as "Undefined" in the dashboard's table view. I've worked with React and JavaScript before, and Apache Superset is a widely used open-source data visualization platform, so a fix here would have real impact on users who depend on accurate data displays in their dashboards. The issue was filed just days ago, has no open pull request, and no assignee, which means I can move on it right away without stepping on anyone's work.

I'm also drawn to it because I want to build stronger intuition for how large-scale React codebases handle data flow between chart configuration and view modes. Understanding why a label that exists in chart edit mode fails to propagate through to the "View as Table" rendering will teach me how Superset's plugin architecture wires together chart metadata, and that knowledge will carry forward to future contributions in the same codebase.

---

## Understanding the Issue

### Problem Description

[In your own words, what's broken or missing?]

### Expected Behavior

[What should happen?]

### Current Behavior

[What actually happens?]

### Affected Components

[Which parts of the codebase are involved?]

---

## Reproduction Process

### Environment Setup

[Notes on setting up your local development environment - challenges you faced, how you solved them]

### Steps to Reproduce

1. [Step 1]
2. [Step 2]
3. [Observed result]

### Reproduction Evidence

- **Commit showing reproduction:** [Link to commit in your fork]
- **Screenshots/logs:** [If applicable]
- **My findings:** [What you discovered during reproduction]

---

## Solution Approach

### Analysis

[Your analysis of the root cause - what's causing the issue?]

### Proposed Solution

[High-level description of your fix approach]

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** [Restate the problem]

**Match:** [What similar patterns/solutions exist in the codebase?]

**Plan:** [Step-by-step implementation plan]
1. [Modify file X to do Y]
2. [Add function Z]
3. [Update tests]

**Implement:** [Link to your branch/commits as you work]

**Review:** [Self-review checklist - does it follow the project's contribution guidelines?]

**Evaluate:** [How will you verify it works?]

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: [Description]
- [ ] Test case 2: [Description]
- [ ] Test case 3: [Description]

### Integration Tests

- [ ] Integration scenario 1
- [ ] Integration scenario 2

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week 1 Progress

[What you built this week, challenges faced, decisions made]

### Week 2 Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
