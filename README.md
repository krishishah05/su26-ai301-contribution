# Contribution 1: Write tests for >12 moon apprentices

**Contribution Number:** 1
**Student:** Krishi Shah
**Issue:** [ClanGenOfficial/clangen#3388 - [CODE] Write tests for >12 moon apprentices](https://github.com/ClanGenOfficial/clangen/issues/3388)
**Status:** Phase I Complete

---

## Why I Chose This Issue

I chose this issue because it targets a specific, documented weak point in the codebase: adult apprentices (cats that are over 12 moons old and still apprentices) have historically caused recurring bugs, especially around mate-related logic. The maintainer explicitly flagged this area as needing test coverage, which means my contribution directly reduces the chance of future regressions. Writing targeted tests is a concrete, bounded task with clear acceptance criteria, and I can make real progress on it within the first week of Phase II.

I also chose it because I want to build stronger habits around test-driven thinking in Python projects. Clangen is a well-organized Python codebase with an active contributor community, and the issue gives me a chance to read through the cat lifecycle logic, understand how game state is structured, and write tests that actually reflect real edge cases. The issue is labeled good first issue, has no prior claims, and the maintainer's description gives enough context to get started without needing additional guidance.

---

## Understanding the Issue

### Problem Description

The codebase currently lacks test coverage for apprentices that are older than 12 moons. This age range is a known source of bugs, particularly in logic involving mates and relationship handling. Without tests, regressions in this area are hard to catch before they reach users.

### Expected Behavior

Tests should verify that cats older than 12 moons who are still apprentices are handled correctly by the relevant game logic, especially code paths involving mates.

### Current Behavior

No tests exist for this edge case, so bugs in adult apprentice handling can go undetected until they surface in gameplay.

### Affected Components

The test suite and any Python modules handling apprentice age checks and mate logic.

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

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [Y] Progress

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
